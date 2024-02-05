# <center>HIBERNATE</center> 
<hr>
_Refer to [Tutorials Point](https://www.tutorialspoint.com/hibernate/index.htm)_

ORM is a programming technique for converting data between relational databases and object oriented programming languages such as Java, C#, etc.

Hibernate is an Object-Relational Mapping (ORM) solution for JAVA. It is an open source persistent framework created by Gavin King in 2001.

### Advantages

1. Hibernate takes care of mapping Java classes to database tables using XML files and without writing any line of code.
2. Provides simple APIs for storing and retrieving Java objects directly to and from the database.
3. Hibernate does not require an application server to operate.
4. Manipulates Complex associations of objects of your database.
5. Minimizes database access with smart fetching strategies.
6. Provides simple querying of data.

Hibernate uses JDBC API internally and other like JTA(Java Transaction API) for managing transactions and JNDI(Java Naming and Directory Interface) for accessing resources.

## Configuration Object

The Configuration object is the first Hibernate object you create in any Hibernate application. It is usually created only once during application initialization. It represents a configuration or properties file required by the Hibernate.
The Configuration object provides two keys components −

1. Database Connection − This is handled through one or more configuration files supported by Hibernate. These files are hibernate.properties and hibernate.cfg.xml.

2. Class Mapping Setup − This component creates the connection between the Java classes and database tables

## SessionFactory Object

Configuration object is used to create a SessionFactory object which in turn configures Hibernate for the application using the supplied configuration file and allows for a Session object to be instantiated.

The SessionFactory is a thread safe object and used by all the threads of an application.

The SessionFactory is a heavyweight object; it is usually created during application start up and kept for later use. You would need one SessionFactory object per database using a separate configuration file. So, if you are using multiple databases, then you would have to create multiple SessionFactory objects.

**Methods of SessionFactory Object**

```java
/ Creating a Configuration instance and configuring Hibernate
Configuration configuration = new Configuration().configure("hibernate.cfg.xml");

// Building the SessionFactory
SessionFactory sessionFactory = configuration.buildSessionFactory();

// Using the SessionFactory to create sessions and interact with the database
Session session = sessionFactory.openSession();
```

**How to get SessionFactory Object**

```java
//SessionFactory Object is created
SessionFactory sessionFactory = new Configuration().configure("hibernate.cfg.xml").buildSessionFactory();

//SessionFactory Object with class mapping
SessionFactory sessionFactory = new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Employee.class).buildSessionFactory();
```

## Session Object

A Session is used to get a physical connection with a database. The Session object is lightweight and designed to be instantiated each time an interaction is needed with the database. Persistent objects are saved and retrieved through a Session object.

The session objects should not be kept open for a long time because they are not usually thread safe and they should be created and destroyed them as needed.

**How to get Session Object**

```java
//Session Object is created
Session session = sessionFactory.openSession();
```

**Methods of Session Object**

```java
//Create Entity Object with values
Employee entity = new Employee(101,"Sagar","Mumbai");

//Save Entity Object in Database
session.save(entity);

//Get Entity Object from Database
Employee entity = session.get(Employee.class, 101);

//Update Entity Object in Database
session.update(entity);

//Delete Entity Object from Database
session.delete(entity);

//Create a Query Object for executing HQL(Hibernate Query Language)
Query query = session.createQuery("FROM Employee WHERE id = :id");
query.setParameter("id", 101);
Employee entity = (Employee)query.uniqueResult();
List<Employee> list = query.list();

//Forces any pending changes to be written to the database
session.flush();

//Detaches all objects from the session
session.clear();

//To create a transaction
Transaction transaction = session.beginTransaction();

//To get the current transaction
Transaction transaction = session.getTransaction();

//Cancel the execution of the current query
session.cancelQuery();

//Close the connection
session.close(); // releases the connection (return Connection object)

//Create a new Criteria instance, for the given entity class
Criteria criteria = session.createCriteria(Employee.class);

//Create a new Criteria instance, for the given entity class
Criteria criteria = session.createCriteria("Employee");

//Return the identifier value of the given entity as associated with this session
Serializable id = session.getIdentifier(entity);

//Create a new instance of Query for the given collection and filter string
Query query = session.createFilter(collection, "filterString");

//Create a new instance of Query for the given HQL query string
Query query = session.createQuery("hqlString");

//Create a new instance of SQLQuery for the given SQL query string
SQLQuery query = session.createSQLQuery("sqlString");

//Remove a persistent instance from the datastore
session.delete("entity", entity);

//Return the persistent instance of the given named entity with the given identifier, or null if there is no such persistent instance
Serializable id = session.getIdentifier("entity",Serializable Identifier);

//Get the session factory which created this session
SessionFactory factory = session.getSessionFactory();

//Re-read the state of the given instance from the underlying database
session.refresh(entity);

//Check if the session is currently connected
boolean connected = session.isConnected();

//Does this session contain any changes which must be synchronized with the database?
boolean dirty = session.isDirty();

//Check if the session is still open
boolean open = session.isOpen();

//Persist the given transient instance, first assigning a generated identifier
Serializable object = session.save(entity);

//Either save(Object) or update(Object) the given instance
session.saveOrUpdate(entity);

//Update the persistent instance with the identifier of the given detached instance
session.update(entity);

//Update the persistent instance with the identifier of the given detached instance
session.update("entity", entity);
```

## Transaction Object

Transactions in Hibernate are handled by an underlying transaction manager and transaction (from JDBC or JTA).

This is an optional object and Hibernate applications may choose not to use this interface, instead managing transactions in their own application code.

**How to get Transaction Object**

```java
//Transaction Object is created
Transaction transaction = session.beginTransaction();

//Get Transaction Object from Session
Transaction transaction = session.getTransaction();
```

**Methods of Transaction Object**

```java
//Commit Transaction
transaction.commit();

//Rolls back the transaction, discarding any changes made within the transaction scope
transaction.rollback();

//Checks if the transaction is active (hasn't been committed or rolled back)
boolean valid = transaction.isActive(); // returns true or false

//Sets a timeout value for the transaction. If the transaction takes longer than this specified time, it may be automatically rolled back
transaction.setTimeout(60); //in seconds

//Sets the isolation level for the transaction. Isolation levels define the degree of isolation between transactions concerning data visibility
transaction.setIsolationLevel(Connection.TRANSACTION_READ_COMMITTED);
```

## Query Object

Query objects use SQL or Hibernate Query Language (HQL) string to retrieve data from the database and create objects. A Query instance is used to bind query parameters, limit the number of results returned by the query, and finally to execute the query.

**How to get Query Object**

```java
//Query Object is created
Query query = session.createQuery("FROM Employee WHERE id = :id");
```

**Methods of Query Object**

```java
//Bind query parameters
query.setParameter("id", 101);

//Execute query
List<Employee> list = query.list();

//Execute query with named parameters
Query query = session.createQuery("FROM Employee WHERE id = :id");
query.setParameter("id", 101);
List<Employee> list = query.list();

//Sets the position of the first result to retrieve
query.setFirstResult(0); // 0 based index

//Sets the maximum number of results to retrieve
query.setMaxResults(10);  // returns 10 results

//Executes the query and returns a list of results
List<Employee> list = query.list();

//Executes the query and returns a single result
Employee employee = query.uniqueResult();

//Executes an update or delete query and returns the number of affected rows
int count = query.executeUpdate();

//Sets the fetch size
query.setFetchSize(10);

//Sets whether the query results should be cached or not
query.setCacheable(true);
```

## Criteria Object

Criteria objects are used to create and execute object oriented criteria queries to retrieve objects.

API provides an object-oriented way to perform queries without writing explicit HQL (Hibernate Query Language) or SQL queries. It allows you to build query criteria dynamically using the Criteria object and its methods.

**How to get Criteria Object**

```java
//Criteria Object is created
Criteria criteria = session.createCriteria(Employee.class);
```

**Methods of Criteria Object**

```java
// Adds restrictions to the query to filter results based on conditions
criteria.add(Restrictions.eq("id", 101)); //Example: property equals a specific value

//: Sets projections (selecting specific fields or aggregates) for the query.
criteria.setProjection(Projections.property("propertyName")); // Select specific property

//Adds sorting order to the query results
criteria.addOrder(Order.asc("propertyName")); // Ascending

//Set the maximum number of results to retrieve
criteria.setMaxResults(10); // returns 10 results

//Set the position of the first result to retrieve
criteria.setFirstResult(0); // 0 based index

//Executes the criteria query and returns a list of matching objects
List<Employee> list = criteria.list();

//Executes the criteria query and returns a single matching object
Employee employee = criteria.uniqueResult();

//Sets the fetch mode for associations
criteria.setFetchMode("association.property", FetchMode.JOIN);
```

```java

//Uses criteria queries with named parameters
Criteria criteria = session.createCriteria(YourEntityObject.class);

criteria.add(Restrictions.eq("propertyName", value)); // Add a restriction
criteria.addOrder(Order.asc("propertyName")); // Add sorting
criteria.setMaxResults(10); // Limit results to 10
criteria.setFirstResult(0); // Start from the first result

List<YourEntityObject> resultList = criteria.list(); // Execute the query and retrieve results
```

## Hibernate Configuration

File : `hibernate.cfg.xml`

```xml
<?xml version='1.0' encoding='UTF-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
          "-//Hibernate/Hibernate Configuration DTD 5.3//EN"
          "http://hibernate.sourceforge.net/hibernate-configuration-5.3.dtd">
<hibernate-configuration>
	<!-- Its a Session Factory object which is used to create session. We can
		do all the CRUD operations. -->
	<session-factory>
		<!-- To automatically validate and update database -->
		<property name="hbm2ddl.auto">update</property>
		<!-- MySQL Dialect : Developing optimized queries -->
		<property name="dialect">org.hibernate.dialect.MySQL5Dialect</property>
		<!-- MySQL Driver -->
		<property name="connection.driver_class">com.mysql.jdbc.Driver</property>
		<!-- MySQL Connection -->
		<property name="connection.url">jdbc:mysql://localhost:3306/hibernate</property>
		<property name="connection.username">root</property>
		<property name="connection.password">sagarm</property>
		<!-- To show query in console -->
		<property name="hibernate.show_sql">true</property>

		<mapping resource="Employee.hbm.xml" />
		<mapping class="com.oupp.hibernatejava.entity.Book" />

      <!-- If get any SessionError try this -->
      <property name="current_session_context_class">thread</property>

	</session-factory>
</hibernate-configuration>
```

File : `hibernate.properties`

```properties
hibernate.hbm2ddl.auto=update
hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
hibernate.connection.driver_class=com.mysql.jdbc.Driver
hibernate.connection.url=jdbc:mysql://localhost:3306/hibernate
hibernate.connection.username=root
hibernate.connection.password=sagarm
hibernate.show_sql=true
```

## Mapping Classes

Create a pojo class and map it with hbm.cfg file.

```java
@Entity
public class Employee {
   private int id;
   private String name;
   private String city;
}
```

File : `Employee.hbm.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN" "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
   <class name="com.oupp.hibernatejava.entity.Employee" table="employee">
      <!-- Primary Key -->
      <id name="id" type="int" column="id">
         <!-- Id generator type : native, sequence, hilo, table, uuid, auto, identity, custom -->
         <generator class="native" />
      </id>
      <!-- Properties -->
      <property name="name" type="string" column="name"/>
      <property name="city" type="string" column="city"/>
   </class>
</hibernate-mapping>
```

Types of Generators

- native
- sequence
- hilo
- table
- uuid
- auto
- identity
- custom

Type of datatypes for maping
Mapping Type | Java Type | ANSI SQL Type
------------ | --------- | -------------
integer | int or java.lang.Integer | INTEGER
long | long or java.lang.Long | BIGINT
float | float or java.lang.Float | FLOAT
double | double or java.lang.Double | DOUBLE
boolean | boolean or java.lang.Boolean | BIT
yes/no | boolean or java.lang.Boolean | CHAR(1) ('Y'/'N')
true/false | boolean or java.lang.Boolean | CHAR(1) ('T'/'F')
string | java.lang.String | VARCHAR
short | short or java.lang.Short | SMALLINT
byte | byte or java.lang.Byte | TINYINT
date | java.sql.Date | DATE
time | java.sql.Time | TIME
timestamp | java.sql.Timestamp | TIMESTAMP
binary | byte[] | BLOB

### Example

```java
@Entity
public class Employee {
   private int id;
   private String name;
   private String city;
   //Generate Getters and Setters and Constructor
}
```

```java
public class TestEmployee {
   public static void main(String[] args) {
      //SessionFactory Object is created
      SessionFactory sessionFactory = new Configuration().configure("hibernate.cfg.xml").buildSessionFactory();
      //Session Object is created
      Session session = sessionFactory.openSession();
      //Transaction Object is created
      Transaction transaction = session.beginTransaction();

      //Employee Object is created
      Employee employee = new Employee(101, "Sagar", "Nashik");

      //Save Entity to Database
      int id = (int)session.save(employee); //returns id of inserted record
      //Commit Transaction
      transaction.commit();

      //Retrieve all records
      Query query = session.createQuery("FROM Employee");
      List<Employee> list = query.list();

      //Or we can write in single line
      //List<Employee> list = session.createQuery("FROM Employee").list();

      //Update a record
      employee = session.get(Employee.class, id);
      employee.setCity("Pune");
      session.update(employee);
      transaction.commit();

      //Delete a record
      employee = session.get(Employee.class, id);
      session.delete(employee);
      transaction.commit();
   }
}
```

**Execution Process**

- Create hibernate.cfg.xml configuration file as explained in configuration chapter.
- Create Employee.hbm.xml mapping file as shown above.
- Create Employee.java source file as shown above and compile it.
- Create ManageEmployee.java source file as shown above and compile it.
- Execute ManageEmployee binary to run the program.

## O/R Mapping

### Collection Mapping

Hibernate can persist instances of java.util.Map, java.util.Set, java.util.SortedMap, java.util.SortedSet, java.util.List, and any array of persistent entities or values.

```xml
<set> : Set, SortedSet, HashSet
<list> : List, ArrayList
<bag> / <ibag> : Collection, ArrayList
<map> : Map, HashMap, SortedMap, TreeMap
<premitive-array> : All the primitive types
<array> : Object
```

### Association Mapping

The mapping of associations between entity classes and the relationships between tables. An association mapping can be unidirectional as well as bidirectional.

There four two types of association mapping

- One-to-one
- One-to-many
- Many-to-one
- Many-to-many

### Component Mapping

Mapping for a class having a reference to another class as a member variable.

## Hibernate Annotations

Hibernate annotations are the newest way to define mappings without the use of XML file. You can use annotations in addition to or as a replacement of XML mapping metadata.

```java
@Entity //to define class as entity class or bean
@Table(name="employee" ) //to define table name by default name is class name
public class Employee {
   @Id //Make id as primary key
   @GeneratedValue(strategy=GenerationType.AUTO)
   @Column(name="emp_id") //to define column name by default name is field name
   private int id;
   @Column(length=50) //to define column length
   private String name;
   @Column(nullable=false, unique=true) //to make column not null and unique
   private String city;
   @Transient //to make the field not persist in database
   private int age;

   //Generate Getters and Setters and Constructor
}
```

**Example of Annotations**

```java
public class TestEmployee {
   public static void main(String[] args) {
      //SessionFactory Object is created
      SessionFactory sessionFactory = new AnnotationConfiguration().configure("hibernate.cfg.xml").addAnnotatedClass(Employee.class).buildSessionFactory();
      //Session Object is created
      Session session = sessionFactory.openSession();
      //Transaction Object is created
      Transaction transaction = session.beginTransaction();

      //Employee Object is created
      Employee employee = new Employee("Sagar", "Nashik", 25);

      //Save Entity to Database
      session.save(employee);
      //Commit Transaction
      transaction.commit();

      //Add your codes as required
   }
}
```

## Hibernate Query Language (HQL)

Hibernate Query Language (HQL) is an object-oriented query language, similar to SQL, but instead of operating on tables and columns, HQL works with persistent objects and their properties. HQL queries are translated by Hibernate into conventional SQL queries, which in turns perform action on database.

**FROM :**
To load a complete persistent objects into memory.

```java
String hql = "FROM Employee";
Query query = session.createQuery(hql);
List<Employee> list = query.list();
```

**AS :**
To assign aliases to the classes in your HQL queries, especially when you have the long queries.

```java
String hql = "FROM Employee AS emp";
Query query = session.createQuery(hql);
List<Employee> list = query.list();
```

AS keyword is optional and you can also specify the alias directly after the class name.

```java
tring hql = "FROM Employee emp";
Query query = session.createQuery(hql);
List<Employee> list = query.list();
```

**SELECT :**
If you want to obtain few properties of objects instead of the complete object, use the SELECT clause.

```java
String hql = "SELECT emp.name FROM Employee emp"; //retrinve name from employee table
Query query = session.createQuery(hql);
List<String> list = query.list(); //List of Employee Names from employee table
```

**WHERE :**
If you want to narrow the specific objects that are returned from storage, you use the WHERE clause.

```java
String hql = "FROM Employee emp WHERE emp.age = 24"; //retrinve data from employee table where age is 24
Query query = session.createQuery(hql);
List<Employee> list = query.list();
```

**ORDER BY :**
To sort your HQL query's results, you will need to use the ORDER BY clause. You can order the results by any property on the objects in the result set either ascending (ASC) or descending (DESC).

```java
String hql = "FROM Employee emp ORDER BY emp.id DESC"; //retrinve data from employee by descending order of id
Query query = session.createQuery(hql);
List<Employee> list = query.list();
```

If you wanted to sort by more than one property, you would just add the additional properties to the end of the order by clause, separated by commas.

```java
String hql = "FROM Employee emp ORDER BY emp.name ASC, emp.age DESC"; //retrinve data from employee by ascending order of name and descending order of age
Query query = session.createQuery(hql);
List<Employee> list = query.list();
```

**GROUP BY:**
This clause lets Hibernate pull information from the database and group it based on a value of an attribute and, typically, use the result to include an aggregate value.

```java
String hql = "FROM Employee emp GROUP BY emp.city"; //retrinve data from employee group by city
Query query = session.createQuery(hql);
List<Employee> list = query.list();
```

**Named Paramater :**
Hibernate supports named parameters in its HQL queries. This makes writing HQL queries that accept input from the user easy and you do not have to defend against SQL injection attacks.

```java
String hql = "FROM Employee emp WHERE emp.name = :name";
Query query = session.createQuery(hql);
query.setParameter("name", "Sagar");
Employee employee = (Employee) query.uniqueResult();
```

**UPDATE :**
The UPDATE clause can be used to update one or more properties of an one or more objects.

```java
String hql = "UPDATE Employee emp SET emp.name = :name WHERE emp.id = :id"; //Update name of employee with id 101
Query query = session.createQuery(hql);
query.setParameter("name", "Sagar");
query.setParameter("id", 101);
int result = query.executeUpdate();
System.out.println("result : " + result);
```

```java
String hql = "UPDATE Employee emp SET emp.name = :name"; //Update name of all employees
Query query = session.createQuery(hql);
query.setParameter("name", "Sagar");
int result = query.executeUpdate();
System.out.println("result : " + result);
```

**DELETE :**
The DELETE clause can be used to delete one or more objects.

```java
String hql = "DELETE FROM Employee emp WHERE emp.id = :id"; //Delete employee with id 101
Query query = session.createQuery(hql);
query.setParameter("id", 101);
int result = query.executeUpdate();
System.out.println("result : " + result);
```

**INSERT INTO :**
HQL supports INSERT INTO clause only where records can be inserted from one object to another object.

```java
String hql = "INSERT INTO Employee(emp.id, emp.name, emp.city) SELECT emp.id, emp.name, emp.city FROM Employee emp";
Query query = session.createQuery(hql);
int result = query.executeUpdate();
System.out.println("result : " + result);
```

**AND :**
HQL supports AND logical operator returning objects where both the conditions are true

```java
String hql = "FROM Employee emp WHERE emp.name = :name AND emp.city = :city";
Query query = session.createQuery(hql);
query.setParameter("name", "Sagar");
query.setParameter("city", "Pune");
List<Employee> list = query.list();
```

**OR :**
HQL supports OR logical operator returning objects where one of the conditions is true

```java
String hql = "FROM Employee emp WHERE emp.name = :name OR emp.city = :city";
Query query = session.createQuery(hql);
query.setParameter("name", "Sagar");
query.setParameter("city", "Pune");
List<Employee> list = query.list();
```

**NOT :**
HQL supports NOT logical operator returning objects where the condition is false

```java
String hql = "FROM Employee emp WHERE NOT emp.name = :name";
Query query = session.createQuery(hql);
query.setParameter("name", "Sagar");
List<Employee> list = query.list();
```

**Aggregates :**
HQL supports aggregate functions such as SUM, MAX, MIN, COUNT, AVG, etc.

```java
String hql = "SELECT SUM(emp.id) FROM Employee emp";
String hql = "SELECT MAX(emp.id) FROM Employee emp";
String hql = "SELECT MIN(emp.id) FROM Employee emp";
String hql = "SELECT COUNT(emp.id) FROM Employee emp";
String hql = "SELECT AVG(emp.id) FROM Employee emp";
```

**Pagination :**
HQL supports pagination with the LIMIT clause. This makes it easy to retrieve a subset of the results from the database.

```java
String hql = "FROM Employee";
Query query = session.createQuery(hql);
query.setFirstResult(0);
query.setMaxResults(10);
List<Employee> list = query.list();
```

## Criteria Queries

Criteria API allows you to build up a criteria query object programmatically where you can apply filtration rules and logical conditions.

The Hibernate Session interface provides createCriteria() method, which can be used to create a Criteria object that returns instances of the persistence object's class when your application executes a criteria query.

```java
Criteria criteria = session.createCriteria(Employee.class); //Get every object of Employee class
List<Employee> list = criteria.list();
```

**Restrictions :**
Using add() method, you can add restrictions to the criteria query object.
`org.hibernate.criterion.Restrictions`

```java
Criteria criteria = session.createCriteria(Employee.class); //Get every object of Employee class
criteria.add(Restrictions.eq("age", 18)); // Return age equal to 18
criteria.add(Restrictions.ne("age", 18)); // Return age not equal to 18
criteria.add(Restrictions.gt("age", 18)); // Return age greater than 18
criteria.add(Restrictions.lt("age", 18)); // Return age less than 18
criteria.add(Restrictions.like("name", "S%")); // Return name starting with S
criteria.add(Restrictions.ilike("name", "S%")); // Return name starting with S (ignores case)
criteria.add(Restrictions.between("age", 18, 25)); // Return age between 18 and 25
criteria.add(Restrictions.isNull("age")); // Return age is null
criteria.add(Restrictions.isNotNull("age")); // Return age is not null
criteria.add(Restrictions.isEmpty("name")); // Return name is empty
criteria.add(Restrictions.isNotEmpty("name")); // Return name is not empty
List<Employee> list = criteria.list();
```

**Pagination :**

```java
Criteria criteria = session.createCriteria(Employee.class); //Get every object of Employee class
criteria.setFirstResult(0);
criteria.setMaxResults(10);
List<Employee> list = criteria.list();
```

**Sort :**
Using addOrder() method, you can add sorting order to the criteria query object.
`org.hibernate.criterion.Order`

```java
Criteria criteria = session.createCriteria(Employee.class); //Get every object of Employee class
criteria.addOrder(Order.asc("name")); // Ascending
criteria.addOrder(Order.desc("name")); // Descending
List<Employee> list = criteria.list();
```

**Projections :**
Using setProjection() method, you can get average, maximum, or minimum of the property values. The Projections class is similar to the Restrictions class, in that it provides several static factory methods for obtaining Projection instances.
`org.hibernate.criterion.Projections`

```java
Criteria criteria = session.createCriteria(Employee.class); //Get every object of Employee class
criteria.setProjection(Projections.rowCount()); // Get count of objects
criteria.setProjection(Projections.avg("age")); // Get average of age
criteria.setProjection(Projections.max("age")); // Get maximum of age
criteria.setProjection(Projections.min("age")); // Get minimum of age
criteria.setProjection(Projections.sum("age")); // Get sum of age
List<Employee> list = criteria.list();
```

## Native SQL Queries

We can use the native SQL queries in HQL.
`org.hibernate.SQLQuery`

```java
SQLQuery query = session.createSQLQuery("SELECT * FROM EMPLOYEE");
List<Employee> list = query.list();
```

Named Query

```java
SQLQuery query = session.createSQLQuery("SELECT * FROM EMPLOYEE WHERE ID = :id");
query.setParameter("id", 101);
List<Employee> list = query.list();
```

Mapping to Entity

```java
query.addEntity(Employee.class); //Map to Employee class
```

Maping to DTOs : when we want to retrive some specific columns from tables that time we create a DTO class with required attributes and map it to the query.

```java
query.setResultTransformer(Transformers.aliasToBean(DTO.class)); //Map to DTO class
```

List Result

```java
List<Employee> list = query.list();
```

Single Result

```java
Employee employee = query.uniqueResult();
```

Execute Update

```java
int result = query.executeUpdate();
```

## Caching

Caching is used to improve the performance of queries. It is a buffer memory that lies between the application and the database. Cache memory stores recently used data items in order to reduce the number of database hits as much as possible.

There are 3 types of cache in Hibernate
1. First Level Cache (Default and Mandatory)
2. Second Level Cache
3. Query Cache

## Batch Processing

By using batch processing, we can process multiple/large amount of records at once.

*We have to set batch size on configuration* : `hibernate.jdbc.batch_size=50`

Example : (Insert 100000 records in batch of 50)
```java
Session session = SessionFactory.openSession();
Transaction transaction = session.beginTransaction();
//Here we are processing 100000 records
for ( int i=0; i<100000; i++ ) {
   Employee employee = new Employee(.....);
   session.save(employee);
   if( i % 50 == 0 ) { // Same as the JDBC batch size
      //flush a batch of inserts and release memory:
      session.flush();
      session.clear();
   }
}
transaction.commit();
session.close();
```

Example : (Update 100000 records in batch of 50)
```java
Session session = sessionFactory.openSession();
Transaction transaction = session.beginTransaction();

ScrollableResults employeeCursor = session.createQuery("FROM EMPLOYEE").scroll();
int count = 0;

while ( employeeCursor.next() ) {
   Employee employee = (Employee) employeeCursor.get(0);
   employee.updateEmployee();
   seession.update(employee); 
   if ( ++count % 50 == 0 ) {
      session.flush();
      session.clear();
   }
}
transaction.commit();
session.close();
```

## Interceptors
It is an Inteface which provides some methods,which can be call different stahes to perform some required tasks.

These methods are callbacks from the session to the application, allowing the application to inspect and/or manipulate properties of a persistent object before it is saved, updated, deleted or loaded.
`org.hibernate.Interceptor`

**findDirty()**
This method is be called when the flush() method is called on a Session object (modified).
`session.findDirty();`
```java
Employee updateEmployee = session.get(Employee.class, 101);
updateEmployee.setName("John");
//update the Employee entity
session.update(updateEmployee);
//Check for dirty properties
Set<String> dirtyProperties = session.findDirty("Employee", updateEmployee)

for(String property : dirtyProperties) {
   System.out.println(property);
}
```

**instantiate()**
This method is called when a persisted class is instantiated.
`session.instantiate();`

**isUnsaved()**
This method is called when an object is passed to the saveOrUpdate() method.
`session.isUnsaved();`

**onDelete()**
This method is called before an object is deleted.
`session.onDelete();`

**onFlushDirty()**
This method is called when Hibernate detects that an object is dirty (i.e. have been changed) during a flush i.e. update operation.
`session.onFlushDirty();`

**onLoad()**
This method is called before an object is initialized.
`session.onLoad();`

**onSave()**
This method is called before an object is saved.
`session.onSave();`

**postFlush()**
This method is called after a flush has occurred and an object has been updated in memory.
`session.postFlush();`

**preFlush()**
This method is called before a flush.
`session.preFlush();`

Example
`MyInterceptor.java`
```java
import java.io.Serializable;
import java.util.Date;
import java.util.Iterator;

import org.hibernate.EmptyInterceptor;
import org.hibernate.Transaction;
import org.hibernate.type.Type;

public class MyInterceptor extends EmptyInterceptor {
   private int updates;
   private int creates;
   private int loads;

   public void onDelete(Object entity, Serializable id,
      Object[] state, String[] propertyNames, Type[] types) {
       // do nothing
   }

   // This method is called when Employee object gets updated.
   public boolean onFlushDirty(Object entity, Serializable id,
      Object[] currentState, Object[] previousState, String[] propertyNames,
      Type[] types) {
         if ( entity instanceof Employee ) {
            System.out.println("Update Operation");
            return true; 
         }
         return false;
   }
	
   public boolean onLoad(Object entity, Serializable id,
      Object[] state, String[] propertyNames, Type[] types) {
         // do nothing
         return true;
   }
   
   // This method is called when Employee object gets created.
   public boolean onSave(Object entity, Serializable id,
      Object[] state, String[] propertyNames, Type[] types) {
         if ( entity instanceof Employee ) {
            System.out.println("Create Operation");
            return true; 
         }
         return false;
   }
   
   //called before commit into database
   public void preFlush(Iterator iterator) {
      System.out.println("preFlush");
   }
   
   //called after committed into database
   public void postFlush(Iterator iterator) {
      System.out.println("postFlush");
   }
}
```
`Employee.java`
```java
public class Employee {
   private int id;
   private String firstName; 
   private String lastName;   
   private int salary;  
   //Generate Getters and Setters and Constructor
}
```
`RunEmployee.java`
```java
import java.util.List; 
import java.util.Date;
import java.util.Iterator; 
 
import org.hibernate.HibernateException; 
import org.hibernate.Session; 
import org.hibernate.Transaction;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class RunEmployee {
   private static SessionFactory factory; 
   public static void main(String[] args) {
      
      try {
         factory = new Configuration().configure().buildSessionFactory();
      } catch (Throwable ex) { 
         System.err.println("Failed to create sessionFactory object." + ex);
         throw new ExceptionInInitializerError(ex); 
      }

      ManageEmployee ME = new ManageEmployee();

      /* Add few employee records in database */
      Integer empID1 = ME.addEmployee("Zara", "Ali", 1000);
      Integer empID2 = ME.addEmployee("Daisy", "Das", 5000);
      Integer empID3 = ME.addEmployee("John", "Paul", 10000);

      /* List down all the employees */
      ME.listEmployees();

      /* Update employee's records */
      ME.updateEmployee(empID1, 5000);

      /* Delete an employee from the database */
      ME.deleteEmployee(empID2);

      /* List down new list of the employees */
      ME.listEmployees();
   }
   
   /* Method to CREATE an employee in the database */
   public Integer addEmployee(String fname, String lname, int salary){
      Session session = factory.openSession( new MyInterceptor() );
      Transaction tx = null;
      Integer employeeID = null;
      
      try {
         tx = session.beginTransaction();
         Employee employee = new Employee(fname, lname, salary);
         employeeID = (Integer) session.save(employee); 
         tx.commit();
      } catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      } finally {
         session.close(); 
      }
      return employeeID;
   }
   
   /* Method to  READ all the employees */
   public void listEmployees( ){
      Session session = factory.openSession( new MyInterceptor() );
      Transaction tx = null;
      
      try {
         tx = session.beginTransaction();
         List employees = session.createQuery("FROM Employee").list(); 
         for (Iterator iterator = employees.iterator(); iterator.hasNext();){
            Employee employee = (Employee) iterator.next(); 
            System.out.print("First Name: " + employee.getFirstName()); 
            System.out.print("  Last Name: " + employee.getLastName()); 
            System.out.println("  Salary: " + employee.getSalary()); 
         }
         tx.commit();
      } catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      } finally {
         session.close(); 
      }
   }
   
   /* Method to UPDATE salary for an employee */
   public void updateEmployee(Integer EmployeeID, int salary ){
      Session session = factory.openSession( new MyInterceptor() );
      Transaction tx = null;
      
      try {
         tx = session.beginTransaction();
         Employee employee = (Employee)session.get(Employee.class, EmployeeID); 
         employee.setSalary( salary );
		 session.update(employee); 
         tx.commit();
      } catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      } finally {
         session.close(); 
      }
   }
   
   /* Method to DELETE an employee from the records */
   public void deleteEmployee(Integer EmployeeID){
      Session session = factory.openSession( new MyInterceptor() );
      Transaction tx = null;
      
      try {
         tx = session.beginTransaction();
         Employee employee = (Employee)session.get(Employee.class, EmployeeID); 
         session.delete(employee); 
         tx.commit();
      } catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      } finally {
         session.close(); 
      }
   }
}
```
