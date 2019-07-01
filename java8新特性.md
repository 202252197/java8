### 为何需要Lambda表达式

```
1.在Java中,我们无法将函数作为参数传递给一个方法,也无法声明返回一个函数的方法
2.在JavaScript中,函数参数是一个函数,返回值是另一个函数的情况是非常常见的; JavaScript是一门非常典型的函数式语言
```

### 何为Lambada表达式

```
Lambda: In programming languages such as Lisp, Python and Ruby lambda is an operator used to denote anonymous functions or closures, following the usage of lambda calculus。
```

### Lambda表达式的基本结构

```
java Lambda表达式是一种匿名函数;他是没有声明的方法,即没有访问修饰符,返回值声明和名字
java中的Lambda表达式基本语法
    (argument)->(body)
比如说
    (arg1,arg2..)->{body}
    (type1 arg1,type2 arg2..)->{body}   
```

### FunctionalInterface

```
java.lang.Functionallnterface
表示所声明的接口为函数式接口
如果不满足函数式接口的要求,则编译器报错
并非必须,但凡满足函数式接口条件的接口,编译器均将其看作是函数式接口,即便没有添加Functionallnterface注解亦如此。
```

### 关于函数式接口

```
如果一个接口只有一个抽象方法,那么该接口就是一个函数式接口。
如果我们在某个接口上声明了FunctionalInterface注解,那么编译器就会按照函数式接口的定义来要求该接口
如果某个接口只有一个抽象方法,但我们并没有给接口声明FunctionalInterface注解,那么该编译器依旧会将该接口看做是函数式接口。
```

### Lambda示例说明

```
Lambda示例说明
(int a,int b)->{return a+b}
()->System.out.println("Hello World")
(String s)->{System.out.println(s);}
()->42
()->{return 3.1415};
```

### Lambda结构

```
一个Lambda表达式可以有零个或多个参数
参数的类型既可以明确声明,也可以根据上下文来推断。例如:(int a)与(a)效果相同
所有参数需包含在圆括号内,参数之间用逗号相隔。例如:(a,b)或(int a,int b)或(String a,int b,float c)
空圆括号代表参数为空。例如:()->42
当只有一个参数,且类型可推导时,圆括号{}可以省略。例如:a->return a*a
Lambda表达式的主体可包含零条或多条语句
如果Lambda表达式的主体只有一条语句,花括号{}可以省略。匿名函数的返回类型与该主体表达式一致
如果Lambda表达式的主体包含一条以上语句,则表达式必须包含在花括号{}中(形成代码块)。匿名函数的返回类型与代码块的返回类型一致,若没有返回则为空
```

### Lambda表达式作用

```
Lambda表达式为java添加了缺失的函数式编程特性,使我们将函数当做一等公民看待
在将函数作为一等公民的语言中,Lambda表达式的类型就是函数。但在java中,Lambda表达式是对象,他们必须依附于一类特别的对象类型——函数式接口(functional interface)
```

### Lambda流的基本使用

```java
public static void main(String[] args) {
		List<String> list=Arrays.asList("lvshihao","i love","zhang");
		list.stream().map(item->item.toUpperCase()).forEach(System.out::println);
		list.stream().map(String::toUpperCase).forEach(System.out::println);
	}
```

### Lambda排序使用

```java
public static void main(String[] args) {
		List<String> list=Arrays.asList("lvshihao","i love","zhang");
		Collections.sort(list,(o1,o2)->o1.compareTo(o2));
		list.stream().map(String::toUpperCase).forEach(System.out::println);
	}
```

### 函数式接口的使用

#### Function接口详解

```java
import java.util.function.Function;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午1:01:06   
* @version
 */
public class FunctionTest {
	public static void main(String[] args) {
		int compat = mycompose(2, value->value*3, value->value*value);
		int compat2 = myandThen(2, value->value*3, value->value*value);
		int compat3 =myapply(3,value->value*8);
		System.out.println(compat);
		System.out.println(compat2);
		System.out.println(compat3);
	}
	public static int myapply(int a,Function<Integer, Integer> function) {
		return function.apply(a);
	}
	public static int mycompose(int a,Function<Integer, Integer> function,Function<Integer, Integer> function2) {
		/**
		 *  default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
		        Objects.requireNonNull(before);
		        return (V v) -> apply(before.apply(v));
		    }
		 * 1.首先调用第一个函数的compose方法将第二个函数传入进去
		 * 2.首先Objects.requireNonNull(before);判断一下此对象是否为空
		 * 3.首先看before.apply(v),先计算的是第二个函数的apply方法也就是v*v
		 * 4.然后计算得出的结果在给第一个函数的apply传入进去,然后返回这个函数
		 * 5.再调用第一个函数的apply进行计算
		 */
		return function.compose(function2).apply(a);
	}
	public static int myandThen(int a,Function<Integer, Integer> function,Function<Integer, Integer> function2) {
		/**
		 * 和上面一样只不过就是反着先计算第一个,在计算第二个函数
		 */
		return function.andThen(function2).apply(a);
	}
}
```

#### BiFunction函数式接口详解

```java
import java.util.function.BiFunction;
import java.util.function.Function;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午1:22:11   
* @version
 */
public class BiFunctionTest {
	public static void main(String[] args) {
		int apply = apply(5, 1, (value1,value2)->value1*value2);
		System.out.println(apply);
		int myandThen = myandThen(2, 3,(value1,value2)->value1+value2, value->value*2);
		System.out.println(myandThen);
		//<P>思考BiFunctionTest为什么没有compose这个默认方法,很容易理解,
		//想想你先计算的是一个<em>function OR bifunction</em>,也就是只返回一个值,
		//那么怎么可能再执行第二个bifunction呢</P>
	}
	public static int apply(int a,int b,BiFunction<Integer, Integer, Integer> function) {
		return function.apply(a, b);
	}
	public static int myandThen(int a,int b,BiFunction<Integer, Integer, Integer> function,Function<Integer,Integer> function2) {
		return function.andThen(function2).apply(a, b);
	}
}
```

#### BiFunction函数式接口实例

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.BiFunction;
import java.util.stream.Collectors;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午2:03:08   
* @version
 */
public class BiFunctionInstance {
	public static void main(String[] args) throws ClassNotFoundException {
		Person person=new Person("lvshihao",17);
		Person person2=new Person("xiaoming", 15);
		Person person3=new Person("zhangying", 17);
		List<Person> list=Arrays.asList(person,person2,person3);
		List<Person> searchAge = searchAge(15,list);
		List<Person> searchUserName = searchUserName("lvshihao", list);
		searchUserName.forEach(value->{System.out.println(value.getUserName());});
		searchAge.forEach(value->{System.out.println(value.getUserName());});
		
		
		List<Person> searchAge2 = searchAge2(15, list,(v,v1)->v1.stream().filter(value->value.getAge()>v).collect(Collectors.toList()));
		searchAge2.forEach(value->System.out.println(value.getUserName()));
		
	}
	public static List<Person> searchUserName(String userName,List<Person> list){
		return list.stream().filter(item->item.getUserName().equals(userName))
				.collect(Collectors.toList());
	}
	public static List<Person> searchAge(int age,List<Person> list){
		BiFunction<Integer, List<Person>, List<Person>> biFunction=(v,v1)->
			v1.stream().filter(person->person.getAge()>age)
			.collect(Collectors.toList());
		;
		return biFunction.apply(age, list);
	}
	public static List<Person> searchAge2(int age,List<Person> list,BiFunction<Integer, List<Person>, List<Person>> bifunction){
		return bifunction.apply(age, list);
	}
}
class Person{ 
	public Person(String userName,int age) {
		setUserName(userName);
		setAge(age);
	}
	private String userName;
	private int age;
	/**
	 * @return the userName
	 */
	public String getUserName() {
		return userName;
	}
	/**
	 * @param userName the userName to set
	 */
	public void setUserName(String userName) {
		this.userName = userName;
	}
	/**
	 * @return the age
	 */
	public int getAge() {
		return age;
	}
	/**
	 * @param age the age to set
	 */
	public void setAge(int age) {
		this.age = age;
	}
}
```

#### Predicate函数式接口使用实例

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午3:28:09   
* @version
 */
public class PredicateTest {
	public static void main(String[] args) {
		Predicate<String> predicate=value->value.length()>5;
		boolean test = predicate.test("lo");
		System.out.println(test);
		
		List<Integer> list=Arrays.asList(1,2,3,4,5,6,7,8,9,10);
		
		conditionFilter(list,(value)->value%2==0);
		System.out.println("------------------");
		conditionFilter2(list,(value)->value%3==0);
		System.out.println("------------------");
		conditionFilterAnd(list, v->v>5, v->v%3==0);
		System.out.println("------------------");
		conditionFilterOR(list, v->v>5,v-> v%3==0);
		System.out.println("------------------");
		conditionFilterAndSumNegate(list, v->v>5, v->v%3==0);
		System.out.println("------------------");
		Predicate<Object> conditionisEqual = conditionisEqual("lvshihao");
		boolean test2 = conditionisEqual.test("lvshihao");
		System.out.println(test2);
	}

	public static void conditionFilter(List<Integer> list,Predicate<Integer> predicate) {
		for (int i = 0; i <=list.size(); i++) {
			if(predicate.test(i)) {
				System.out.println(i);
			}
		}
	}
	public static void conditionFilter2(List<Integer> list,Predicate<Integer> predicate) {
		List<Integer> collect = list.stream().filter(predicate).collect(Collectors.toList());
		collect.forEach(System.out::println);
	}
	
	public static void conditionFilterAnd(List<Integer> list,Predicate<Integer> predicate,Predicate<Integer> predicate2) {
		List<Integer> collect = list.stream().filter(v->predicate.and(predicate2).test(v)).collect(Collectors.toList());
		collect.forEach(System.out::println);
	}
	
	public static void conditionFilterOR(List<Integer> list,Predicate<Integer> predicate,Predicate<Integer> predicate2) {
		List<Integer> collect = list.stream().filter(v->predicate.or(predicate2).test(v)).collect(Collectors.toList());
		collect.forEach(System.out::println);
	}
	
	public static void conditionFilterAndSumNegate(List<Integer> list,Predicate<Integer> predicate,Predicate<Integer> predicate2) {
		List<Integer> collect = list.stream().filter(v->predicate.and(predicate2).negate().test(v)).collect(Collectors.toList());
		collect.forEach(System.out::println);
	}
	public static Predicate<Object> conditionisEqual(String name) {
		return 	Predicate.isEqual(name);
	}
}
```

#### Supplier函数式接口使用实例

```java
import java.util.function.Supplier;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午6:28:09   
* @version
 */

public class SupplierTest {
	public static void main(String[] args) {
		Supplier<String> supplier=()->"Hellow LVSHIHAO";
		System.out.println(supplier.get());
		Supplier<Student> supplier2=()->new Student("lvshihao",15);
		System.out.println(supplier2.get().getName());
		Supplier<Student> supplier3=Student::new;
		System.out.println(supplier3.get().getName());
	}
}
class Student{
	private String name;
	private int age;
	public Student() {
		// TODO Auto-generated constructor stub
		setName("lvshihao I Love You");
		setAge(18);
	}
	public Student(String name,int age) {
		// TODO Auto-generated constructor stub
		setName(name);
		setAge(age);
	}
	/**
	 * @return the name
	 */
	public String getName() {
		return name;
	}
	/**
	 * @param name the name to set
	 */
	public void setName(String name) {
		this.name = name;
	}
	/**
	 * @return the age
	 */
	public int getAge() {
		return age;
	}
	/**
	 * @param age the age to set
	 */
	public void setAge(int age) {
		this.age = age;
	}
	
}
```

#### BinaryOperator函数式接口使用

```java
import java.util.function.BinaryOperator;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午6:55:52   
* @version
 */
public class BinaryOperatorTest {
	public static void main(String[] args) {
		BinaryOperator<Integer> binaryOperator=(v1,v2)->v1*v2;
		Integer apply = binaryOperator.apply(5, 7);
		System.out.println(apply);
		Integer apply2 = BinaryOperator.minBy((Integer o1,Integer o2)-> o1-o2).apply(5, 3);
		System.out.println(apply2);
		Integer apply3 = BinaryOperator.maxBy((Integer o1,Integer o2)-> o1-o2).apply(5, 3);
		System.out.println(apply3);
		
		System.out.println("-----------------");
		System.out.println(getShort(5, 6,BinaryOperator.minBy((v1,v2)->v1-v2)));
		System.out.println(getLong(5, 6,BinaryOperator.maxBy((v1,v2)->v1-v2)));
	}
	public static int getShort(int a,int b,BinaryOperator<Integer> binaryOperator) {
		return binaryOperator.apply(a, b);
	}
	public static int getLong(int a,int b,BinaryOperator<Integer> binaryOperator) {
		return binaryOperator.apply(a, b);
	}
}
```

### Optional类详解

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
import java.util.Optional;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午8:05:25   
* @version
 */
public class OptionalTest {
	public static void main(String[] args) {
		//构造一个空值Optional对象
		Optional<String> optional=Optional.empty();
		if(!optional.isPresent()) {
			System.out.println("没有值");
		}
		//构造一个包含lvshihao值的Optional对象
		Optional<String> optional2=Optional.of("lvshihao");
		if(optional2.isPresent()/*判断此对象的值是否为空*/) {
			String string = optional2.get();//获取此对象容器的值
			System.out.println(string);
		}
		//如果像上面这种写法和普通判断值是否为空操作一样所有没有上面简单的
		//下面采用Lambda表达式(推荐使用)
		Optional<String> optional3=Optional.of("lvshihao I Love You");
		optional3.ifPresent(System.out::println);
		
		//意思是如果optional没有值就会输出当前的值
		System.out.println(optional.orElse("没有值"));
		
		//意思是如果optional没有值就会输出当前的值,但是支持Lambda表达式
		System.out.println(optional.orElseGet(()->"没有值"));
		
		//此方法用于不知道传入进来的值是否为空
		Optional<String> optional4=Optional.ofNullable("555");
		optional4.ifPresent(System.out::println);
		
		//使用map判断对象是否为空,为空则返回空,否则返回当前集合
		Employee employee=new Employee();
		employee.setName("lvshihao");
		
		Employee employee1=new Employee();
		employee.setName("zhangying");
		
		Company company=new Company();
		List<Employee> list=Arrays.asList(employee,employee1);
		company.setEmployees(list);
		Optional<Company> optional5=Optional.ofNullable(company);
		System.out.println(optional5.map(theCompany->theCompany.getEmployees()).orElse(Collections.emptyList()));
	}
}

class Company{
	private String name;
	private List<Employee> employees;
	/**
	 * @return the name
	 */
	public String getName() {
		return name;
	}
	/**
	 * @param name the name to set
	 */
	public void setName(String name) {
		this.name = name;
	}
	/**
	 * @return the employees
	 */
	public List<Employee> getEmployees() {
		return employees;
	}
	/**
	 * @param employees the employees to set
	 */
	public void setEmployees(List<Employee> employees) {
		this.employees = employees;
	}
	
}

class Employee{
	private String name;

	/**
	 * @return the name
	 */
	public String getName() {
		return name;
	}

	/**
	 * @param name the name to set
	 */
	public void setName(String name) {
		this.name = name;
	}
}

```

### 方法引用

#### 类名::静态方法名

```java
import java.util.Arrays;
import java.util.List;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午8:45:52   
* @version
 */
public class MethodReference {
	public static void main(String[] args) {
		Studente studente1=new Studente("lvshihao",44);
		Studente studente2=new Studente("liuxinyu",25);
		Studente studente3=new Studente("zhangying",12);
		Studente studente4=new Studente("guoyayu",2);
		List<Studente> list=Arrays.asList(studente1,studente2,studente3,studente4);
		list.sort((v1,v2)->Studente.compareStudentByScore(v1,v2));
		list.forEach((v)->System.out.println(v.getScore()));
		
		
		list.sort((v1,v2)->Studente.compareStudentByName(v1,v2));
		list.forEach((v)->System.out.println(v.getName()));
		System.out.println("---------------");
		//使用静态方法引用的方式
		list.sort(Studente::compareStudentByScore);
		list.forEach((v)->System.out.println(v.getScore()));
		
		list.sort(Studente::compareStudentByName);
		list.forEach((v)->System.out.println(v.getName()));
	}
	
}
class Studente{
	private String name;
	private int score;
	public Studente(String name, int score) {
		super();
		this.name = name;
		this.score = score;
	}
	/**
	 * @return the name
	 */
	public String getName() {
		return name;
	}
	/**
	 * @param name the name to set
	 */
	public void setName(String name) {
		this.name = name;
	}
	/**
	 * @return the score
	 */
	public int getScore() {
		return score;
	}
	/**
	 * @param score the score to set
	 */
	public void setScore(int score) {
		this.score = score;
	}
	public static int compareStudentByScore(Studente student1,Studente student2) {
		return student1.getScore()-student2.getScore();
	}
	public static int compareStudentByName(Studente student1,Studente student2) {
		return student1.getName().compareToIgnoreCase(student2.getName());
	}
}
```

#### 引用名（对象名）::实例方法名

```java
import java.util.Arrays;
import java.util.List;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午8:45:52   
* @version
 */
public class MethodReference {
	public static void main(String[] args) {
		Studente studente1=new Studente("lvshihao",44);
		Studente studente2=new Studente("liuxinyu",25);
		Studente studente3=new Studente("zhangying",12);
		Studente studente4=new Studente("guoyayu",2);
		List<Studente> list=Arrays.asList(studente1,studente2,studente3,studente4);
		list.sort((v1,v2)->Studente.compareStudentByScore(v1,v2));
		list.forEach((v)->System.out.println(v.getScore()));
		
		
		list.sort((v1,v2)->Studente.compareStudentByName(v1,v2));
		list.forEach((v)->System.out.println(v.getName()));
		System.out.println("---------------");
		//使用实例方法引用的方式
		StudentComparator studentComparator=new StudentComparator();
		list.sort(studentComparator::compareStudentByScore);
		list.forEach((v)->System.out.println(v.getScore()));
		
		list.sort(studentComparator::compareStudentByName);
		list.forEach(v->System.out.println(v.getName()));
	}
	
}
class StudentComparator{
	public int compareStudentByScore(Studente student1,Studente student2) {
		return student1.getScore()-student2.getScore();
	}
	public int compareStudentByName(Studente student1,Studente student2) {
		return student1.getName().compareToIgnoreCase(student2.getName());
	}
}
class Studente{
	private String name;
	private int score;
	public Studente(String name, int score) {
		super();
		this.name = name;
		this.score = score;
	}
	/**
	 * @return the name
	 */
	public String getName() {
		return name;
	}
	/**
	 * @param name the name to set
	 */
	public void setName(String name) {
		this.name = name;
	}
	/**
	 * @return the score
	 */
	public int getScore() {
		return score;
	}
	/**
	 * @param score the score to set
	 */
	public void setScore(int score) {
		this.score = score;
	}
	public static int compareStudentByScore(Studente student1,Studente student2) {
		return student1.getScore()-student2.getScore();
	}
	public static int compareStudentByName(Studente student1,Studente student2) {
		return student1.getName().compareToIgnoreCase(student2.getName());
	}
}
```

#### 类名::实例方法名

```java
import java.util.Arrays;
import java.util.List;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午8:45:52   
* @version
 */
public class MethodReference {
	public static void main(String[] args) {
		Studente studente1=new Studente("lvshihao",44);
		Studente studente2=new Studente("liuxinyu",25);
		Studente studente3=new Studente("zhangying",12);
		Studente studente4=new Studente("guoyayu",2);
		List<Studente> list=Arrays.asList(studente1,studente2,studente3,studente4);
		list.sort((v1,v2)->Studente.compareStudentByScore(v1,v2));
		list.forEach((v)->System.out.println(v.getScore()));
		
		
		list.sort((v1,v2)->Studente.compareStudentByName(v1,v2));
		list.forEach((v)->System.out.println(v.getName()));
		System.out.println("---------------");
		//使用类名的实例方法
		//第一个参数就是这个Studente,第二个Studente是传进去的参数
		list.sort(Studente::compareByScore);
		list.forEach((v)->System.out.println(v.getScore()));
		
		list.sort(Studente::compareByName);
		list.forEach(v->System.out.println(v.getName()));
		System.out.println("--------------------");
		//比如
		List<String> list2=Arrays.asList("lvshihoa","kks","aas");
		list2.sort(String::compareToIgnoreCase);
		list2.forEach(System.out::println);
	}
	
}
class StudentComparator{
	public int compareStudentByScore(Studente student1,Studente student2) {
		return student1.getScore()-student2.getScore();
	}
	public int compareStudentByName(Studente student1,Studente student2) {
		return student1.getName().compareToIgnoreCase(student2.getName());
	}
}
class Studente{
	private String name;
	private int score;
	public Studente(String name, int score) {
		super();
		this.name = name;
		this.score = score;
	}
	/**
	 * @return the name
	 */
	public String getName() {
		return name;
	}
	/**
	 * @param name the name to set
	 */
	public void setName(String name) {
		this.name = name;
	}
	/**
	 * @return the score
	 */
	public int getScore() {
		return score;
	}
	/**
	 * @param score the score to set
	 */
	public void setScore(int score) {
		this.score = score;
	}
	public static int compareStudentByScore(Studente student1,Studente student2) {
		return student1.getScore()-student2.getScore();
	}
	public static int compareStudentByName(Studente student1,Studente student2) {
		return student1.getName().compareToIgnoreCase(student2.getName());
	}
	public int compareByScore(Studente student1) {
		return this.getScore()-student1.getScore();
	}
	public int compareByName(Studente student1) {
		return this.getName().compareTo(student1.getName());
	}
}
```

#### 构造方法引用(类名::new)

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;
import java.util.function.Supplier;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月25日 下午8:45:52   
* @version
 */
public class MethodReference {
	
	public static void main(String[] args) {
		Studente studente1=new Studente("lvshihao",44);
		Studente studente2=new Studente("liuxinyu",25);
		Studente studente3=new Studente("zhangying",12);
		Studente studente4=new Studente("guoyayu",2);
		List<Studente> list=Arrays.asList(studente1,studente2,studente3,studente4);
		list.sort((v1,v2)->Studente.compareStudentByScore(v1,v2));
		list.forEach((v)->System.out.println(v.getScore()));
		
		
		list.sort((v1,v2)->Studente.compareStudentByName(v1,v2));
		list.forEach((v)->System.out.println(v.getName()));
		System.out.println("---------------");
		//构造方法引用(类名::new)
		String toString = getToString(String::new);
		System.out.println(toString);
		String toString2=getToString2(String::new);
		System.out.println(toString2);
		
	}
	public static String getToString(Supplier<String> supplier) {
		return supplier.get()+"吕世昊";
	}
	public static String getToString2(Function<String, String> function) {
		return function.apply("aaaaaa")+"吕世昊";
	}
}
class StudentComparator{
	public int compareStudentByScore(Studente student1,Studente student2) {
		return student1.getScore()-student2.getScore();
	}
	public int compareStudentByName(Studente student1,Studente student2) {
		return student1.getName().compareToIgnoreCase(student2.getName());
	}
}
class Studente{
	private String name;
	private int score;
	public Studente(String name, int score) {
		super();
		this.name = name;
		this.score = score;
	}
	/**
	 * @return the name
	 */
	public String getName() {
		return name;
	}
	/**
	 * @param name the name to set
	 */
	public void setName(String name) {
		this.name = name;
	}
	/**
	 * @return the score
	 */
	public int getScore() {
		return score;
	}
	/**
	 * @param score the score to set
	 */
	public void setScore(int score) {
		this.score = score;
	}
	public static int compareStudentByScore(Studente student1,Studente student2) {
		return student1.getScore()-student2.getScore();
	}
	public static int compareStudentByName(Studente student1,Studente student2) {
		return student1.getName().compareToIgnoreCase(student2.getName());
	}
	public int compareByScore(Studente student1) {
		return this.getScore()-student1.getScore();
	}
	public int compareByName(Studente student1) {
		return this.getName().compareTo(student1.getName());
	}
}
```

### jdk8接口引入默认方法的原因

```
1.接口中的默认方法不会让你去实现,因为已经有写好的方法体
2.接口中设计默认方法好处是有很好的兼容性,以前的代码假如使用了一个接口,那么现在你再在这个接口上新加一个
抽象方法那么就必须又要实现,要是这么回事的话,很老的项目岂不是还需要一个一个手动实现方法,所有有了默认方
法不需要去实现,直接就继承了
```

### Stream流

```
1.源
2.零个或多个中间操作
3.终止操作
Collection提供了新的stream()方法
流不存储值,通过管道的方式获取值
本质是函数式的,对流的操作会生产一个结果,不过并不会修改底层的数据源,集合可以作为流的底层数据源
延迟查找,很多流操作(过滤,映射,排序等)都可以延迟实现

1.和迭代器又不同的是, Stream可以并行化操作,迭代器只能命令式地、串行化操作
2.当使用串行方式去遍历时,每个item读完后再读下一个 item
3.使用并行去遍历时,数据会被分成多个段,其中每一个都在不同的线程中处理,然后将结果一起输出Stream的并行操作依赖于Java7中引入的Fork/Join框架
```

#### 流操作的分类

```
1.惰性求值
2.及早求值
```

#### Stream创建的方式

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月26日 下午2:59:33   
* @version
 */
public class StreamTest {
	@SuppressWarnings("all")
	public static void main(String[] args) {
		//传入多个元素
		Stream stream=Stream.of("lvshihao","zhangying","liuxinyu");
		String[] arr=new String[] {"lvshihao","zhangying","liuxinyu"};
		//传入一个数组
		Stream stream2=Arrays.stream(arr);
		//获取一个集合的流
		List<String> asList = Arrays.asList(arr);
		Stream stream3=asList.stream();
	}
}
```

#### Stream的基本使用

```java
import java.util.stream.IntStream;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月26日 下午4:03:01   
* @version
 */
public class IntStreamTest {
	public static void main(String[] args) {
		IntStream.of(new int[] {44,5,6,7,9,5}).forEach(System.out::println);;
		System.out.println("---------------");
		//包前不包尾
		IntStream.range(3,6).forEach(System.out::println);
		System.out.println("---------------");
		//包前包尾
		IntStream.rangeClosed(3,6).forEach(System.out::println);
		
		//使用Lambda表达式一行代码计算所有值相加的和
		int reduce = IntStream.rangeClosed(1,100).reduce(0,Integer::sum);
		System.out.println(reduce);
	}
}

```

#### Stream深度解析

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Optional;
import java.util.TreeSet;
import java.util.stream.Collectors;
import java.util.stream.Stream;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月27日 上午9:23:45   
* @version
 */
public class StreamTest2 {
	public static void main(String[] args) {
		Stream<String> stream=Stream.of("lvshihao","lvshihao1","lvshihao2");
		String[] array = stream.toArray(String[]::new);
		Arrays.asList(array).forEach(System.out::println);
		
		Stream<String> stream1=Stream.of("lvshihao","lvshihao1","lvshihao2");
		List<String> collect = stream1.collect(()->new ArrayList<>(),(item,v2)->item.add(v2), (item1,item2)->item1.addAll(item2));
		collect.forEach(System.out::println);
		
		Stream<String> stream2=Stream.of("lvshihao","lvshihao1","lvshihao2");
		ArrayList<String> collect2 = stream2.collect(ArrayList::new, List::add,List::addAll);
		collect2.forEach(System.out::println);
		
		Stream<String> stream3=Stream.of("lvshihao","lvshihao1","lvshihao2");
		List<String> collect3 = stream3.collect(Collectors.toList());
		collect3.forEach(System.out::println);
		
		Stream<String> stream4=Stream.of("lvshihao","lvshihao1","lvshihao2");
		List<String> collect4 = stream4.collect(Collectors.toCollection(ArrayList::new));
		collect4.forEach(System.out::println);
		
		Stream<String> stream5=Stream.of("lvshihao","lvshihao1","lvshihao2");
		TreeSet<String> collect5 = stream5.collect(Collectors.toCollection(TreeSet::new));
		collect5.forEach(System.out::println);
		
		Stream<String> stream6=Stream.of("lvshihao","lvshihao1","lvshihao2");
		String string = stream6.collect(Collectors.joining()).toString();
		Optional.of(string).ifPresent(System.out::println);
	}
}
```

#### Stream实例剖析

```java
import java.util.Arrays;
import java.util.List;
import java.util.Optional;
import java.util.UUID;
import java.util.stream.Collectors;
import java.util.stream.Stream;

/**
* 个性签名：今天的努力只要对得起自己就满足了
* 创建人：吕世昊  
* 创建时间：2019年3月28日 下午2:48:55   
* @version
 */
public class StreamTest3 {
	public static void main(String[] args) {
		List<String> asList = Arrays.asList("lvshihao","zhangying","saaa");
		asList.stream().map((v)->v.toUpperCase()).collect(Collectors.toList()).forEach(System.out::println);
		Optional.of("-------------------------").ifPresent(System.out::println);
		List<Integer> list=Arrays.asList(1,5,6,7,8,9);
		list.stream().map(v->v*v).collect(Collectors.toList()).forEach(System.out::println);
		Optional.of("-------------------------").ifPresent(System.out::println);
		
		//在流中操作多个集合,然和再解析成流在进行使用
		Stream<List<Integer>> of = Stream.of(Arrays.asList(1),Arrays.asList(2,3),Arrays.asList(4,5,6));
		of.flatMap(v->v.stream()).map(x->x*x).forEach(System.out::println);
		
		Optional.of("-------------------------").ifPresent(System.out::println);
		Stream<String> stream=Stream.empty();
		stream.findFirst().ifPresent(System.out::println);
		
		Optional.of("-------------------------").ifPresent(System.out::println);
		Stream<String> stream1=Stream.generate(UUID.randomUUID()::toString);
		stream1.findFirst().ifPresent(System.out::println);
		
		
		Optional.of("找出该流中大于2的元素,然后将每个元素乘以2,然后忽略掉流中的前两个元素,然后再取流中的前两个元素,最后求出流中元素的总和").ifPresent(System.out::println);
		Stream.iterate(1,item->item+2).limit(6).forEach(System.out::println);
		Optional.of("-------------------------").ifPresent(System.out::println);
		Stream.iterate(1,item->item+2).limit(6).filter(v->v>2).forEach(System.out::println);
		Optional.of("-------------------------").ifPresent(System.out::println);
		Stream.iterate(1,item->item+2).limit(6).filter(v->v>2).map(value->value*2).forEach(System.out::println);
		Optional.of("-------------------------").ifPresent(System.out::println);
		Stream.iterate(1,item->item+2).limit(6).filter(v->v>2).map(value->value*2).skip(2).forEach(System.out::println);
		Optional.of("-------------------------").ifPresent(System.out::println);
		Stream.iterate(1,item->item+2).limit(6).filter(v->v>2).map(value->value*2).skip(2).limit(2).forEach(System.out::println);
		Optional.of("-------------------------").ifPresent(System.out::println);
		Integer reduce = Stream.iterate(1,item->item+2).limit(6).filter(v->v>2).map(value->value*2).skip(2).limit(2).reduce(0,(v,v2)->v+v2);
		Optional.of(reduce).ifPresent(System.out::println);
	}
}
```

#### 内部迭代与外部迭代本质剖析

```
集合关注的是数据与数据存储本身;
流关注的则是对数据的计算.
流与迭代器类似的一点是:流是无法重复使用或消费的。
```

#### Stream的分区和分组用法

```java
package com.lvshihao.service;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

/**
 * 功能描述
 *
 * @author 吕世昊
 * @date $
 * @describe 想成为世界最厉害的程序员
 * @email 202252197@qq.com
 * @signature 我的梦想---兰博基尼{奋斗}
 */
public class StreamTest {
    public static void main(String[] args) {
        User user=new User(1,"ll",5);
        User user1=new User(2,"lla",5);
        User user11=new User(3,"ll",52);
        List<User> asList =new ArrayList<>(Arrays.asList(user,user1,user11));
        //对数据进行分组
        Map<Integer, List<User>> collect2 = asList.stream().collect(Collectors.groupingBy(User::getAge));
        System.out.println(collect2);
        //对数据进行分组,返回组内数据的个数
        Map<Integer, Long> collect = asList.stream().collect(Collectors.groupingBy(User::getAge,Collectors.counting()));
        System.out.println(collect);
        //对数据进行分组,返回组内指定元素的平均值
        Map<String, Double> collect3 = asList.stream().collect(Collectors.groupingBy(User::getName,Collectors.averagingInt(User::getAge)));
        System.out.println(collect3);
        //对数据进行分区一共有两区true和false
        Map<Boolean, List<User>> collect1 = asList.stream().collect(Collectors.partitioningBy(v -> v.getAge() > 6));
        System.out.println(collect1);
        //对数据进行二次分区
        Map<Boolean, Map<Boolean, List<User>>> collect4 = asList.stream().collect(Collectors.partitioningBy(v -> v.getAge() > 6, Collectors.partitioningBy(vc -> vc.getId() > 1)));
        System.out.println(collect4);

    }
}
class User{
    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    private int id;
    private String name;
    private int age;

    public User() {
    }

    public User(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

