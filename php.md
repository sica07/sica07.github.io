#PHP
[<TIL](Programming.md)

## Prevent adding a new property to a instance of a class

```PHP
  public function __set($name, $value) {
        throw new Exception("Cannot add new property \$$name to instance of " . __CLASS__);
    }
```
When you set a value to an instance variable in PHP, it will first look for that variable.
If it exists then the value is set directly and `__set()` is never called. If the instance variable
does not exist (or if it cannot be accessed due to a private declaration) then it will call `__set()`.
If `__set()` doesn't exist, then it either creates the variable or throws an exception (in the case of a private variable).

## Checking if extension is enabled from the command line
 You can see the names of various extensions by using phpinfo() or if you're using the CGI or CLI version of PHP you can use the -m switch to list all available extensions:
`$ php -m`
