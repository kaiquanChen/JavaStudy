# 1.Java中创建对象的五中不同方法

## 1.1.使用new关键字
>
    Student std = new Student();

## 1.2.使用class类的newInstance方法
>
    Student s = Student.class.newInstance();
    
或者

>
    Student std = Class.forName("com.fly.Student");
    
## 1.3.使用构造方法类的newInstance方法
>
    // NoArgsConstructor
    Constructor<Person> constructor = Person.class.getConstructor();
    Person person = constructor.newInstance();

    // AllArgsConstructor
    Constructor<Person> cons = Person.class.getConstructor(String.class, int.class);
    Person david = cons.newInstance("david", 11);
    System.out.println(david);
    
Spring Hibernate Structs等框架使用的就是这种创建对象的方式.
    
## 1.4.使用clone方法
    Java虚拟机创建一个新的对象,并将之前对象的内容复制到这个新对象中,clone()不会调用任何构造方法.
    
>
    Person person = Person.class.newInstance();
    person.setAge(18);
    person.setName("david");
    System.out.println(person);

    Person david = (Person) person.clone();
    System.out.println(david);
    
pojo要实现Cloneable接口并重写clone(),才能使用clone();

## 1.5.使用反序列化
>
    Person lucy = new Person("lucy", 18);
    Person david = new Person("david", 11);
    // searialization
    ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("data.obj"));
    out.writeObject(david);
    out.writeObject(lucy);
    out.close();

    // desearialization
    ObjectInputStream in = new ObjectInputStream(new FileInputStream("data.obj"));
    lucy = (Person) in.readObject();
    in.close();

    System.out.println(lucy)
    
# 2. Java序列化深入学习

## 2.1.什么是序列化和反序列化:

- Java序列化是指把java对象保存为二进制字节码的过程.
- Java反序列化是指把二进制码重新转化成Java对象的过程.

## 2.2.什么时候需要序列化
- 第一种情况是,Java对象的声明周期要比Java虚拟机的周期要短,而我们希望有时候JVM停止运行后能够持久化指定的Java对象

- 第二种情况是,需要将Java对象进行网络传输,而网络传输只能以二进制的形式进行传输,所以发送之前要先序列化成二进制数据,
在接收端再反序列化得到Java对象

## 2.3.怎么序列化以及反序列化
- 实体类需要实现Serializable接口

另外:被static和transient字段不能被序列化.

# 3. Java集合
## 3.1.Java7/8中的HashMap和ConcurrentHashMap解析

### 3.1.1.Java7中的HashMap
1. 数据结构为一个数组,数组中的每个元素又是一个单链表

2. put()方法解析
>
    1. 初始化数组大小,并计算扩容的阈值
    2. 添加entry节点,先判断是否需要扩容,然后将新的数据添加到相应位置的头结点
    3. 数组扩容:当size已经达到了阈值,并且要插入的数组位置上已经有元素了,会出发扩容,并且扩容后的数组为原来的2倍,
    扩容的实质就是用一个新的大数组替换原来的小数组,并将原来数组的元素迁移到新的数组中.
    
3. get()方法解析
>
    1. 根据key计算hash值
    2. 找到相应的数组下标:hash & (length - 1)
    3. 遍历该数组位置处的链表,直到找到相同的key
    
## 3.1.2.Java7中的ConcurrentHashMap
1. 数据结构为一个segment数组,该数组不能扩容,默认为16,segment通过继承ReentrantLock来进行分段加锁,每个segment都
是一个或者多个单链表,所谓扩容是对单链表的扩容.

2. 

## 3.1.3.Java8中HashMap
1. 数据结构为数组+链表+红黑树,当链表中的数组元素达到8个以后,会将链表(时间复杂度为O(logN))转化为红黑树,此时查找的时间复杂度为O(logN);
2. 

# 4. 设计Restful API
### 4.1.请求方法
>
    1. GET 用于查询资源
    2. POST 用于创建资源
    3. PUT 用于更新资源
    4. DELETE 用于删除数据
    
### 4.2.Response Code
#### 4.2.1.因客户端原因导致的请求失败:
- 400 Bad Request:如数错误,格式错误
- 401 Unauthoried:用户未被认证过,如密码错误,证书错误
- 403 Forbidden:权限不足
- 404 Not Found:服务器端无此资源.通常为URL不存在或者某个Method不存在
- 409 Confict:请求存在冲突无法吃力该请求

#### 4.2.2.因服务器端导致请求失败:
- 500 Internal Server Error:服务端错误消息
- 501 Not Implemented:服务器不支持当前请求所需要的某个功能
- 503 Service Unaviable:服务器不可用,如服务器维护或者过载等
    
### 4.3.编码
- 从API入口,到业务逻辑处理,再到存储,一律使用UTF-8

在HTTP协议中,
- Request 头部:Accept-Charset=utf-8
- Response 头部:Content-Type:charset=utf-8

### 4.4.序列化
- Request 头部:Accept=application/json
- Response 头部:Content-Type:application/json
    
# 5. HTTP和HTTPS
## 5.1.HTTP
1. 安全性和幂等性:
- 安全性:无论请求多少次,都不会改变资源的状态.如编写GET类的API时,不该出现删除或者修改资源的逻辑,(GET有)
- 幂等性:无论执行一次还是多次,效果是等价的.POST请求是没有幂等性的.

2. 四种请求方式具有的特性  
|Method|安全性|幂等性|是否有request body|是否有response body|  
|:---: |:---:|:---:|:---:|:---:|  
|POST|否|否|是|是|  
|DELETE|否|是|否|最好无|  
|PUT|否|是|是|option|  
|GET|是|是|否|是|  

## 5.2.HTTPS  
### 5.2.1.HTTPS和HTTP的区别:
1. https需要一定的费用
2. http是超文本传输协议,信息是明文传输,https则是具有安全性的ssl加密传输协议
3. http和https使用安全不同的连接方式,用的端口也不一样,前者是80,后者是443
4. http连接简单,无状态;https是由具有可加密性和真实性

### 5.2.2.HTTPS的缺点:
1. 握手比较耗时,增加页面加载时间
2. 缓存不如HTTP,会增加数据开销和功耗

## 6.Java并发编程:volatile关键字学习
### 6.1.并发编程中的三个概念:
1. 原子性:一个操作或者多个操作要么全部执行成功,要么全部执行失败
2. 可见性:共享变量发生变化时,其他线程能够立即看到变化
3. 有序性:程序的执行顺序按照代码的先后顺序执行

- volatile关键字保证可见性,被修饰的变量,会保证修改后的值立即被更新到主存,其他线程会去主存读取

### 6.2.volatile关键字:
具有两层含义:
1. 保证变量的可见性
2. 禁止指令重排序

- volatile不能保证原子性
- volatile能够保证有序性
- volatile能够保证可见性

### 6.3.使用volatile关键字的场景:
1. 对变量的写操作不依赖于当前的值
2. 该变量没有包含在具有其他变量的不变式中

- 保证原子性的操作才能保证使用volatile关键字的程序得到正确执行

## 7.finalize关键字
- 通知gc在该对象可以被回收,但gc并不一定会立即去回收,甚至有可能不会被执行,注重时间的任务不应该由终结方法来完成

