# Helpers

Laravel has a great set of helpers for String and Arrays. We felt it would be really nice if some of these were available for our Vue components as well.

[Source Code](https://github.com/grafiteinc/helpers)

!!! warning "Helper is a mirroring of Laravel's helper methods, but in some cases will not work due to JavaScripts typecasting."

## String Helpers

**explode**: A rough clone of PHP's explode method for a string converting it to an array
```
"This is my name".explode()
// ["This", "is", "my", "name"]
```

**after**: Returns everything after the given value
```
"This is my name".after("This is")
// ' my name'
```

**afterLast**: Returns everything after the last occurrence of the given value
```
"App\\Http\\Controllers\\Controller".afterLast("Controller");
// 'Controller'
```

**before**: Returns everything before the given value
```
"This is my name".afterLast("my name");
// 'This is '
```

**beforeLast**: Returns everything before the last occurrence of the given value
```
"This is my name".afterLast("is");
// 'This '
```

**camel**: Converts the string to camelCase
```
"foo bar".camel();
// 'fooBar'
```

**contains**: Determines if the string contains the given value
```
"foo bar".contains("boo");
// false
```

**containsAll**: Determines if the string contains the all the given values
```
"foo bar".containsAll(["boo", "foo"]);
// false
```

**endsWith**: Determines if a string ends with the given string
```
"foo bar".endsWith("bar");
// true
```

**finish**: Adds the value to the end of the string unless it already exists
```
"foo bar".finish("bar");
// "foo bar"

"foo bar".finish(" cool");
// "foo bar cool"
```

**is**: Determines if a string matches the given value
```
"foo bar".is("foo bar");
// true
```

**kebab**: Converts a string to kebab case
```
"foo bar".kebab();
// "foo-bar"
```

**limit**: Truncates a string to the given limit
```
"foo bar".limit(5);
// "foo-b..."
```

**plural**: Converts a string to its plural form (English)
```
"test".plural();
// "tests"
```

**replaceArray**: Replaces the given value with an array sequentially
```
"The event will take place between ? and ?".replaceArray('?', ['8:30', '9:00']);
// "The event will take place between 8:30 and 9:00"
```

**replaceFirst**: Replaces first occurrence of the given value
```
"the quick brown fox jumps over the lazy dog".replaceFirst("the", "a");
// "a quick brown fox jumps over the lazy dog"
```

**replaceLast**: Replaces last occurrence of the given value
```
"the quick brown fox jumps over the lazy dog".replaceLast("the", "a");
// "the quick brown fox jumps over a lazy dog"
```

**singular**: Converts a string to its singular form (English)
```
"dogs".singular();
// "dog"
```

**slug**: Converts a string to its singular form
```
"I love watching movies".slug();
// "i-love-watching-movies"
```

**snake**: Converts a string to snake case
```
"watch dog".snake();
// "watch_dog"
```

**start**: Adds the given value to the start of a string unless it's already there
```
"is my name".start("This ");
// "This is my name"
```

```
"This is my name".start("This ");
// "This is my name"
```

**startsWith**: Determines if a string begins with the given string
```
"This is my name".startsWith("This");
// true
```

**title**: Converts a string to a title format
```
"zen and the art of Motorcyle Maintenance".title();
// "Zen And The Art Of Motorcyle Maintenance"
```

**words**: Limits the number of words in a string
```
"zen and the art of Motorcyle Maintenance".words(4);
// "zen and the art"
```

## Debug Helper

**dd**: It may be silly but there are plenty of times when its easy to type `dd` rather than `console.log` so this removes the mistake.
```
dd(args)
```

## Array Helpers

**Implode**: This is a rough clone of PHP's implode method, though it works almost the same as `join` it's a little more elegant
```
["Nissan", "BMW", "Ferrari"].implode();
// "Nissan BMW Ferrari"
```

**First**: Grabs the first item from an array.
```
["Nissan", "BMW", "Ferrari"].first();
// "Nissan"
```

**Last**: Grabs the last item from an array.
```
["Nissan", "BMW", "Ferrari"].last();
// "Ferrari"
```

**Remove**: Takes the specified item out of an array
```
["Nissan", "BMW", "Ferrari"].remove("BMW");
// ["Nissan", "Ferrari"]
```

