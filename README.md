# Notes and Examples from PHP II Fundamentals Course

## Q & A
* Q: Are there any issues creating a stored procedure from PHP code?
* A: YES!  See: https://stackoverflow.com/questions/11300595/the-mysql-delimiter-keyword-isnt-working
    * Commands between the `BEGIN` and `END` block of the stored procedure creation code need to have the delimiter `;`
    * The shell command `DELIMITER` which is used to change the delimiter, is *not part of SQL*!
    * PDO will not send an SQL statement which contains `DELIMITER`: you will get a fatal error
    * The best approach is to create the stored procedure as a database administrator (DBA)
    * Alternatively ... and this is an *extremely* poor substitute, you can issue a _command shell_ command as follows:
```
shell_exec('mysql -u vagrant -pvagrant -e "DELIMITER #"');
```
    * You can then use PDO to send the commands to build the stored procedure
```
$pdo->exec($stored);
```
    * Afterwards, shell out to reset the delimiter:
```
shell_exec('mysql -u vagrant -vagrant -e "DELIMITER ;"');
```

* Q: What's a good quick way to learn about the Domain Model?
* A: See: https://www.infoq.com/minibooks/domain-driven-design-quickly

* Q: How long are traditional RDBMS databases going to be around?
* A: See: https://db-engines.com/en/ranking

* Q: major diffs between PSR-0 and PSR-4?
* A: PSR-4 has the following differences:
    * Main difference: *less restrictive*
    * got rid of the "requirement" that the top-level of a namespace must be the "vendor" name
    * underscores (_) have no special meaning
    * removed implementation details/requirements: leaves that up to you

* Q: do you have an example of plugin manager functionality using `__call()`?
* A: you could implement an array of callbacks which could be consulted by `__call()`

* Q: Is there documentation on the effort to make `__construct()` method failures consistent by throwing Exceptions?
* A: See: https://wiki.php.net/rfc/internal_constructor_behaviour

* Q: Is `realPath()` useful or recommended?
* A: Yes: example from OrderApp:
```
// function to ensure path exists
function reallyRealPath($path)
{
    if ($newPath = realpath($path)) {
        return $newPath;
    } else {
        throw new RuntimeException('Base path undefined');
    }
}

define('BASE', reallyRealPath(__DIR__ . '/../'));
```
