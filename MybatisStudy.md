## 1.Mybatis基本构成
### 1.1.基本构成图:
![](./images/mybatis基本构成)

### 1.2. SqlSessionFactoryBuilder
- 生命周期很短,只是为了创建SqlSessionFactory对象

### 1.3. SqlSessionFactory
- 是一个产生SqlSession对象的工厂类,生命周期会伴随着整个Mybatis的生命周期

### 1.4. SqlSession
- 类比JDBC的Connection,是线程不安全的,需要手动关闭

### 1.5. Mapper
- 最大生命周期应该和SqlSession相同,一个方法级别的东西

## 2. XML配置标签元素
### 2.1. select
>包含的属性:
>
    1. id标识了这条sql,id与接口名保持一致
    2. parameterType定义参数类型
    3. resultType定义返回值类型, 基本数据返回值类型也可以插入POJO中只要提供Setter方法
    
### 2.2. insert
>包含属性:
>
    1. useGeneratedKeys:是否使用数据库内置主键生成策略
    2. keyProperty: 指定主键
    3. selectKey: 自定义主键生成策略
    
### 2.2. 输入映射
- 通常有三种方式--通过Map,注解或者POJO对象

>1.通过Map:将map的key/value映射到sql对应的占位符中
>
    List<Commodity> getCommodityByMap(Map<String, Object> params);
    
>2.通过注解:
>
    List<Commodity> getCommodityByAnotation(@Param("name") String name, @Param("price") Double price);
    
>3.通过POJO:会将POJO里非null的属性传递到sql对应的占位符中
>
    List<Commodity> getCommodityByPOJO(Commodity commodity);
    
- #和$的区别:
>
    #相当于JDBC中的?,起到预编译的效果,$只是单纯的字符串替换.#方法能够很大程度上的防止sql注入,$方式一般用于传入数据库对象
    
### 2.3. 输出映射
1. resultType:只有当查询出来的列明和pojo中的属性名一致时,才能映射成功

## 3. 配置文件属性设置
### 3.1. autoMappingBehavior:
>
    - NONE: 取消自动映射
    - PARTIAL: 默认值,只会自动映射没有定义嵌套结果集映射的结果集
    - FULL: 会自动映射任意复杂的结果集(无论是否嵌套)