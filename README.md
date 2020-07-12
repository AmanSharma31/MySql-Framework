# MySql-Framework
Framework to make use of mysql easy and fast for everyone. It reduces the time of creating connection with mysql database, writing statements for all type of operations and then extracting the values from result set. The goal of this framework is to make the use of mysql database easy for everyone so that anybody can use mysql database.

## Getting-Started    
This framework will work only for primary key and not for composite keys.   
By following the below instruction you can use the framework in your machine. 

### Installation    
Create your project folder anywhere you want. For better understading let's assume it to be `testing`.     
In *testing* create following folders which are necessary-
* com\thinking\machines\beans
* lib   

Download all jar files from the repository, and put them in *lib* folder    


### Usage    

Create a database in your mysql, and then create tables and views in that database. An UI has been provided with this framework which will create JavaBeans for the tables and views in the database. These JavaBeans will be used to perform the add, update, remove and select operations. The framework will establish connection by getting information written in a *dbconf.json* file, which will be created in the *lib* folder after you click *save* button in the *Database Installation* UI.    
If you are running this framework for the first time or the *dbconf.json* file is not present in *lib* folder then you will see the *Database Installation* window first and then you need to fill your database information in it.    

#### Using UI

![](images/Database%20Installation.png)   

Fill all the information and click the *save* button. Remember the next window will appear only if the connection with the mysql database would be successfull, otherwise it will show an error message.    

After you have filled all the right information, press *save* button and then the file *dbconf.json* will be created in *lib* folder which will contain the information you have filled in the above UI.    

Then a new window is going to appear which will contain the information of your database.    

![](images/Database%20Information%20Table.png)    

![](images/Database%20Information%20View.png)

It has two Tab Panel, one for the tables and another one for the views. On the top it has a list of names of tables present in the database, by clicking on them it will show you the name of the class of JavaBean for that clicked table and below it has a table which will show the properties name of the JavaBean with its data type and it also show if there is any constraints applied on the properties.    
At the bottom most it has two buttons *Create All* and *Create*. By clicking on the *Create All* button it will generate or create the java and class files for all the tables present in the database. On the other hand if you want to create java and class files of a particular table then select that table from the list and then click the *Create* button, it will create the java and class files for that particular table.   
<br>
On the second tab it has same information for the views in the database. All components here have the same functionality as I have described for the first tab of table tab.    


If you want to use the UI, you can use it from anywhere by following lines-    

    java -classpath ..\testing; ..\testing\lib\*;. com.thinking.machines.ui.TMORMFrameworkUI   

After this you have the java and class files of the tables/views in the following directory    

    com\thinking\machines\beans  
    
#### Using Framework

You have the compiled classes. Now let's move toward the framework. First understand the things the framework will need to work.     

##### Annotations   

Framework will need annotations to perform operations on mysql database. These annotations are alreay available in    

    com\thinking\machines\annotations  
    
Following are the annotations available in the framework.      

* **DBTable** - DBTable annotation is appliable only on class of JavaBeans, it has a member named *value* which contains the name of the table for which the JavaBean has been created. Its value must be equal to the table name.

* **DBView** - DBView annotation is same as DBTable, the only difference is that it is appliable on the class of JavaBeans which refer to the views of the database.   

* **DBField** - DBField annotation is appliable only on properties of classes. Its *value* member will contains the name of a table field. Based on that table field following 5 annotation are going to applied on the property.

* **PrimaryKey** - PrimaryKey annotation tells the framework that the property referring to a table field has primary key constraints on itself.    

* **ForeignKey** - ForeignKey annotation tells the framework that the property referring to a table field has foreign key constraints on itself.    

* **Unique** - Unique annotation tells the framework that the property referring to a table field has Unique constraints on itself.     

* **NotNull** - NotNull annotation tells the framework that the property referring to a table field has NotNull constraints on itself.   

* **AutoIncrement** - AutoIncrement annotation tells the framework that the property referring to a table field has AutoIncrement constraints on itself.     

All this annotation will be applied on respective properties of respective classes by the code. You don't have to write anything here. I just introduced you with the annotations of the framework, that's it you don't have to dive into the depth of the use of annotation.     

##### Beans 

There are some JavaBean present in  
  
    com\thinking\machines\beans
    
which are 

* **Table** for a table of database.
* **View** for a view of database. 
* **Attribute** for a field of a table of view.
* **DBConfiguration** for the database connection information.      


##### Framework     

There is a *TMDatabase* class present with framework, this class is kind of initializer or data structure populator, it will create data structure of the database in a map.    

*TMFramework* is the framework which will perform operations on mysql database. Framework will instantiate the *TMDatabase* class and will get the data structure of the database. Remember this framework can't be instantiated normally, you can get its instant by *getInstance()* method. It has four methods for four operations add, update, remove and select. Also remember that before going for any method you should first call *begin()* method, which will set auto commit as false, and after you have done with your operation you have to call *commit()* method which will commit all the changes. Following are the methods available.

* **public void save(Object object)** - Wrap your data in a object of a JavaBean representing the table in which you want to insert or save, and then pass that object to this method and this method will insert that data into the table.

* **public void update(Object object)** - Wrap your data in a object of a JavaBean representing the table in which you want to update, and then pass that object to this method and this will update the data into the table. The updation will be done on the basis of the value of the property of the class on which *Primary Key* annotation is applied. Insert the value of primary key of the record you want to update, into this property.     

* **public void remove(Object value, Class cls)** - This method will be used for removing or deleting a record from the table. The first parameter of this method is the value of 
primary key of the record you want to delete and second parameter is the name of the class representing that table. Remember to give the name of class with the *class* extension.   

* **public Select select(Class cls)** - This method will receive the name of class representing the table or view of which data you want to retrieve with the *class* extension. In this method there is a lot of flexibility available. You can use this method same as you can write a mysql select statement.  

      tmORMFramework.select(Employee.class).where("employeeId").ge("102").and("name").in("lucky","mehar").orderBy("employeeId").ascending().orderBy("panNumber").ascending().query()     
      
You can use any combination to achieve your goal. Following are the methods that you can use as above -  

* public Select where(String name)
* public Select eq(Object value)
* public Select ne(Object value)
* public Select gt(Object value)
* public Select lt(Object value)
* public Select ge(Object value)
* public Select le(Object value)
* public Select and(String name)
* public Select or(String name)
* public Select in(Object... values)
* public Select orderBy(String orderBy)
* public Select ascending()
* public Select descending()
* public List<Object> query() 

Let me clarify that eq=equal, ne=not equal, gt= greater than, lt= less than, ge=greater or equal, le=less or equal.    
First of all you have to call the *select()* method of *TMORMFramework* and then must finish with the *query()* method. Between these two methods you can apply any valid combination.   
Now the *query()* method will return a List of Object type, you can type cast it to a List of any type by the following code- 

    List<Employee> list=(List<Employee>)(Object)tm.select(Employee.class).query();	   
    
**Remember that you can only call *select()* method for a view JavaBean, you can't save, update or remove through a view**   

**Note:- Framework will raise a *ORMException* if you there is any chance of getting a mysql error further. *ORMException* will explain that mysql error in very simple and easily understandable manner**

Thank you.
		
