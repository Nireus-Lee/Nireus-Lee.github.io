1、@Table(name="T_X")这句话可以不写，不写就已类名作为表名 

2、如果想让两个类的属性生成一个数据表，在一个类里这样加入另一个类即可：	@Embedded 
private C c; 

3、如果想要一个类继承另一个类的所有属性，则在父类里这样写： 
@SuppressWarnings("serial") 
@Entity 
@MappedSuperclass   //增加这一行 
并把父类的所有属性的private改为protected即可 

4、建议在一对多关联中在"一"方用延迟加载"多"方可以在HQL中显式的"迫切左外连接" left join fetch 这样做Hibernate可以少访问数据库,也可以用"@BatchSize(size = 5)"来减少访问数据库的次数 


1. @Id 声明属性为主键 


2. @GeneratedValue表示主键是自动生成策略，一般该注释和 @Id 一起使用 


3. @Entity 任何 hibernte 映射对象都要有次注释 


4. @Table(name = “tablename”) 类声明此对象映射到哪个表 


5. @Column(name = “Name”,nullable=false,length=32) 声明数据 库字段和类属性对应关系 


6. @Lob 声明字段为 Clob 或 Blob 类型 



7. @OneToMany(mappedBy=”order”,cascade = CascadeType.ALL, fetch = FetchType.LAZY) 
   @OrderBy(value = “id ASC”) 
   一对多声明，和 ORM 产品声明类似，一看就明白了。 
   @ManyToOne(cascade=CascadeType.REFRESH,optional=false) 
   @JoinColumn(name = “order_id”) 
   声明为双向关联 


8. @Temporal(value=TemporalType.DATE) 做日期类型转换。 



9. @OneToOne(optional= true,cascade = CascadeType.ALL, mappedBy = “person”) 
   一对一关联声明 
   @OneToOne(optional = false, cascade = CascadeType.REFRESH) 
   @JoinColumn(name = “Person_ID”, referencedColumnName = “personid”,unique = true) 
   声明为双向关联 



10. @ManyToMany(mappedBy= “students”) 
   多对多关联声明。 
  @ManyToMany(cascade = CascadeType.PERSIST, fetch = FetchType.LAZY) 
  @JoinTable(name = “Teacher_Student”, 
    joinColumns = {@JoinColumn(name = “Teacher_ID”, referencedColumnName = “teacherid”)}, 
    inverseJoinColumns = {@JoinColumn(name = “Student_ID”, referencedColumnName = 
    “studentid”)}) 
   多对多关联一般都有个关联表，是这样声明的！ 



11. @Transiten表示此属性与表没有映射关系，是一个暂时的属性 



12. @Cache(usage= CacheConcurrencyStrategy.READ_WRITE)表示此对象应用缓存 


JPA规范 
@Entity：通过@Entity注解将一个类声明为一个实体bean 

@Table：通过 @Table注解可以为实体bean映射指定表，name属性表示实体所对应表的名称，如果没有定义 @Table，那么系统自动使用默认值：实体的类名（不带包名） 

@Id：用于标记属性的主键 

@Column：表示持久化属性所映射表中的字段，如果属性名与表中的字段名相同，则可以省略@Column注解，另外有两种方式标记，一是放在属性前，另一种是放在getter方法前，例如： 

@Column(name = "EMPLOYEE_NAME") 

private String employee_name; 或者 

@Column(name = "EMPLOYEE_NAME") 

public String getEmployee_name() { 

return employee_name; 

} 这两种方式都是正解的，根据个人喜好来选择。大象偏向于第二种，并且喜欢将属性名与字段名设成一样的，这样可以省掉@Column注解，使代码更简洁。 



@Temporal(TemporalType.DATE)：如果属性是时间类型，因为数据表对时间类型有更严格的划分，所以必须指定具体时间类型，如④所示。在javax.persistence.TemporalType枚举中定义了3种时间类型： 

通过 @Temporal 定义映射到数据库的时间精度： 
@Temporal(TemporalType.DATE)       日期 
@Temporal(TemporalType.TIME)       时间 
@Temporal(TemporalType.TIMESTAMP) 两者兼具 

                  

@Temporal只是起映射作为 



@Transient   

@Target({METHOD, FIELD}) @Retention(RUNTIME) 

public @interface Transient {} 

指明一个属性或方法不能持久化 





@TableGenerator：表生成器，将当前主键的值单独保存到一个数据库表中，主键的值每次都是从指定的表中查询来获得，这种生成主键的方式是很常用的。这种方法生成主键的策略可以适用于任何数据库，不必担心不同数据库不兼容造成的问题。大象推荐这种方式管理主键，很方便，集中式管理表的主键，而且更换数据库不会造成很大的问题。各属性含义如下：


name：表示该表主键生成策略的名称，这个名字可以自定义，它被引用在@GeneratedValue中设置的"generator"值中 

table：表示表生成策略所持久化的表名，说简单点就是一个管理其它表主键的表，本例中，这个表名为GENERATOR_TABLE 

pkColumnName：表生成器中的列名，用来存放其它表的主键键名，这个列名是与表中的字段对应的 

pkColumnValue：实体表所对应到生成器表中的主键名，这个键名是可以自定义滴 

valueColumnName：表生成器中的列名，实体表主键的下一个值，假设EMPLOYEE表中的EMPLOYEE_ID最大为2，那么此时，生成器表中与实体表主键对应的键名值则为3 

allocationSize：表示每次主键值增加的大小，例如设置成1，则表示每次创建新记录后自动加1，默认为50
