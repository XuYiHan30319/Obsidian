# File IO

[toc]

## 集中标准库用于文件

- ifstream:用于文件输入

- ofstream:用于文件输出

- fstream:用于文件读写

打开文件的步骤如下

1. 首先创建一个fstream对象
2. 打开文件.fout.open("1.txt",ios::app)或者fstream fout("1.txt",ios::app),如果文件不存在就会创建该文件,如果存在就会
3. fout<<"1.cpp";

```c++
char ch;
ifstream fin;
fin>>ch;
char buf[80];
fin>>buf;
fin.getline(buf,80);

string line;
getline(fin,line);
while(fin.get(ch)){
  cout<<ch;
}
fout.flush();
fin.close();
```

可以使用!fin检查是否成功打开

![image-20231214092605644](/Users/blackcat/Documents/北交大软件学院许一涵学习资料/大三上学期/c++/File IO.assets/image-20231214092605644.png)文件打开格式有很多种,可以使用|符号进行多次操作.`ofstream fout("bagels", ios_base::out | ios_base::app);`.

<img src="/Users/blackcat/Documents/北交大软件学院许一涵学习资料/大三上学期/c++/File IO.assets/image-20231214093100135.png" alt="image-20231214093100135" style="zoom:50%;" />

其中,以Binary方式打开文件会有很多的奇怪问题,上面的代码把一个结构体以binary的形式读出来了

## 随机访问

1. seekg() moves the input pointer to a given file location;     
2. seekp() moves the output pointer to a given file location.

```c++
fin.seekg(-1,ios::cur)
```



