# Youlang Design

## About This Document
This document uses several techniques to describe the grammar.   
Inside "[]" is regarded as programming concepts instead of literals.   
Use "[[]]" to specify a code block.      
"..." is used to duplicate the previous block or statement (but not literal) for 0 or more times.     
"[opt]" is a flag to indicate that this code block is optional. It is paired with "[/opt]"
"[opt if condition]" is used to indicate that this code block is optional if the condition holds.     "
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
[value|expression returns integer] to .. by ..
```

## Identifier
Identifiers could include (UTF-8)letters, numbers and underlines. An identifier must starts with a letter or an underline. "k_many123", "__init", "哈哈" are all valid identifiers. Note that the third identifier is not recommended.

## Reserved Words
Reserved words are words that cannot be used as identifiers. These words are: and, async, await, break, by, const, continue, dynamic, else, emun, exec, false, for, goto, if, immut, implements, import, in, interface, match, mod, module, not, or, public, return, self, shl, shr, static, struct, super, to, true, unsafe, while, xor.   

## Goto
Goto is a keyword that is useless. It is listed in the reserved words because we do not want programmers to define it or implement it. As in many programming languages, it is harmful for designing.

## Constants
A constant is a value that binds to an identifier. The value must be known, or at least could be known at compile time. Defining constants uses the keyword "const".    
Grammar:
```
const [identifier] = [value|expression] 
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
immut [identifier] = [value|expression]
```
Here are some examples:
```
immut test_int = 12345
immut test_float = 1.23f128
immut test_string = "Howdy"
```
This method is also applicable for defining a pure function. We will talk about it later.

## Mutable Variables
A mutable variable is a value that need to be changed at runtime.    
Grammar:
```
[identifier] = [value|expression]
```
Here are some examples:
```
test_int = 12345
test_float = 1.23f128
test_string = "Howdy"
test_int = 123456 // Can be modified.
test_float = 12345 // Will throw an error.
// It seems to be a dynamically typed language, but it's actually a statically typed language. So changing variable's type is not permitted.
uninit_int = i32(uninit = true) // Uninitialized variable. Use constructor function.
```
This method is also applicable for defining an impure function. We will talk about it later.

## Arithmetic
Arithmetic is what you expected in mathematics.    
Binary operators: +, -, *, /, mod, **(power), =     
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
Binary operators: >, <, >=, <=, ==(equals), !=(not equals), and, or, xor, shl (shift left), shr (shift right)     
Unary operator: not
The operator "and", "or", "xor" and "not" are overloaded for logical operation. If both of the operands are booleans, then the return value is boolean. If one of the operand is boolean and one is integer, we will regard "true" as 1 and "false" as 0 to do bitwise operation.   
Remark: The priority of all the logical operations are the same, and lower than bitwise operation. Then priority of all the bitwise operations are the same, and lower than arithmetic operations.
Here are some examples:
```
a = 2 and 3 // 2
b = true or false and true // true
c = 2 + (5 and 3) // 1
d = 1 shl 5 // 32
```

## Control Flows
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
a = "apple"
match a {
    "apple", "pear", "orange" -> println "fruit"
    "tomato", "potato", "garbage" -> println "vegitable"
    else -> println "unknown"
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
// Or initialize with array literal
[opt][modifier] [/opt][identifier] = \[[element][opt], ..[/opt]...\]
// Or do not initialize
[opt][modifier] [/opt][identifier] = array<[type]>([dimension],[opt] ..,[/opt]...uninit = true)
// Or initialize with default value (0 for integers, 0.0 for floats and false for boolean)
[opt][modifier] [/opt][identifier] = array<[type]>([dimension],[opt] ..,[/opt]...)
```
Example:
```
a = [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]] // a 4 * 3 array
b = array<f64>(100, uninit = true) // a f64 array without initializing.
c = array<char>(50) // a char array initialized with '\0'.
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
c = a[5][5] // Out-of-bound error. Can be closed.
```