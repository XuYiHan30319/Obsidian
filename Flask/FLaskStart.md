post请求有可能会改变服务器端的内容,而g<u>et请求则不会,</u>而且注意,post请求的数据存放在路径中,而get请求的数据存放在request.body中
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def fun():
	return 'helloworld'

if __name__=="__main__":
	app.run()
```

如果想要建立一个可以接受和访问的程序,就需要使用request,jsonify来当做接受和中转
```python
from flask import Flask,jsonify,request
app = Flask(__name__)
app.config["JSON_AS_ASCII"] = False

@app.route('/user/login',methods=['post'])
def login():
	data = request.get_json();
	userName = data.get('userName')
	password = data.get('password')
	return jsonify({'code':0,data:{'token':'666'}})
```

## flask架构的文件如下
```text
yourapp/
    __init__.py
    admin/
        __init__.py
        views.py
        static/
        templates/
    home/
        __init__.py
        views.py
        static/
        templates/
    control_panel/
        __init__.py
        views.py
        static/
        templates/
    models.py
```

## 蓝图
```python
from flask import BluePrint

user_blue = Blueprint('user', __name__, url_prefix='/user')

@user_blue.route('/')
...

#main函数
from user.user import user_blue
app.register_blueprint(user_blue)
```


## flask
首先创建引擎
engine = create_engine("数据库类型+数据库驱动://数据库用户名:数据库密码@IP地址:端口/数据库"，其他参数)
比如使用mysql的时候,我们的engine代码就是
engine = create_engine("mysql+pymysql://root:505066278@localhost::3306/tet",echo=True)
```python
from flask_sqlalchemy import SQLAlchemy
from flask import Flask
app = Flask(__name__)
app.config["SQLALECHEMY_DATABASE_URI"]="mysql+pymysql://..."
db = SQLAlchemy()
db.init_app(app)

class student(db.Model):
	__tablename__='student'
	id = db.column(db.Integer,primary_key=True)
	name = db.column(db.String(64),unique=True,nullable=False)

with app.app_context():
	db.create_all()
	#创建所有表
	student = Student(id=1,name='xyh',gender='男')  
	db.session.add(student)  
	db.session.commit()
	#单个查询
	student =db.session.execute(db.select(Student).order_by(Student.id)).scalar()
```
当我们获得某个user的时候,我们只要直接在python中对他进行修改,那么数据库中的数据自动就会修改,注意,记得使用commit提交






