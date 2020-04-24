# FAQs & General Help

!!! Tip
    Just a few tips and tricks for getting started with Grafite Builder.

### Mail

Make sure you set your mail to `log` or a mail provider instance or you may end up hitting snags once the Starter Kit has been made.

### XDebug
In case you're encountering issues where PHP is just spitting out nested caps etc. You can
either set the nesting level in your `bootstrap/autoload.php` or your `php.ini` file.

```
ini_set('xdebug.max_nesting_level', 500);
```

