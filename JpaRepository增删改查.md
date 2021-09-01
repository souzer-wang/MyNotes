1. JpaRepository 简单查询

可以分为两种：
- 一种是 Spring data 默认已经实现
- 另一种是 根据查询的方法来自动解析成 SQL

		一些默认的，比如说：
		- findAll()
		- findOne(~)
		- save(~)
		- delete(~)
		- count()
		- exists()


		根据查询方法解析成 SQL 的：

		- find~By~
		- read~By~
		- query~By~
		- count~By~
		- get~By~

具体的可以参考：[SpringDataJpa：JpaRepository增删改查](https://blog.csdn.net/fly910905/article/details/78557110/)

关于查询方法的内容，根据 Spring Data 的规范：
- 查询方法以 find, read, get 开头
- 查询条件中，条件属性 用 **条件关键字 连接**，且 条件属性以**首字母大写**

举例： `findAddressByNameAndAge(......)`

2. 使用 JPA 的 NamedQueries

具体步骤：

> 1. 在实体类上使用 @NamedQuery，如下：
> ```java
> @NamedQuery(name = "UserModel.findByAge", query = " select o from UserModel o where o.age >= ?")
> ```
> 2. 在自己实现的 DAO 的 Repository 接口里面定义一个同名的方法：public List\<UserModel> findByAge(int age);
> 
> 3. 然后使用的时候，Spring 会查找是否有同名的 NamedQuery

3. 使用 @Query 来进行本地查询

需要设置 nativeQuery 为 true

举例：

```java
@Query(value="select * from tbl_user where name like %?1" ,nativeQuery=true)
public List<UserModel> findByUuidOrAge(@Param("nn") String name);
```

注意，使用命名化参数：@Param

4. JpaRepository 限制查询

有时候我们需要查询前 n 个元素，或者只取前 n 个实体

```java
User findFirstByOrderByLastnameAsc();
User findTopByOrderByAgeDesc();
Page<User> queryFirst10ByLastname(String lastname, Pageable pageable);
List<User> findFirst10ByLastname(String lastname, Sort sort);
List<User> findTop10ByLastname(String lastname, Pageable pageable);
```

5. JpaRepository 多表查询

略，可参考上方链接

6. 添加、更新、删除

需要添加 @Modifying、@Transactional（声明式事务） 注解，具体使用和源码实现可参考链接

