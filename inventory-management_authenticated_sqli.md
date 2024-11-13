# Vulnerability Advisory: Time-Based SQL Injection in Lifestyle Store Web Application

- **Application Name**: Online Shop Store  
- **Software Link**: [Download Link](https://code-projects.org/inventory-management-in-php-with-source-code/)  
- **Vendor Homepage**: [Vendor Homepage](https://code-projects.org/)  
- **Bug**: Time-Based SQL Injection  
- **Author**: sh3rl0ckpgp  

## About Project:
The Inventory Management system in PHP allows admins and staff to manage products, customers, and orders. It features login-based access for CRUD operations, offering an efficient way for store admins to manage inventory and records.

## Vulnerability Description:
- The vulnerability is a **Time-Based SQL Injection** found in the `editProduct.php` script, where the `id` parameter is improperly handled. This parameter is directly inserted into a SQL query without sanitization, allowing an attacker to inject malicious SQL code. The vulnerability occurs in the GET request through the `id` parameter, which is used to fetch product details.

- An attacker can exploit this vulnerability by manipulating the URL to execute arbitrary SQL commands. In particular, they can trigger a time delay using `SLEEP()` to determine whether the SQL injection is successful, based on the response time.

## Vulnerable Code Section:

```php
<?php
    $id = $_GET['id']; // Vulnerable to SQL Injection
    $sql = "SELECT * FROM products WHERE id='$id' LIMIT 1";
    $res = mysqli_fetch_assoc($conn->query($sql));
?>
```

## Proof of Concept (PoC):


- put the single quote to the `id` parameter => http://localhost/online-shop/editProduct.php?id=12%27XOR(if(now()=sysdate(),sleep(5),0))XOR%27Z

![image](https://github.com/user-attachments/assets/b305d118-0ab1-4d75-bf53-3b403ef3bb4a)


![image](https://github.com/user-attachments/assets/22e20ba4-15fb-4622-8a28-fb976a4763d7)

Payload: 12' XOR(if(now()=sysdate(),sleep(5),0)) XOR 'Z

The payload triggers a 5-second delay if the SQL query is successful, allowing an attacker to confirm that the SQL injection is functional.

- After identifying an SQL injection vulnerability, the exploitation phase can be performed using sqlmap, a powerful tool that automates the process of exploiting SQL injection flaws to extract information and perform further attacks.

![image](https://github.com/user-attachments/assets/2e93ae91-2a21-4ce7-8b4b-2e7da3a929b5)

PoC Video: [Video Link](https://youtu.be/RhBDAApSELc)



## Impact:
Data Exposure: Attackers can use this vulnerability to extract sensitive data from the database.
Denial of Service: Time-based SQL injection can cause delays or denial of service due to the use of functions like SLEEP().
Privilege Escalation: If exploited further, this could lead to unauthorized access or modification of critical data.


## Mitigation:
Sanitize User Input: Use prepared statements with parameterized queries (e.g., mysqli_prepare) to prevent SQL injection.
Use ORM Libraries: Implement Object-Relational Mapping (ORM) libraries to handle SQL queries securely.
Input Validation: Validate and sanitize all user inputs, especially those coming from GET and POST parameters.
Set SQL Timeouts: Implement timeouts for SQL queries to mitigate potential DoS attacks using time-based SQL injection.
