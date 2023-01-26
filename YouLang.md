# Youlang Design

## About This Document
This document uses several techniques to describe the grammar.   
Inside "[]" is regarded as programming concepts instead of literals.   
Use "[[]]" to specify a code block.      
"..." is used to duplicate the previous block or statement (but not literal) for 0 or more times.     
"[opt]" is a flag to indicate that this code block is optional. It is paired with "[/opt]"
"[opt if condition]" is used to indicate that this code block is optional if the condition holds.     
"[statement returns type]" is used to indicate that the statement returns a specific type of a kind of type(like integer).     
".." is used to duplicate the previous block or statement (but not literal or [opt]) for 1 time.
Some logical symbols: &(and), |(or), !(not)     
Use back-slash("\") to keep the original meaning.

## Purpose

- To build a multi-paradigm programming language.
- To build a easy-to-use programming language.
- To build a system-level programming language without high expense.
- To build a programming language with sort of flexibility. 

## Hello World
Most of the programming examples starts from "Hello, World!". Let me illustrate it in  Youlang.

```
main = {
    println "Hello, World!"
}
```

We will explain it later.
## Basic Grammar
The statements are separated by semicolons(;). However, the semicolon at the end of one line can be omitted. Use back-slash("\") to continue a line. 
A code block starts with "{" and ends with "}".


## Comment   
### Single Line Comment
A single line comment starts with "//".

### Multiline Comment
A multiline comment starts with "/*" and ends with "*/".   
Note that comments can be nested, which make the code block easier to be commented out.

Here is one example.
```
// This is a single line comment.
/* This is a multiline comment.
    /* Comments can be nested.
    This is a nested comment. */
*/
```

## Primitive Data Types
### Integers
The prefix of signed integers is "i" and the prefix of unsigned integers is "u".    
After the prefix is the number of bits used to store the integer.    
Available options: i8, i16, i32, i64, i128, u8, u16, u32, u64, u128    

### Floats
IEEE 754 float. The prefix of floats is "f".    
After the prefix is the number of bits used to store the float.    
Available options: f32, f64, f128.    

### Boolean
Boolean has only two values: true and false.   
It is actually i8, but only stores 0(false) and 1(true).    
Available option: bool.    

### Character
Character stores the UTF-8 value of a character.   
It is actually u32.
Available option: char.

## Literals
### Integers
An integer is a sequence of number without decimal points. If you want to specify the integer's type, just add the type name after the integer, like 123i8. Any integer without postfix has the type depending on it's usage. And if the usage is not specified, it is by default i32, and expands to i64 if the literal exceeds 2 ^ 31 - 1, and expands to i128 if the literal exceeds 2 ^ 63 - 1, and expands to u128 if the literal exceeds 2 ^ 127 - 1. Otherwise, it will throw an error.

### Floats
A float is a sequence of number with one decimal point. If you want to specify the float's tye, just add the type name after the float, like 1.0f32. Any float without postfix has the type depending on it's usage. And if the usage is not specified, it is by default f64.    

### Boolean
A boolean is represented by "true" or "false".   

### Character
A character is wrapped by single quotes ('). 'A', '🐱' and '靐' are all valid characters.

### String
A single line string is a sequence or an array of characters wrapped by quotes (").    
A multiline string is a sequence of characters wrapped by paired three quotes(""").    
We will talk about it later.    

### Range
A range is a special abstract data type that has literals. It is a closed interval.   
Grammar:
```
[value|expression returns integer] to .. [opt]by ..[/opt]
```

## Identifier
Identifiers could include (UTF-8)letters, numbers and underlines. An identifier must starts with a letter or an underline. "k_many123", "__init", "哈哈" are all valid identifiers. Note that the third identifier is not recommended.

## Reserved Words
Reserved words are words that cannot be used as identifiers. These words are: abstract, async, await, break, by, catch, const, continue, del, dynamic, else, enum, exec, false, for, goto, if, immut, implements, import, in, inner, match, module, public, return, self, static, struct, super, to, true, try, unsafe, while.   

## Goto
Goto is a keyword that is useless. It is listed in the reserved words because we do not want programmers to define it or implement it. As in many programming languages, it is harmful for designing.

## Constants
A constant is a value that binds to an identifier. The value must be known, or at least could be known at compile time. Defining constants uses the keyword "const".    
Grammar:
```
const [identifier][opt], ..[/opt] = [value|expression][opt], ..[/opt] 
```
Here are some examples:   
```
const PI = 3.14159
const HELLO_WORLD = "Hello, World!"
const MAX_I32 = 2147483647
```
"const" keyword can also define a function whose return value is expected to be viewed as a constant. We will talk about it later.

## Immutable Variables
An immutable variable is a value that cannot be known at compile time, but doesn't need to be changed at runtime. The variable can be rebinded, but cannot be reassigned. Defining immutable constants uses the keyword "immut".    
Grammar:
```
immut [identifier][opt], ..[/opt] = [value|expression][opt], ..[/opt]
```
Here are some examples:
```
immut test_int = 12345
immut test_float = 1.23f128
immut test_string = "Howdy"
immut a, b = 1, 2
immut a, b = b, a // Can rebind
```
This method is also applicable for defining a pure function. We will talk about it later.

## Mutable Variables
A mutable variable is a value that need to be changed at runtime.    
Grammar:
```
[[[identifier][opt]: [data type][/opt] [opt if uninitialized]]][opt], ..[/opt]= [value|expression][/opt][opt], ..[/opt]
```
Here are some examples:
```
test_int = 12345
test_float = 1.23f128
test_string = "Howdy"
test_1, test_2 = "1", 2
test_int = 123456 // Can be modified.
test_float = 12345 // Will be converted to float automatically.
// It seems to be a dynamically typed language, but it's actually a statically typed language. So the value will be implicitly converted to the suitable data type if possible. Otherwise, it will throw an error.
uninit_int: i32 // Uninitialized variable.
```
This method is also applicable for defining an impure function. We will talk about it later.

## Arithmetic
Arithmetic is what you expected in mathematics.    
Binary operators: +, -, *, /, %(mod), **(power), =     
Unary operators: +(positive), -(negative), ++(increase), --(decrease)     
Note: If both the operands of "/" are integers, then the result is a flooring-down integer. Or else the result is a float.    
Remark: You can combine one binary operator and "=" to shorten "[variable] = [variable] + [expression]".
Here are some examples:
```
a = 1 + 2 * 3 // 7
b = 3 * 5 / 4 // 3
c = 3 * 5 / 4.0 // 3.75
b++ // 4
c = -3 ** 5 // -243.0, a float.
```

## Logical Operation and Bitwise Operation
Logical operation and bitwise operation are also what you expected.   
Ternary operator: ?: (works as if/else but returns a value)       
Binary operators: >, <, >=, <=, ==(equals), !=(not equals), && (and), || (or), & (bit-and), | (bit-or), ^ (bit-xor), << (shift left), >> (shift right)     
Unary operator: ! (not), ~(bit-not)
The operator "and", "or", "xor" and "not" are overloaded for logical operation. If both of the operands are booleans, then the return value is boolean. If one of the operand is boolean and one is integer, we will regard "true" as 1 and "false" as 0 to do bitwise operation.   
Remark: The priority of all the logical operations are the same, and lower than bitwise operation. Then priority of all the bitwise operations are the same, and lower than arithmetic operations.
Here are some examples:
```
a = 2 & 3 // 2
b = true || false && true // true
c = 2 + (5 & 3) // 1
d = 1 << 5 // 32
```

## Control Flows
### Scope
The variables created inside the scope will be automatically deleted when the scope is end. This can simplify memory management.     
Grammar:
```
exec {
    [statement]
    ...
}
```
Example:
```
a = 1
exec {
    b = 2
    c = a + b
    println c
}
println a
// b and c are no longer accessible.
```
Another characteristic is that it can have "return values". It is actually very different from function, because it is expanded into the procedure and does not need function call.    
Example:
```
a, b = exec {
    s = 0
    i: i32
    for i in 1 to 100 {
        s += i
    }
    s, i
}
println a // 5050
println b // 101
```


### If Statement
If statement is the most common branching statement.    
Grammar:
```
if [condition] {
    [statement]
    ...
}
[opt]
[[
elif [condition] { // else if
    [statement]
    ...
}
]]
...
[/opt]
[opt]
else {
    [statement]
    ...
}
[/opt]
```
Here is one example:
```
if 2 > 1 {
    println "2 > 1"
}
elif 2 == 1 {
    println "2 = 1"
}
else {
    println "2 < 1"
}
```
It will definitely prints "2 > 1".

### Match Statement
Match statement is used for pattern matching. It can match one variable for many patterns.     
Grammar:
```
match [variable] {
    [[
    [value|variable|statement|range][opt], ..[/opt]... -> [opt if one statement]{[/opt]
        [statement]
        ...
    [opt if one statement]}[/opt]
    ]]
    ...
    [opt]
    else -> [opt if one statement]{[/opt]
        [statement]
        ...
    [opt if one statement]}[/opt]
    [/opt]
}
```
Example:
```
a = "apple" // string
match a {
    "apple", "pear", "orange" -> println "fruit"
    "tomato", "potato", "garbage" -> println "vegitable"
    else -> println "unknown"
}
n = 14 // number
match n {
    1, 2 -> println "Smaller than 3"
    3 to 15 -> println "Between 3 and 15" // range
    15 to 31 by 2 -> println "Odd number between 15 and 31"
    else -> println "Others"
}
```

### While Statement
While statement is used for execute one code block repeatedly if the condition is true. It is useful if the looping time is unknown.     
Grammar:
```
while [condition] {
    [statement]
    ...
}
```
Example:
```
a = 2
while a < 100 {
    a *= 2
}
println a // 128
```

### For Statement
For statement is used to iterate one iterable object. Range is one common iterable object. Array is another which will be discussed on the following.     
Grammar:
```
for [variable|identifier] in [variable|statement returns iterable] {
    [statement]
    ...
}
```
Example:
```
for i in 1 to 10 by 2 {
    println i
}
```
It will print all the odd numbers between 1 and 10.    

## Array
Array is a fixed-length sequence for a specific data type. The dimensions must be constants.    
Define Grammar:
```
// Initialize with returned array
[opt][modifier] [/opt][identifier] = [value|expression returns array]
// Note that this method just created a pointer to the original array. If the value of this array is changed, the original array will also change.
// Or initialize with array literal
[opt][modifier] [/opt][identifier] = \[[element][opt], ..[/opt]...\]
// Or do not initialize
[opt][modifier] [/opt][identifier]: [data type][[\[[dimension]\]]]...
// Or initialize with default value
[opt][modifier] [/opt][identifier]: [data type][[\[[dimension]\]]]... = [value]
```
Example:
```
a = [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]] // a 4 * 3 array
b: f64[100] // a f64 array without initializing.
c: char[50] = 0 // a char array initialized with '\0'.
```
Invoke Grammar:
```
[identifier]\[[variable|statement returns integer]\]...
```
Example:
```
a = [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]]
b = a[3][1] // b is 11
a[3][1] = 5 // Now a[3][1] is 5
c = a[5][5] // Out-of-bound error. Can be disabled.
```

Sequential Search and Judge If Present Grammar:
```
[value|expression][opt], ..[/opt]... in [value|expression returns array]
```
Example:
```
if 12, 23 in [1, 2, 3, 12, 15, 26] {
    println "In the array"
}
else {
    println "Not all in the array"
}
```
This is also applicable for strings.

## Function
Function is an important component in procedure oriented programming. Every program starts from "main" function, the starting point. In Youlang, the form of defining a function is almost the same as defining a closure. One function can have multiple return values (separated by commas). But actually, it is returning a tuple containing these values.    

The grammar is as follows:
```
[identifier] = [opt if no parameter]([opt][parameter][/opt][opt], ...[/opt])[/opt]{
    [statement]
    ...
}
```
If there is no explicit "return" statement in the function, then the function will return the value of the last statement. If you want to return nothing but the last statement do return something, just place "return" in the end.    

To call a function, the grammar is as follows:    
```
[identifier][opt if no parameter] [opt][parameter][/opt][opt], ...[/opt]
```
The return value will be ignored.     

To use a function's return value in other statements, the bracket cannot be ignored because it will make the compiler unable to tell whether you want to use the return value or the function itself.     

Example:
```
fib = (n){
    n == 0 or n == 1 ? n : fib(n - 1) + fib(n - 2)
}
hello_world = {
    println "Hello, World!"
}
swap = (a, b){
    b, a
}
main = {
    println fib(10) // 55
    hello_world // Hello, World!
    a, b = swap(1, 2) // a is 2, b is 1.
}
```

Function prototype and function overload is not present in Youlang. Function prototype is useless in this language as you can define functions after using them. Function overload is not allowed as it will mingle some usage. You can implement this function yourself by generic, however.    

### Lambda Function
Lambda functions are almost the same as other functions. The grammar is as follows:    
```
[opt if no parameter]([opt][parameter][/opt][opt], ...[/opt])[/opt]{
    [statement]
    ...
}
```
Lambda functions can act as a parameter of a function.    

### Optional Argument
You can define an optional argument by initializing an argument's value when defining an function. Note that initializing one parameter also means that you specify the data type.     
The grammer is as follows:
```
[identifier] = [opt if no parameter]([[[opt][parameter] = [value][/opt][opt]]], ...[/opt])[/opt]{
    [statement]
    ...
}
```
When using this function, you can just left one argument to blank when you prefer to use the default value (but you should keep the comma there). Also, you can use ":=" operator to specify the parameters you would like to use. With this operator, you can change the order of the parameters.    

Example:
```
print_info = (name, sex, school = "", age){
    println "Your name is " + name
    println "Your sex is " + sex
    println "Your age is " + age
    println "Your school is" + school
}
main = {
    print_info("Candy", "Female", "Yale", 20)
    print_info("Cindy", "Female",, 15) // Optional argument
    print_info(name := "Cathy", age := 30, sex := "Female") // Change the order
}
```

### Explicit Data Type
Like explicitly defining the data type of a variable, defining the data type of function parameters also uses colons.  

Example:
```
gcd = (x: i32, y: i32){
    r = x % y
    while r != 0 {
        x = y
        y = r
        r = x % y
    }
    i32(y) // Not necessary because y itself is i32.
}
main = {
    println gcd(5, 15) // 5
    println gcd(5.0, 15.0) // 5, implicitly convert the data type to i32.
}
```

### Variadic Argument List
There could be at most one variadic argument list. And the variadic argument list must be at the end of all the arguments. The argument list will be regarded as an array in the function. All of the elements' data types must be the same.     
Here is the grammar:
```
[identifier] = ([[opt][parameter][/opt][opt], ...[/opt], [parameter]\.\.\.]) {
    [statement]
    ...
}
```
Example:
```
print_info = (name, jobs...){
    println "Your name is " + name
    for job in jobs {
        println "One of your job is " + job
    }
}
main = {
    print_info("Lisa", "teacher")
    print_info("Mario", "professor", "streamer")
}
```

## Object Oriented Programming     
OOP is an important concept in modern programming. Three basic components in OOP are encapsulation, polymorphism and inheritance. 

### Structure
Structure is an abstract data type which can store a bundle of relative data. But unlike many programming languages, it can have functions (methods), so it is more like a class. It cannot be inherited.      
Defining a structure:
```
[identifier] = struct {
    [opt][opt][permission][/opt][variable definition][/opt] // Properties
    ...
    [opt][opt][permission][/opt][function definition][/opt] // Methods
    ...
    [opt][prev identifier] = [opt if no parameter]([opt][parameter][/opt][opt], ...[/opt])[/opt]{
        [statement]
        ...
    }[/opt] // Initializer
    [opt]~[prev identifier] = [opt if no parameter]([opt][parameter][/opt][opt], ...[/opt])[/opt]{
        [statement]
        ...
    }[/opt] // Deconstructor
}
```
Using a structure:
```
[identifier] = [struct name]([opt][parameter][/opt][opt], ...[/opt]) // Have initializer
[identifier] = [struct name]([opt][property] := [value|expression][/opt][opt], ...[/opt]) // Do not have initializer
```
Invoke:
```
[identifier].[property|method]
```
Example:
```
Student = struct {
    id: i32
    public name: string // Not suggested
    public grade: i32 // Also not suggested
}
Teacher = struct {
    id: i32
    name: string
    position: string
    public set_position = (position){
        self.position = position
    }
    public print_info = {
        println name + ": " + position
    }
    Teacher = (id, name, position){
        self.id = id
        self.name = name
        self.position = position
    }
    ~Teacher = {
        println "Deconstructor"
    }
}
main = {
    student = Student(id := 20230101, name := "Catherine", grade := 96)
    teacher = Teacher(12345678, "Alice", "Assistant Professor")
    println student.name + ": " + student.grade
    teacher.set_position "Vice Professor"
    teacher.print_info
    del teacher // delete the object and invoke deconstructor
}
```
Only "public" properties and methods can be used outside the structure (class). "self" keyword is used to avoid naming confusion. It is used to refer the variable outside the scope. "del" keyword is used to delete one object.