# Rocket



## Request

### 请求守卫

任何实现了fromRequest的方法都是请求守卫

->FromParam

```rust
struct MyParam<'r> {
    key: &'r str,
    value: usize
}

use rocket::request::FromParam;

impl<'r> FromParam<'r> for MyParam<'r> {
    type Error = &'r str;

    fn from_param(param: &'r str) -> Result<Self, Self::Error> {
        // We can convert `param` into a `str` since we'll check every
        // character for safety later.
        let (key, val_str) = match param.find(':') {
            Some(i) if i > 0 => (&param[..i], &param[(i + 1)..]),
            _ => return Err(param)
        };

        if !key.chars().all(|c| c.is_ascii_alphabetic()) {
            return Err(param);
        }

        val_str.parse()
            .map(|value| MyParam { key, value })
            .map_err(|_| param)
    }
}

#[get("/<key_val>")]
fn hello(key_val: MyParam) -> String {
    ...
}
```

->FromRequest

```rust
use rocket::http::Status;
use rocket::request::{self, Outcome, Request, FromRequest};

struct ApiKey<'r>(&'r str);

#[derive(Debug)]
enum ApiKeyError {
    Missing,
    Invalid,
}

#[rocket::async_trait]
impl<'r> FromRequest<'r> for ApiKey<'r> {
    type Error = ApiKeyError;

    async fn from_request(req: &'r Request<'_>) -> Outcome<Self, Self::Error> {
        /// Returns true if `key` is a valid API key string.
        fn is_valid(key: &str) -> bool {
            key == "valid_api_key"
        }

        match req.headers().get_one("x-api-key") {
            None => Outcome::Error((Status::BadRequest, ApiKeyError::Missing)),
            Some(key) if is_valid(key) => Outcome::Success(ApiKey(key)),
            Some(_) => Outcome::Error((Status::BadRequest, ApiKeyError::Invalid)),
        }
    }
}

#[get("/sensitive")]
fn sensitive(key: ApiKey<'_>) -> &'static str {
    "Sensitive data."
}

#[rocket::async_trait]
impl<'r> FromRequest<'r> for User {
    type Error = ();

    async fn from_request(request: &'r Request<'_>) -> Outcome<User, ()> {
        let db = try_outcome!(request.guard::<Database>().await);
        request.cookies()
            .get_private("user_id")
            .and_then(|cookie| cookie.value().parse().ok())
            .and_then(|id| db.get_user(id).ok())
            .or_forward(Status::Unauthorized)
    }
}

#[rocket::async_trait]
impl<'r> FromRequest<'r> for Admin {
    type Error = ();

    async fn from_request(request: &'r Request<'_>) -> Outcome<Admin, ()> {
        // This will unconditionally query the database!
        let user = try_outcome!(request.guard::<User>().await);
        if user.is_admin {
            Outcome::Success(Admin { user })
        } else {
            Outcome::Forward(Status::Unauthorized)//forward表示继续向下执行
        }
    }
}

#[get("/dashboard")]
fn admin_dashboard(admin: Admin) { }

#[get("/dashboard", rank = 2)]
fn user_dashboard(user: User) { }
```

想要直接从请求中得到数据,可以用FromData,对于常用的json,可以

```rust

#[derive(Deserialize)]
#[serde(crate = "rocket::serde")]
struct Task<'r> {
    description: &'r str,
    complete: bool
}

#[post("/todo", data = "<task>")]
fn new(task: Json<Task<'_>>) { /* .. */ }
```



最简单的方法如图:

```rust
use rocket::serde::json::{json, Json, Value};
use rocket::serde::Deserialize;
use serde::Serialize;

#[macro_use]
extern crate rocket;

#[derive(Deserialize, Serialize)]//反序列化
struct User<'r> {
    name: &'r str,
    age: u8,
}

#[post("/user", format = "json", data = "<user>")]
fn new(user: Json<User<'_>>) -> Value {
    json!({ "status": "ok", "user_name":user.name})
}

#[launch]
fn rocket() -> _ {
    rocket::build().mount("/", routes![new])
}

```

