##### **保存时自动移除未使用的类引用**

Windows——Preferences——Java——Editor——Save Actions

然后选择Perform the selected action on save，再勾选Organize imports即可



##### MySQL需要给一个表添加一个列"id"并从1自动增长到末尾

ALTER TABLE author ADD id INT(4) NOT NULL
    PRIMARY KEY AUTO_INCREMENT FIRST;



##### substring返回一个新字符串

它是此字符串的一个子字符串。该子字符串从指定的 `beginIndex` 处开始，一直到索引 `endIndex - 1` 处的字符

```
public  substring(int beginIndex，int endIndex)
C++中
string a="12345asdf".substr(0,5);       //获得字符串s中 从第0位开始的长度为5的字符串//默认时的长度为从开始位置到尾 12345
```



##### eclipse中删除多余的tomcat server

在eclipse菜单中选择Window--Show View--Server--Servers，打开这个服务窗口，将多余的服务删除即可。



##### java数组为参数传递的是值

```
int []A= {1,2,3,4};
reverse(A[2]);//正变负
System.out.println(Arrays.toString(A));
//[1, 2, 3, 4]
```

##### 判断并修改

```
A[a+1][b]=(A[a+1][b]==0)?1:0;
```

##### java转多进制

```
System.out.println(a.toString(8));
```

