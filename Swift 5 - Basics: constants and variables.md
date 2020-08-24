# Swift 5 - Basics: constants and variables

> This article is published also on [DEV Community](https://dev.to/dragonfly/swift-5-basics-constants-and-variables-4mjf)

## Intro

Some time ago I've decided to start learning Swift by writing posts about it. So, this is the first post...

I'm not trying to write "tutorials", it's not true. These posts are just my thoughts on electronic paper for learning purposes. Though I hope that someone will find them to be useful.

## Constants and variables

### Keywords: `let` and `var`

Finally, I saw clear boundaries between `let` and `var` keywords... No, not in JS, but in Swift!

```swift
let pi = 3.14
var description = "Pi"
```

The keyword `let` is used to define a constant.
The keyword `var` is used to define a variable.

```bash
age = 27
```

If you won't specify the keyword, like in the example above,  you'll get an error - `Use of unresolved identifier 'age'`.

You don't specify the `var` keyword only if you're overriding an existing variable.

You can define multiple variables in one line:

```swift
var name = "George", age = 27, position = "Software Engineer"
```

It is true for constants too:
```swift
let pi = 3.14, e = 2.718, phi = 1.618
```

You can override variables in Swift, but you can't override constants.

```swift
var description = "Pi"
description = "Ratio of a circle's circumference to its diameter"

print(description)
```

So, the output will be "Ratio of a circle's circumference to its diameter", because the variable `description` is overridden.

```swift
let pi = 3.14
pi = 3.1415
```

The result is an error - `Cannot assign to value: 'pi' is a 'let' constant`.

How about redeclaring a constant?
```swift
let pi = 3.14

let pi = 3.1415
```

The result is another error - `Invalid redeclaration of 'pi'`.

Though, we can't redeclare variables too!
```swift
var name = "John"

var name = "Bob"
```

Getting the same error message - `Invalid redeclaration of 'name'`.

So... we can't override or redeclare any constant or variable we have defined. I like it.


### Type annotations

Swift lets you to provide type annotations for declared variables and constants. Exciting! 

I like type annotations, because they improve the readability of the code.

```swift
var message: String

message = "Swift is cool."

let pi: Double

pi = 3.14
```

You can define multiple variables on a single line that have a same type:

```swift
var firstName, lastName, country: String
let pi, e, phi: Double
```

Let's try and violate the type annotation by assigning a boolean `true` to the variable `message`.

```swift
var message: String

message = true
```

Got an expected error! `Cannot assign value of type 'Bool' to type 'String'`.

Attention! Your variables always have a type, even if you don't have type annotations provided. Let's see another error example.

```swift
var myString = "some string"

myString = 777

print(myString)
```

Guess the error? `Cannot assign value of type 'Int' to type 'String'`.


## PHP

At this moment PHP is my main instrument, and whatever I learn - I compare it with PHP...

In PHP 7 we have type annotations for functions, class methods and properties. 

Anyways, I like type annotations in Swift more than in PHP, and there are various reasons for that:

* You can't define a typed variable out of the class.
* You need to call the `declare(strict_types=1);` at the top of the script to be able to define types for function arguments out of the class.
* If your argument is expecting `int`, then you must pass an integer... but you're still able to override that variable and assign a value of other type, e.g. `string`, just like in the example below.

```php
class Foo 
{
    public function bar(int $number)
    {
        $number = 'Oops! I am not a number anymore.';

        echo $number;
    }
}

$foo = new Foo();

echo $foo->bar(10);

// Output: Oops! I am not a number anymore.
```

## Outro

Well... I enjoyed!
