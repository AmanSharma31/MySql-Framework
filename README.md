# MySql-Framework
Framework to make use of mysql easy and fast for everyone. It reduces the time of creating connection with mysql database, writing statements for all type of operations and then extracting the values from result set. The goal of this framework is to make the use of mysql database easy for everyone so that anybody can use mysql database.

## Getting-Started    
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

![](image/Database%20Installation.jsp)   



