***\*程序填空题\****

1、若需返回自增主键对应的类变量为cardId，请完善以下代码:

<insert id = “insert" parameterType="CardPOJO” 

——————————————————

<linsert>

 

2、若存在唯一的 SpellChecker类型的bean，则将其注入，否则匹配id为spell的bean。请按以上要求完善以下代码。

public TextEditor {

————

————

private SpellChecker spellChecker;

}

 

3、完善以下代码。

public class StudentDaoImpl extends JdbcDaoSupport{

public void insert(StudentPOJO student){

String sql = "insert into student (student_name, age, course) value(?,?,?)";

——————

student.getAge(),student.getCourse());

}

}

 

 

4、完善以下代码。

public static void main(String[]args) throws Exception{

ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");

JdbcTemplate jdbcTemplate = context.getBean("jdbcTemplate", JdbcTemplate.class);

String sql = "select* from student where student_id = ?";

————————rowMapper = new StudentRowMapper<StudentPOJO>();

StudentPOJO student = jdbcTemplate.——————(sql, rowMapper, "1");

}

 

5、填写完善以下代码，使得请求/get/ {id}与get方法匹配。

@Controller

public class StudentController {

@RequestMapping(————, produces=“applicationljson; charset=UTF-8'')

public StudentPOJO get(—————— String studentId){}

}

 

6、填写完善以下代码，使得请求/student/insert 与insert方法匹配，请求方式为POST。

@Controller

@RequestMapping("student")

public class StudentController {

————————

public void insert(—————— StudentPOJO studentPOJO){}

}

 

7、填写完善以下代码，使得请求/get 与get方法匹配，请求方式为Get,请求参数名为id，响应数据以JSON格式返回。

@Controller

public class StudentController{

————————

————————

public StudentPOJO get( @RequestParam("id")String studentId){}

}

 

8、将在SpringMVC配置文件中完成自定义参数解析器的配置。

————————

<mvc:argument-driven>

<——————class="com.example.MyResovler"/>

</mvc:argument-driven>

————————











程序题

1、根据以下表字段与类属性的对应关系，在Mybatis配置文件中定义get方法，该方法可根据主键card_id获取card_info表数据，表对应的类的别名为CardPOJo。请给出定义的<ResultMap>和<select>。

![img](file:///C:\Users\HP\AppData\Local\Temp\ksohtml15772\wps1.jpg) 

 







 



![image-20220603153359686](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220603153359686.png)



![image-20220603153407605](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220603153407605.png)

![image-20220603153413427](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220603153413427.png)













![image-20220603153419850](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220603153419850.png)

![









![image-20220603161046194](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220603161046194.png)

.



.



![image-20220603153430182](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220603153430182.png)