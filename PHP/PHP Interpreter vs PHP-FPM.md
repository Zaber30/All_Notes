The PHP interpreter is the engine that understands and executes PHP code.

PHP-FPM manages PHP interpreter processes.

## Think of it like this

PHP interpreter = **engine**

PHP-FPM = **engine manager**

PHP-FPM manages PHP interpreter processes by creating, controlling, and reusing **PHP worker processes** that execute PHP code.


PHP Interpreter = the engine that executes PHP  
  
PHP Worker = a running PHP interpreter process  
  
PHP-FPM = manager that creates and controls those workers

PHP-FPM = Restaurant manager  
  
Worker = Chef  
  
PHP Interpreter = Chef's cooking ability