### 利用 toJSONString() 来包含或排除指定的属性

Java 开发中，经常需要将一个实体对象转化为 Json 字符串，而利用 FastJson 来实现非常方便，只要使用 FastJson 中 JSONObject 静态类提供的 toJSONString() 静态方法即可。

而如果实体对象中有些属性的值为 null，那么默认转换后的 Json 字符串中是不包含这些值的。

案例代码：

```java
import com.alibaba.fastjson.JSONObject;
import com.alibaba.fastjson.serializer.SerializerFeature;

/**
 * 使用FastJson将实体对象转换成Json字符串测试类
 */
public class FastJsonApplication {

    public static void main(String[] args) {
        User user = new User();
        user.setId(1L);
        user.setUsername("张三");
        user.setPassword("");
        user.setMobile(null);

        /**
         * 忽略值为null的属性
         */
        String jsonUser = JSONObject.toJSONString(user, true);
        System.out.println(jsonUser);

        /**
         * 忽略值为null的属性
         */
        jsonUser = JSONObject.toJSONString(user, SerializerFeature.PrettyFormat);
        System.out.println(jsonUser);

        /**
         * 包括值为null的属性
         */
        //第二个属性为展示格式，第三个属性为是否保留 null 的字段
        jsonUser = JSONObject.toJSONString(user, SerializerFeature.PrettyFormat, SerializerFeature.WriteMapNullValue);
        System.out.println(jsonUser);
    }

    /**
     * 用户实体类
     */
    @Data
    @ToString
    @AllArgsConstructor
    @NoArgsConstructor
    public static class User {
        private Long id;
        private String username;
        private String password;
        private String mobile;
    }
```



得到输出：
{
    "id":1,
    "password":"",
    "username":"张三"
}
{
    "id":1,
    "password":"",
    "username":"张三"
}
{
    "id":1,
    "mobile":null,
    "password":"",
    "username":"张三”
}

除了上面的默认去除或手动保留null字段，还可以使用SerializeFilter来指定包含或者排除的属性，使得生成的JSON字符串中包含或者不包含某些属性。

示例代码：

```java
import com.alibaba.fastjson.JSONObject;
import com.alibaba.fastjson.serializer.SerializerFeature;
import com.alibaba.fastjson.support.spring.PropertyPreFilters;

/**
 * 使用FastJson将实体对象转换成Json字符串测试类
 */
public class FastJsonApplication {

    public static void main(String[] args) {
        User user = new User();
        user.setId(1L);
        user.setUsername("张三");
        user.setPassword("");
        user.setMobile(null);
        user.setCountry("中国");
        user.setCity("武汉");

        String jsonUser = null;

        /**
         * 指定排除属性过滤器和包含属性过滤器
         * 指定排除属性过滤器：转换成JSON字符串时，排除哪些属性
         * 指定包含属性过滤器：转换成JSON字符串时，包含哪些属性
         */
        String[] excludeProperties = {"country", "city"};
        String[] includeProperties = {"id", "username", "mobile"};
        PropertyPreFilters filters = new PropertyPreFilters();
        PropertyPreFilters.MySimplePropertyPreFilter excludefilter = filters.addFilter();
        excludefilter.addExcludes(excludeProperties);
        PropertyPreFilters.MySimplePropertyPreFilter includefilter = filters.addFilter();
        includefilter.addIncludes(includeProperties);


        /**
         * 情况一：默认忽略值为null的属性
         */
        jsonUser = JSONObject.toJSONString(user, SerializerFeature.PrettyFormat);
        System.out.println("情况一:\n" + jsonUser);

        /**
         * 情况二：包含值为null的属性
         */
        jsonUser = JSONObject.toJSONString(user, SerializerFeature.PrettyFormat, SerializerFeature.WriteMapNullValue);
        System.out.println("情况二:\n" + jsonUser);

        /**
         * 情况三：默认忽略值为null的属性，但是排除country和city这两个属性
         */
        jsonUser = JSONObject.toJSONString(user, excludefilter, SerializerFeature.PrettyFormat);
        System.out.println("情况三:\n" + jsonUser);

        /**
         * 情况四：包含值为null的属性，但是排除country和city这两个属性
         */
        jsonUser = JSONObject.toJSONString(user, excludefilter, SerializerFeature.PrettyFormat, SerializerFeature.WriteMapNullValue);
        System.out.println("情况四:\n" + jsonUser);

        /**
         * 情况五：默认忽略值为null的属性，但是包含id、username和mobile这三个属性
         */
        jsonUser = JSONObject.toJSONString(user, includefilter, SerializerFeature.PrettyFormat);
        System.out.println("情况五:\n" + jsonUser);

        /**
         * 情况六：包含值为null的属性，但是包含id、username和mobile这三个属性
         */
        jsonUser = JSONObject.toJSONString(user, includefilter, SerializerFeature.PrettyFormat, SerializerFeature.WriteMapNullValue);
        System.out.println("情况六:\n" + jsonUser);
    }
}
```