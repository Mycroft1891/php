# Learning PHP

> In order to work with PHP we need a PHP server. The simplest option is to
> use a docker container and compose it up within the current directory mounted
> as a volume. View [docker-compose.yml](docker-compose.yml)

- [Learning PHP](#learning-php)
  - [The Basics](#the-basics)
    - [Variables](#variables)
    - [Numbers and Arithmetic](#numbers-and-arithmetic)
    - [Strings](#strings)
    - [Control Structure](#control-structure)
  - [Format HTML](#format-html)
  - [Working with Dates](#working-with-dates)
  - [Using a Database](#using-a-database)
  - [Working with Forms](#working-with-forms)
    - [SQL Injection](#sql-injection)
  - [Sessions and Session Variables](#sessions-and-session-variables)
  - [File Upload](#file-upload)

## The Basics

### Variables

Variables start with a dollar sign ($) and are case sensitive. The scope of the
variable depends on where you set it. It is possible to make variable globally
available using the **global** keyword:

```php
$normal_var = 'I have my own scope';
global $global_variable = 'I can be accessed anywhere';
```

### Numbers and Arithmetic

PHP supports basic arithmetic operators: (+, -, /, *, %, +=, -=,).
Some common numeric functions which come in handy:

- abs();
- pi();
- round();
- sqrt();

### Strings

Strings can be enclosed in single or double quotes. The difference between both
is that single quoted string return that exact string while double quoted
strings evalute any variable inside the string before returning the string.
Some common string functions include:

- htmlenteties(); // convert string to html
- html_entity_decode(); // convert html to string
- str_pad();
- str_repeat();
- str_replace();
- strtoupper();

### Control Structure

PHP supports all the common control structures:

- if/else
- switch
- while
- for

```php
if ($today == 'Sunday') {
  echo 'I do not have time on sunday';
} else {
  echo 'I do not have time in the week either';
}

switch($today) {
  case 'Monday':
    echo 'Monday is free';
    break;
  case 'Tuesday':
    echo 'Tuesday is busy';
    break;
  default:
    echo 'The other days are fexible call me';
}

while($today != 'Sunday'):
  echo 'Another busy day';
endwhile;

for($i = 0; $i < 10; $i++) {
  echo "This is round number $i";
}
```

## Format HTML

PHP is used to format HTML and inject dynamic content. This is archived through
a server capable of parsing PHP and the special PHP tag:

```php
// print the version in the browser
<?php echo phpversion() ?>
```

We can inject HTML partials into our main HTML in a similar way:

```php
<body>
  <h1>My awesome title</h1>
  <?php include 'content.php'; ?>
</body>
```

> Note: variables declared in a partial are accessable where ever you include
> the partial.

## Working with Dates

PHP has built in functions to work with dates:

- mktime() - (timestamp for a time)
- time() - (get today's date)
- date() - (get and format date)

## Using a Database

PHP has support for various database systems. The most popular option for PHP
is MySQL. We use the official mysql docker image to speed things up. Furthermore
, we install the **mysqli** extension to work with our mysql database:

```php
// connect to database
$conn = new mysqli(host, user, username);

// check for errors
if ($conn->connect_error) {
  echo 'ERROR: failed to connect to the database'.$conn.connect_error;
}

// perform an SQL query
$conn->query('SQL QUERY GOES HERE');

// display latest error
$conn->error;
```

## Working with Forms

You can access form field properties with the **$_POST/$_GET** variables which
holds all the post parameters and queries respectively:

```php
$name = $_POST['name'];

$conn = new msqli(init, db, connection);

// don't trust user input
$conn->escape_string($name);
```

### SQL Injection

As always, never trust user input. Any user can sneak in SQL statements into our
database if we don't clean up the input previously. For those reasons clean
and double check user inputs before inserting it into an SQL query.

## Sessions and Session Variables

Session variables persits across sites until the browser is closed or until we
terminate it explicitly. Sessions work by storing all the information using a
UID in a cookie. We can use Sessions in PHP like this:

```php
session_start();

<html>
...
```

We can then retrieve and set data in our Session using the **$_SESSION**
variables:

```php
// set data
$_SESSION['name'] = 'Alexander';
$_SESSION['country'] = 'Germany';

// get data
$_SESSION['name'];

// check if data exists
if (isset($_SESSION['name'])) {
  echo 'Name is already set';
} else {
  echo 'Name is not set';
}

// remove specific session data
unset($_SESSION['name'])

// remove all session data
session_destroy();
```

## File Upload

Similar to **$_GET/$_POST** we can catch any file from a from using the
**$_FILE** variable.

```php
// any errors with the file
$_FILE["file"]["error"];

// the file itself
$_FILE["file"]["file_field_name"];

// file metadata
$_FILE["file"]["type"];
$_FILE["file"]["size"];
$_FILE["file"]["tmp_name"];
```
