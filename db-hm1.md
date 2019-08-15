Trigger
===

###
 建触发器，工资插入的时候触发，触发内容为将插入的工资额度+1000
``` sql
DELIMITER $$
CREATE TRIGGER insert_salary BEFORE INSERT
ON salary
FOR EACH ROW
BEGIN
  SET NEW.sal = NEW.sal + 1000;
END $$
DELIMITER;

```
事务
===
一个事务是一连串的操作组成，增删改查的集合。就好比java方法一样，java方法中有N条语句对属性进行修改。在java执行语句中，有一个语句执行出现问题而导致抛出异常了，那么相当于这个方法只执行了一半。如果只执行了一半，那么它的一致性很有可能遭到了破坏。所以原子性就要求你一个java方法要么你就全部执行成功，要么你就不执行，我不允许你只执行一半。所以原子性是一致性的必要条件，但是只要保证了原子性就可以保证一致性了吗？显然不是，所以原子性是一致性的一个必要条件，但是不充分条件。

好的，这时候我们假设原子性保证了。一个事务未提交的时候，发生了错误就执行rollback，那么事务就不会提交了。但是当我们事务执行成功了，执行commit指令之后，遇到了错误会怎么样？我们都知道，执行commit后会让事务刷盘，进行持久化操作。进行刷盘操作时是需要一定时间的，在这个刷盘过程中出现宕机、停电、系统崩溃等等可以中断刷盘的操作，那么这个过程会不会有一半数据刷盘成功，另一半没有刷进去？当然不会出现，持久性就保证了，但你当你提交事务commit之后，它一定会持久化到数据库中。尽管你写了一半的数据到数据库中，然后数据库宕机了，当你下一次重启的时候，数据库根据提交日志进行回滚，将另一半的数据写入，至于是如何操作的，这是一个非常复杂而艰难的过程，伟大的oracle程序员已经为我们做好了，我们就不用操心了。 

原子性，持久性他们两个都是事务一致性的充分条件，但是还无法构成必要条件。这两个都解决了，接下来就是线程安全的问题了。数据库是支持并发访问，这是毫无疑问的。隔离性，官方解释是说“在并发事务下，多个事务有自己的事务空间，相互独立互不干扰。我们在一个事务中是感觉不到其他并发事务的存在”。以java方法来说的话，不同的方法都有自己的栈帧，虽然多个线程调用同一个方法，但是不同进程，进入这个方法的栈帧是相互独立的。如果你在这个类中定义了成员变量，线程在工作时，会将主内存中的数据拷贝到工作内存的栈帧中。这样对数据的任何操作都是基于工作内存(效率提高)，并且不能直接操作主内存以及其他线程工作内存中的数据，之后再将更新之后的数据刷新到主内存中。这种肯定会引起线程安全问题，解决线程安全问题，通常来说就是加锁，java为我们提供了不同锁粒度的锁，来供我们用户自己选择。我们可以再变量上加入volitate来实现内存可见性，还可以加入synchronized实现同步方法块。

回到隔离性，很显然事务的隔离性，说的就是，多个并发事务实际上都是独立事务上下文，多个事务上下文之间彼此隔离，互补干扰。但是多个事务如果对共享数据进行查看，删除，修改如果不加以修改，就会出现线程安全问题。如何避免，你可能会使用一把锁，当线程A修改共享数据的时候，让线程B不要来查看共享数据，除非等我修改完毕；或者说，我不让别人读取到我修改共享数据的中间值，只能读取到初始值，和我修改完成之后的值。但是这些都是你根据业务来操作的，所以数据库为我们封装了‘四把锁’，对应四种隔离级别：

读未提交，其隔离级别最低，允许脏读。换句话说就是，如果一个事务正在处理某一数据，并对其进行了更新，但是同时没有提交事务，允许另一个事务也可以访问
读已提交，和读未提交的区别就是。读未提交可以读取到别人没有提交的数据，但是读已提交只能读取到别人提交后的值，事务进行的中间值不会读取到
可重复读，简单来说就是事务处理过程中多次读取同一个数据的时候，这个值不会发生改变，其值都和第一次查询到的数据是一致的
串行化，是最严格的隔离级别，他要求所有的事务都被串行执行，既事务只能一个接一个的进行处理，不能并发执行
这样原子性、持久性、隔离性(锁机制以及各种并发安全控制机制)和事务一致性就构成了充分必要条件

Atomicity（原子性）：一个事务（transaction）中的所有操作，或者全部完成，或者全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。即，事务不可分割、不可约简。
Consistency（一致性）：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设约束、触发器、级联回滚等。
Isolation（隔离性）：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括未提交读（Read uncommitted）、提交读（read committed）、可重复读（repeatable read）和串行化（Serializable）。
Durability（持久性）：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

存储过程
===
存储过程
存储过程（Stored Procedure）是在大型数据库系统中，一组为了完成特定功能的SQL 语句集，经编译后存储在数据库中，用户通过指定存储过程的名字并给出参数（如果该存储过程带有参数）来执行它。
###
建一个存储过程， 执行 插入工资并判断是否重复（日期和员工id来判断）
```sql
drop procedure if exists check_salary_repeat;
 
create procedure check_salary_repeat(insertempno int,insertdate datetime,sal float)
begin
    if (insertempno not in (select empno from salary where salary.senddate=insertdate))
    then
    insert into salary values(insertempno, insertdate, sal);
    END IF;
end;
 
call check_salary_repeat(7788, '2019-08-11',2000.00);

```
### 
mysql basic operations
https://blog.csdn.net/bulrush__xiao/article/details/52847941
###
common data types
int, float, decimal(小数值, 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2; 其范围依赖于D,M的值), varchar(变长字符串), text(长文本数据), datetime(YYYY-MM-DD HH:MM:SS)
```sql
create table employee
(
	id int,
	name varchar(40),
	sex char(4),
	birthday date,
	Entry_date date,
	job varchar(100),
	salary Decimal(8,2),
	resume Text
);
```
###
插入数据
```sql
1.插入数据
insert into employee(id,name,sex,birthday,entry_date,salary,resume) values(1,'zhangsan','male','1993-03-04','2016-11-10','1000','i am a developer');
//可以省略表字段，但是必须插入全部字段
insert into employee values(null,null,'male','1993-03-04','2016-11-10','1000','i am a developer');
//无法插入
insert into employee values('male','1993-03-04','2016-11-10','1000','i am a developer');
//可以包起来所有类型
insert into employee values('5',null,'male','1993-03-04','2016-11-10','1000','i am a developer');
//指定某些列插入数据
insert into employee(id) values(6);
```
