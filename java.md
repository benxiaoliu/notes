JVM memory
===

1. JVM 主要包括三块内存空间： 栈内存，堆内存，方法区内存
2. 堆内存和方法内存各有一个， 一个线程一个栈内存
3. 方法调用的时候，该方法所需要的内存空间在栈内存中分配，称为压栈。方法执行结束之后，该方法所属的内存空间释放，称为弹栈
4. 栈中主要存储的是方法体当中的局部变量
5. 方法的代码片段以及整个类的代码片段都被存储到方法区内存当中，在类加载的时候这些代码片段会载入
6. 在程序执行过程中使用new运算符创建的java对象，存储在堆内存当中。对象内部有实例变量。所以实例变量存储在堆内存当中
7. 变量分类：
    -局部变量（方法体中声明）
    -成员变量（方法体外声明）
      实例变量（前边修饰符没有static）
      静态变量（前边修饰符中有static）
8. 静态变量存储在方法区内存当中
9. 三块内存当中变化最频繁的是栈内存，最先有数据的是方法区内存。垃圾回收器主要针对的是堆内存
10. 垃圾回收器【自动垃圾回收机制，GC机制】
    -当堆内存当中的Java对象成为垃圾数据的时候，会被垃圾回收器回收
      没有更多的引用指向它的时候
      这个对象无法访问，因为访问对象只能通过引用的方式访问
      
Java 传产时传的是参数里面存的那个字面值

Java 自动垃圾回收机制
===
不再使用的内存空间应回收--垃圾回收
在C/C++等语言中，由程序员负责回收无用内存（优点：能够在内存不使用时快速回收，准确高效；缺点：容易失误出现bug，例如忘记编写回收内存的代码，内存一直不回收）
Java语言消除了程序员回收无用内存空间的责任：它提供一种系统级线程跟踪存储空间的分配情况。并在JVM空闲时，检查并释放那些可被释放的存储空间
垃圾回收在Java程序运行过程中自动进行，程序员无法进行精确控制和干预（优点：自动的，意味着不会出现忘记回收；缺点：回收不及时）

一般的观点是：宁可回收不及时但是一定要回收


正确的jdk使用方式：
（如果你同时有多个项目在做 并且不同项目使用的是不同版本的jdk，不可能每次都去卸载重新安装）
使用压缩版的jdk,根据情况解压不同版本来使用




this
===
this在方法内部使用，即这个方法所属对象的引用
this在构造器内部使用，表示该构造器正在初始化的对象

this表示当前对象，可以调用类的属性,方法，构造器

什么时候使用this关键字： 当在方法内需要用到 调用该方法的对象时，就用this

1. 当形参与成员变量重名时，如果在方法内部需要使用成员变量，必须添加this来表明该变量是类变量
2. 在任意方法内，如果使用当前类的成员变量或成员方法 可以在其前面添加this，增强程序的阅读性
3. this 可以作为一个类中，构造器相互调用的特殊格式 （使用this()必须放在构造器的首行！！使用this调用本类中其他的构造器，保证至少有一个构造器是不用this的【实际上是不能出现自己调用自己】）


```java
class Person{
	private String name;
	private int age;
    
    /////////////////////////////////////// 1. 当形参与成员变量重名时，如果在方法内部需要使用成员变量，必须添加this来表明该变量是类变量
	public Person(String name, int age){
		this.name = name;
		this.age = age;
	}
	
	public void getInfo(){
		System.out.println("xinming" + name);
		this.speak();
	}
	public void speak(){
    ////////////////////////////////////////////2. 在任意方法内，如果使用当前类的成员变量或成员方法 可以在其前面添加this，增强程序的阅读性
		System.out.println("nianning: "+this.age);
	}
}



// 3.  this 可以作为一个类中，构造器相互调用的特殊格式

class Person{
	private String name;
	private int age;
	
	public Person(){  // 无参构造
		System.out.println("新对象实例化");
	}
	public Person(String name){
		this();  //调用本类中的无参构造方法
		this.name = name;
	}
	public Person(String name, int age){
		this(name);  //调用本类中的有一个参数的构造方法
		this.age = age;
	}
}

```



static
===

当我们编写一个类时，其实就是在描述其对象的属性和行为，而并没有产生实质上的对象，只有通过new 关键字才会产生出对象，这时系统才会分配内存空间给对象，其方法才可以供外部调用。我们有时候希望无论是否产生了对象或无论产生了多少对象的情况下，某些特定的数据在内存空间里只有一份，例如所有的中国人都有个国家名称，每一个中国人都共享这个国家名称，不必再每一个中国人的实例对象中都单独分配一个用于代表国家名称的变量。
通过类名.属性去设置和访问

类属性，类方法的设计思想
类属性作为该类各个对象之间共享的变量。在设计类时，分析哪些类属性不因对象的不同而改变，将这些属性设置为类属性。相应的方法设置为类方法。
如果方法与调用者无关，则这样的方法通常被声明为类方法，由于不需要创建对象就可以调用类方法，从而简化了方法的调用

static 使用范围：
在Java类中，可用static修饰属性, 方法，代码块，内部类

被修饰的成员具备以下特点：
随着类的加载而加载
优先于对象存在（不用new就能用）
修饰的成员，被所有对象共享
访问权限允许时，可不创建对象，直接被类调用


类方法
因为不需要实例就可以访问static方法，因此static方法内部不能有this (也不能有super)
重载的方法需要同时为static的或者非static的
```java
public class Chinese {
	public Chinese(){
	    Chinese.count += 1;
	}
	static String country;  //类变量不用实例化，直接类名.属性名就可以使用。是类的一部分，被所有这个类的实例化对象所共享，也叫静态变量
	public static int count; // 计数一共被new了多少次
	String name;  // instance variable 实例变量， 只有实例化之后才能使用，属于实例化对象的一部分，不能共用
	int age;
	
	public static void test() {
		System.out.println("这是一个静态方法");
	}
	public static void showCount(){
	   System.out.println("总共new了" + Chinese.count + "个中国人对象" )
	}
}

Chinese.country = "China";
Chinese.test();

//判断字符串不为空
//在未来开发中 可能会多次使用这样的判断，那么在大量次数的基础上看，就发现代码的重复很多，所以把这样的代码抽取到工具类做成一个方法
public class Utils {
	public static boolean isEmpty(String s){
		boolean flag = false;
		if(s != null && !s.equlas("")){
			flag = true;
		}
		return flag;
	}	
}
```
没有static的方法，变量被称为实例方法，变量，通过引用访问，先创建对象，具体的对象.xxx

带有static的方法是类的所有对象共有的，通过类名.xxx调用

带有static的方法当中不能直接访问实例变量和实例方法，因为实力变量和实例方法都需要对象的存在。而static的方法中是没有this的，也就是说当前对象是不存在的，自然也是无法访问当前对象的实例变量和实例方法。


final
===

在Java中声明类，属性和方法时，可使用关键字final来修饰，表示“最终”。
final 标记的类不能被继承。提高安全性，提高程序的可读性。【String 类， System 类， StringBuffer类】
final 标记的方法不能被子类重写【object类中的getClass()】
final 标记的变量(成员变量或局部变量）即称为常量。约定名称大写，必要时下划线连接，且只能被赋值一次【final标记的成员变量必须在声明的同时 或在每个构造方法中 或代码块中显式赋值，然后才能使用  final double PI=3.14;】




================================================================================



object:
object 类是所有类的根父类 （默认继承）[子类可以执行父类的方法] [父类可以接收子类实例 Object o = new Student()]
若形参为一个类 但是不确定是什么类 就可用Object
public boolean equals (Object obj) 比较是不是指向同一个instance
public int hashCode()  取得Hash码
public String toSting()  返回的是对象内存地址 

hashCode()
我觉得hashcode可以看作是人的名字，人名相同的不一定是同一个人
默认情况下，hashCode方法是将对象的存储地址进行映射。
hashCode()方法和equals()方法都是Object类中的方法，不过hashCode()方法是一个native方法，它的返回值默认与System.identityHashCode(object)一致。这个值是对象头的一部分二进制位组成的数字，这个数字具有一定的标识对象的意义所在，但绝不等价于地址。
不同的对象可能会生成相同的hashcode值。虽然不能根据hashcode值判断两个对象是否相等，但是可以直接根据hashcode值判断两个对象不等，如果两个对象的hashcode值不等，则必定是两个不同的对象。如果要判断两个对象是否真正相等，必须通过equals方法。
hashCode简单理解为对象的标识，一般用于hash算法中，这样就可以在查找数据的时候根据这个key快速的缩小数据范围。hashCode与equals似乎是天生一对，一个未来算法快速定位数据而存在，一个是为了对比真实值而存在。但是不能说hashCode是唯一的，不同对象的hashCode值可能会相同。

　　也就是说对于两个对象，如果调用equals方法得到的结果为true，则两个对象的hashcode值必定相等；

　　如果equals方法得到的结果为false，则两个对象的hashcode值不一定不同；

　　如果两个对象的hashcode值不等，则equals方法得到的结果必定为false；

　　如果两个对象的hashcode值相等，则equals方法得到的结果未知。
  
  在重写equals方法的同时，必须重写hashCode方法. （在hashMap等集合的查找过程中是这样处理的，首先会对比根据hashCode值确定对应的桶，然后在桶里查找equals相同的对象，因此hashMap的查找速率比List要快。试想重写了equals()的两个对象相同，但没有覆写hashCode方法，很有可能会造成两个对象不再一个桶，这样map中的contains方法在查找的时候就找不到在其他桶的相同的对象了）
  
只要equals方法的比较操作用到的信息没有被修改，那么对这同一个对象调用多次，hashCode方法必须始终如一地返回同一个整数。
  
  final： 最终
  可修饰 类（不能被继承），属性（不能被改变，必须在定义的时候就显式赋值）【常量，全大写】，方法（不能被重写）
  
  
  
  ==================================================================================
  
  
  Java 异常：
  用于处理非预期的情况，如文件没找到，网络错误，非法的参数
  解决方法： 终止程序的运行；程序员在编写程序时就考虑到错误的检测.错误消息的提示，以及错误的处理
  Error： JVM系统内部错误（StackOverFlowError, OutOfMemoryError）， 资源耗尽等严重情况
  Exception(IOException, RuntimeException)： 其他编程错误或偶然的外在因素导致的一般性问题，如空指针访问（指向空），试图读取不存在的文件，网络连接中断
  
  异常处理机制： 防止程序中断
  捕获 try{
  可能出现异常的代码段
  }catch(Excetion e){
  当不知道捕获的是什么类型的异常时，可以直接使用所有异常的父类Exception
  e.printStackTrace(); 打印异常的类型
  System.out.println(e.getMessage())  打印异常信息
  }catch{
  可多个捕获  （在捕获异常的代码块try中， 如果前面的代码有异常了，就不会执行后面的）
  finally{ 可选
  最终会执行的操作 
  }
  
  
  
  
  抛出
  throws
  throws抛出的异常，在调用方法去处理（try catch). main方法抛出的异常直接抛到虚拟机上去了，就在程序中不能处理
  
  子类不能抛出比父类还大的异常类型
  
  ===========================================================================
  
  集合：用来存放对象的容器， 存放的是对象的引用
  集合可以存放不同类型，不限数量的数据类型
  Java集合可分为Set, List, Map 三大体系
  Set：无序，不可重复的集合
  List：有序，可重复的集合
  Map: 具有映射关系的集合
  
  ==============================================================================
  
  HashSet 是Set接口的典型实现，大多数时候使用Set集合使用的都是这个实现类
  HashSet按Hash算法来存储集合中的元素
  HashSet不能保证元素的排列顺序，不可重复，不是线程安全的，集合元素可以使用null
  当向HashSet集合中存入一个元素是，HashSet会调用该对象的hashCode()方法来得到该对象的hashCode值，然后根据hashCode值决定该对象在HashSet中的存储位置 （存在set集合的哪个位置由这个值的hashcode决定； 如果两个元素的equals()方法返回true,但它们的hashCode() 返回值不相等，依然可以添加成功，但会把它们存储在不同的位置。
  HashSet集合判断两个元素相等的标准：两个对象通过equals()方法比较相等，并且两个对象的hashCode()方法返回值也相等
  遍历HashSet:
  Set set = new HashSet();  //等价于 Set<Object> set = new HashSet<Object>(); 
  set.add(1);
  set.add("a");
  set.add(null);
  System.out.println(set);
  Iterator it = set.iterator();
  while(it.hasNext()){
  it.next();
  }
  
  for(Object obj : set){
    ob;
  }
  
  泛型： 如果想要让集合中只能存同样类型的对象，使用泛型          <>
  Set <String> set1 = new HashSet<String>();   //比如指定String为
    
=============================================================================

    TreeSet
    
    TreeSet是SortedSet接口的实现类，treeSet可以确保集合元素处于排序状态（自然排序/定制排序）。 默认情况下采用自然排序
    Set<Integer> set = new TreeSet<Integer>();
    set.add(5);
    set.add(2);
    System.out.println((set); // [2,5]
    仍然可使用iterator或foreach遍历
    
    自然排序：TreeSSet会调用集合元素的compareTo(Object obj) 方法来比较元素之间的大小关系，然后将集合元素按升序排序。
    必须放入同样类的对象。（默认会进行排序）否则可能会发生类型转换异常。我们可以使用泛型来进行限制
    
    
    定制排序： 
    ```java
    Person p1 = newPerson("zhangsan",23);
    Person p2 = newPerson("lisi",20);
    Person p3 = newPerson("wangwu",16);
    Person p4 = newPerson("maliu",29);
    
    Set<Person> set = new TreeSet<Person>(new Person());
    set.add(p1);
    set.add(p2);
    set.add(p3);
    set.add(p4);
    
    如果我们要在TreeSet里面放Person Object
    Comparator 接口  Person泛型
    class Person implements Comparator <Person>{
        int age;
        String name;
        public Person(){
        
        }
        public Person(String name, int age){
            this.name = name;
            this.age = age;
        }
        
        @override
        public int compare(Person o1, o2) {
            if(o1.age > o2.age) {
                    return 1;
                }else if(o1.age < o2.age){
                    return -1;
                }else{
                    return 0;
                }
            }
        }
    ```
  =============================================================================
  
List： 元素有序，可重复， 每个元素都有索引，默认按元素的添加顺序设置元素的索引，可以通过索引来访问指定位置的集合元素; List集合里添加了一些根据索引来操作集合元素的方法

```java
//实现类可以赋给接口
List<String> list = new ArrayList<String>();
list.add("b"); index 0 
list.add("d"); index 1
list.add("a");
list.add("b";
list.get(2); // a
list.add(1, "f"); // 在指定位置插入元素 [b,f,d,a,b]  List集合里添加了一些根据索引来操作集合元素的方法

List<String> l = new ArrayList<String>();
l.add("123");
l.add("456");

list.addAll(2, l); // 在指定位置插入集合

System.out.println(list); // [b,f,123,456,d,a,b]

list.indexOf("b");  // 第一次出现的下标
list.lastIndexOf("b")  // 最后一次出现的下标

list.remove(2); // 根据指定的索引移除元素

list.set(1, "ff"); // 根据指定的索引修改元素

List<String> sublist = list.subList(2, 4); // [list[2], list[3]]

list.size() //长度



ArrayList 和 Vector 是 List 接口的两个典型实现
Vector是线程安全的，但是一个古老的集合，ArrayList是线程不安全的，但总推荐使用ArrayList
```

==========================================================================

Map接口
Map用于保存具有映射关系的数据，Map集合里保存着两组值，一组值用于保存Map里的Key，另一组用于保存Map里的Value
Map中的Key Value可以是任何引用类型数据
Key不允许重复， 即同一个Map对象的任何两个Key通过eaules方法比较总返回false
Key和Value之间存在单向一对一关系， 即通过指定的Key总能找到唯一的，确定的Value

实现类 HashMap

```java
Map<String, Integer> map = new HashMap<String, Integer>();
map.put("b", 1);
map.put("c", 2);
map.put("e", 2);
System.out.println(map); // {b=1,c=2,e=2}

map.get("b"); // 1  根据key取值
map.remove("c") //  {b=1,e=2} 根据key移除键值对
map.size(); // 2  map 集合的长度

map.containsKey("b"); // 判断当前集合是否包含指定的key
map.containsValue(10); // 判断当前集合是否包含指定的Value

map.clear() // 清空集合

// 遍历map集合
Set<String> keys = map.keySet(); // 获取map集合的key集合

map.values(); //获取集合的所有value值

for(String key : keys) {
    key: key, valeu: map.get(key)
}


// 通过map.entrySet();  set 里面套一个 Entry
Set<Entry<String, Integer>> entrys = map.entrySet();
for(Entry<String, Integer> en : entrys) {
    key: en.getKey(),   value: en.genValue()
}
```

HashMap 和 Hashtable是Map 接口的两个典型实现类
Hashtable古老，不推荐使用， 不允许使用 null 作为key和value, 但是线程安全
HashMap 允许使用null作为key和value 线程不安全

与HashSet集合不能保证元素的顺序一样， Hashtable, HashMap 也不能保证其中key-value对的顺序

Hashtable, HashMap 判断两个key 相等的标准是： 两个Key通过equals 方法返回true, hashCode值也相等



TreeMap
TreeMap存储key-value对时,需根据key对key-value对进行排序。TreeMap 可以保证所有的key-value对处于有序状态。
自然排序(alphebetic)： TreeMap的所有的key必须实现comparable接口，而且所有的key应该是同一类的对象，否则会抛出ClassCastException
定制排序： 创建TreeMap时，传入一个Comparator对象，该对象负责对TreeMap中的所有key进行排序（一般用不到）

```java
Map<Integer, String> map = new TreeMap<Integer, String>();
map.put(4, "a");
map.put(2, "a");
map.put(3, "a");
map.put(1, "a");

System.out.println(map); // {1=a, 2=a, 3=a, 4=a}

Map<String, String> map1 = new TreeMap<String, String>();
map.put("b", "a");
map.put("c", "a");
map.put("da", "a");
map.put("a", "a");

System.out.println(map1); // {a=a, b=a, c=a, da=a}
'''

=================================================================================


操作集合的工具类 ： Collections

Collections 是一个操作 Set, List 和 Map等集合的工具类
Collections中提供了大量方法对集合元素进行排序，查询和修改等操作，还提供了对集合对象设置不可变，对集合对象实现同步控制等方法
排序操作：
reverse(List): 反转List元素的顺序
shuffle(List): 对List集合元素进行随机排序
sort(List): 根据元素的自然顺序 对指定List集合元素按升序排序
sort(List, Comparator): 根据指定的Comparator产生的顺序对List集合元素进行排序
swap(List, int, int): 将指定list集合中的i处元素和j处元素进行交换
Object max(Collection): 根据元素的自然顺序，返回集合中的最大元素
Object max(Collection, Comparator): 根据Comparator指定的顺序，返回给定集合中的最大元素
Object min(Collection)
Object min(Collection, Comparator)
int frequency(Collection, Object): 返回指定集合中指定元素的出现次数
boolean replaceAll(List list, Object oldVal, Object newVal): 使用新值替换List对象的所有旧值


同步控制：
Collections类中提供了多个synchronizedXXX()方法，可将指定集合包装成线程同步的集合，从而解决多线程并发访问集合时的线程安全问题

```java
List<String> list = new ArrayList<String>();
list.add("b");
list.add("a");
list.add("1");

Collections.reverse(list); // [1, a, b]
Collections.shuffle(list); // [a,1,b]
Collections.sort(list);  // [1, a, b]
Collections.max(list); // b
Collections.min(list); // 1

list.add("a");
Collections.frequency(list, "a"); // 2

Collections.replaceAll(list, "a", "aa");  // [1, aa, b, aa]

Student s1 = new Student (14, "zhangsan");
Student s2 = new Student (12, "lisi");
Student s3 = new Student (13, "wangwu");
Student s4 = new Student (11, "xiaoliu");
List<Student> stus = new ArrayList<Student>();
stus.add(s1);
stus.add(s2);
stus.add(s3);
stus.add(s4);

for(Student stu : stus){
    System.out.println(stu.name + "," + stu.age);
}


Collections.sort(stus, new Student());  // 第二个参数是无参构造

Collection.max(stus, new Student()); // zhangsan 14



for(Student stu : stus){
    System.out.println(stu.name + "," + stu.age);
}


Collections.swap(list, 0, 2);


class Student implements Comparator<Student>{
	int age;
	String name;
	
	public Student(){
		
	}
	
	public Student(int age, String name){
		this.age = age;
		this.name = name;
	}
	
	@override
	public int compare(Student o1, Student o2) {
		if(o1.age > o2.age){
			return 1;
		else if(o1.age < o2.age){
			retuen -1;
		}else{
			return 0;
		}
	}



```


  
  
