# SQL_Injection

## How SQL works:

#### Suppose this is the target link:
```
http://www.victim.com/products.php?val=100
```

#### and this is the inside php code
```
// connect to the database
$conn = mysql_connect(“localhost”,“username”,“password”);
// dynamically build the sql statement with the input
$query = “SELECT ∗ FROM Products WHERE Price < ‘$_GET[“val”]’ ”.
 “ORDER BY ProductDescription”;
// execute the query against the database
$result = mysql_query($query);
// iterate through the record set
while($row = mysql_fetch_array($result, MYSQL_ASSOC))
{
// display the results to the browser
echo “Description : {$row[‘ProductDescription’]} <br>”.
 “Product ID : {$row[‘ProductID’]} <br>”.
 “Price : {$row[‘Price’]} <br><br>”;
}
```
#### Here's the breakdown:

1. **Connect to the Database**: Establish a connection to the database server (`localhost`) with a specified username and password.

2. **Build SQL Query**: Dynamically construct an SQL query using input from the URL parameter (`$_GET['val']`). The query selects all columns (`*`) from the `Products` table where the `Price` column is less than the value provided in the URL parameter.

3. **Execute Query**: Execute the constructed SQL query against the database using `mysql_query()`.

4. **Process Results**: Iterate through the result set returned by the query using `mysql_fetch_array()`. For each row in the result set, display the product description, product ID, and price to the browser.
