# Vulnerability Advisory: Authenticated Time-Based SQL Injection in Employee management system

- **Application Name**: Employee management system 
- **Software Link**: [Download Link](https://www.sourcecodester.com/php/17689/best-employee-management-system-php.html/)  
- **Vendor Homepage**: [Vendor Homepage](https://www.sourcecodester.com/)  
- **Bug**: Authenticated Time-Based SQL Injection  
- **Author**: sh3rl0ckpgp  

## About Project:
This employee management system in PHP that serves as an essential tool for organizations seeking to optimize their human resource management practices through effective software development in PHP.  You can say human resource management system in php also. Its implementation can lead to improved operational efficiency and enhanced employee-related record management.
## Vulnerability Description:
- The vulnerability is a **Time-Based SQL Injection** found in the `edit_role.php` script, where the `id` parameter is improperly handled. This parameter is directly inserted into a SQL query without sanitization, allowing an attacker to inject malicious SQL code. The vulnerability occurs in the POST request through the `id` parameter, which is used to edit role details.

- An attacker can exploit this vulnerability by manipulating the URL to execute arbitrary SQL commands. In particular, they can trigger a time delay using `SLEEP()` to determine whether the SQL injection is successful, based on the response time.

## Vulnerable Code Section:

```php
$stmt = $conn->prepare("SELECT * FROM groups WHERE id='" . $_POST['id'] . "'");
            $stmt->execute();
            $result = $stmt->fetch(PDO::FETCH_ASSOC);
```

## Proof of Concept (PoC):


![image](https://github.com/user-attachments/assets/ab15208a-f1e6-495b-ab51-e72e2481dd03)

![image](https://github.com/user-attachments/assets/f4763745-f2c4-4e9f-990b-d40959ff3f40)


Payload: ';SELECT SLEEP(5) AND 'id'='id

The payload triggers a 5-second delay if the SQL query is successful, allowing an attacker to confirm that the SQL injection is functional.

- After identifying an SQL injection vulnerability, the exploitation phase can be performed using sqlmap/ghauri

![image](https://github.com/user-attachments/assets/b8b5b27f-0ae5-48e2-85b4-99c3ad47f3e8)




## Impact:
Data Exposure: Attackers can use this vulnerability to extract sensitive data from the database.
Denial of Service: Time-based SQL injection can cause delays or denial of service due to the use of functions like SLEEP().
Privilege Escalation: If exploited further, this could lead to unauthorized access or modification of critical data.


## Mitigation:
Sanitize User Input: Use prepared statements with parameterized queries (e.g., mysqli_prepare) to prevent SQL injection.
Use ORM Libraries: Implement Object-Relational Mapping (ORM) libraries to handle SQL queries securely.
Input Validation: Validate and sanitize all user inputs, especially those coming from GET and POST parameters.
Set SQL Timeouts: Implement timeouts for SQL queries to mitigate potential DoS attacks using time-based SQL injection.
