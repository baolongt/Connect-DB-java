## Before begin
* DBMS (EX: [DBeaver](https://dbeaver.io/))
* JDBC driver
* Java IDE (EX: Intelliji)

## Coding
**Note**: In this instruction I used postgreSQL database

To work with database we create a class that help us connect and use JDBC API to manipulate database. We will name class as `DBHelpe.java`.
```
    public class DBhelper {
    static String url = // Your database url connection;
    public static Connection connectDB(){
        Connection c = null;
        try{
            // Can change to another driver like sqlserver,... etc
            Class.forName("org.postgresql.Driver");
            c = DriverManager.getConnection(url,
            // username ,
            // password);
        }
        catch (Exception e){
            e.printStackTrace();
            System.err.println(e.getClass().getName()+": "+e.getMessage());
        }
        return c;
    }
```
> Class `Connection` - A connection (session) with a specific database

From Connection object we can create SQL query and execute them.

To work with database first we should have a database and some table
*EX:*
**Table Brand**
| brand_id | brand_name |
| -------- | ---------- |
| 1        |    Nike    |
| 2        |    Uniqlo  |

After that go to main class and connect to database
```
 public class main {
    public static void main(String[] args) throws SQLException {
        // Connect to DB
        Connection connection = DBhelper.connectDB();
        Statement stm = null;
        ResultSet rs = null;
        int input = // input;
    }
}    
```
> `Statement` - The object used for executing a static SQL statement and returning the results it produces.
    3 Types:
    `Statement`
    `PreparedStatement`
    `CallableStatement` - Stored procedures

> `ResultSet` - Generated by executing a statement that queries the database

After that add this block of code
```
        try{
               
        }
        catch(Exception e){
            e.printStackTrace();
            System.err.println(e.getClass().getName()+": "+e.getMessage());
        }
        finally{
            if(rs != null){
                rs.close();
            }
            if(stm != null){
                stm.close();
            }
            if(connection != null){
                connection.close();
            }
        }
```
To avoid `memory leak` we should close all the JDBC API object when we don't use them anymore which in this case is the end of function
Next we continue inside *try* block
```
    ...
    try{
            // create statement
            stm = connection.createStatement();
            // config statement
            String sql = "SELECT * " +
                         "FROM brand " +
                         "WHERE brand_id = " + input;
            // execute query
            rs = stm.executeQuery(sql);
    }
    ...
```
If you query Brand table with query
```
SELECT *
FROM brand
WHERE brand_id = 1
```
The result you will get is
>1 - Nike

That is exactly tuple inside the `ResultSet rs` object return from `executeQuery` method

We can get it with method `next()`
```
    // handle result
    while (rs.next()){
        int resultId = rs.getInt("brand_id");
        String name = rs.getString("brand_name");
        System.out.println(resultId + " - " + name);
    }
```
Build and run project we will have an application connect to database and search a brand with id 1 and the console will print out
> 1 - Nike

### Diffirent from `Statement` and `PreparedStatement`
`PreparedStatement` can be dynamic change the input value with its methods instead of get value from one or some variables.
```
   ...
    PreparedStatement stm;
    ...
    String sql = "SELECT * " +
                "FROM brand " +
                "WHERE brand_id = ?";
    stm = connection.prepareStatement(sql);
    stm.setInt(1, input);
    rs = stm.executeQuery();
```
We change variable `stm` class to `PreparedStatement` and create a fixed sql query string and with some dynamic content we will put `?` to it. After that we can set it with method `set...(index, value)` which is `index` start from 1 and any value you want.

## Modularity and MVC pattern
In the world of developer, instead of code in 1 main file we usually code in multi files and each file do some particular things. And we call it `Modularity`

`MVC parttern` is well known parttern for software design. `MVC` stand for `Model View Control`




