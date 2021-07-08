在SpringDataJPA中使用repository来进行数据层操作，作用相当于dao层，直接使用repository对象调用已经实现好的数据层操作方法。

在项目中，我们通常需要为每一个实体类创建一个 repository 接口，通常继承自JpaRepository接口，以EnvironmentInfo这张表为例，申明的接口名一般是表名+Repository。

- 可以直接修改参数实现查表操作，返回的数据可以是一张表，也可以是多张表
- 做复杂查询，删除或者修改操作时，需要加注解，并添加数据库语句，具体可以见下表

```java
public interface TaskInfoRepository extends JpaRepository<TaskInfo, Long> {
    //查找单张表
    TaskInfo findById(int id);

    //查找多张表
    List<TaskInfo> findByTaskStatus(TaskStates taskState);

    //复杂查询
    @Query(value = "select id from test_task where test_type_id = :testType and update_time <= :time "
                + "and task_status in :states", nativeQuery = true)
    List<Integer> findIdByTestTypeIdAndUpdateTimeAndTaskStatus(@Param("testType") int testType,
                                                                  @Param("time") long time,
                                                                  @Param("states") List<Integer> states);

    //修改（删除也类似）
    @Transactional
    @Modifying
    @Query(value = "update test_task set task_status = :taskStatus, task_error_msg = :errorMsg " 
                + "where id in :idList", nativeQuery = true)
    void setTaskStatusAndErrorMsg(@Param("idList") List<Integer> idList, @Param("taskStatus") int taskStatus,
                                  @Param("errorMsg") String errorMsg);
}
```
