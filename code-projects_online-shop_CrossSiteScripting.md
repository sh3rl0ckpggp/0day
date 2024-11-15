Vulnerability Advisory: XSS Vulnerabilities in Lifestyle Store Web Application
- **Application Name**: Online Shop Store 
- **Software Link**: [Download Link](https://code-projects.org/online-shop-store-in-php-with-source-code/)
- **Vendor Homepage**: [Vendor Homepage](https://code-projects.org/)
- **Bug**: Cross Site Scripting Reflected (XSS)
- **Author** : sh3rl0ckpgp

## About project:
The online shop store In PHP is a simple project developed using PHP, JavaScript, and CSS. The project contains only the end-user sides. The users can go through the store without logging in but have to log in to the system form adding the items to the cart and buy the items.

## Vulnerability Description:

- The vulnerability is due to improper handling of user input in the m2 parameter. The value of this parameter is directly echoed in the HTML response without sanitization or escaping, which allows attackers to inject arbitrary JavaScript into the page.

## Vulnerable Code Section:

```
<?php
if(isset($_GET["m2"])){
  echo $_GET['m2'];
}
?>
```
- The $_GET['m2'] comes from the URL query string (e.g., signup.php?m2=<script>alert(1)</script>). This is user-controlled input, which means that an attacker can manipulate the value of m2 and inject malicious content.


## Proof of Concept (PoC):

- Location: `http://localhost/online-shop/signup.php?m2=`
- Payload : `<svg%20onload=alert(document.cookie)>`
- Poc Video : [Video Link](https://youtu.be/QThAqddl5Dk)


## CVSS Score
- **CVSS Base Score**: 6.1 (Medium)
- **Attack Vector**: Web (remote)
- **Impact**: High
- **Exploitability**: Low (requires a victim to visit a crafted URL)
