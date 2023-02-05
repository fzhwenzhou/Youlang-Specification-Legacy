# Standard Library of Youlang

## About This Document
This document only describes the standard library of Youlang.     
About basic grammar, please refer to "YouLang.md" in the same directory.    
The describing techniques are also described in "YouLang.md".

## IO Module
The package name for this module is "io".

### Output
Output stream package is encapsulated, so you cannot access it directly.     
Public Methods: 
```
write = (buf: u8[]) -> u64
write_fmt = (fmt: u8[], args...) -> u64
flush = () -> void
```

#### Standard Output
Use "stdout" method in "io" module to get an output stream instance. You can also directly use it.      
There are some macros to simplify this process.      
```
print! = (obj){}
println! = (obj){}
print_fmt! = (fmt, args...){}
```
These macros are in general namespace.

### Input
Input stream package is also encapsulated, so you cannot access it directly.     
Public Methods:
```
read = (buf: u8[]) -> u64
read_fmt = (fmt: u8[], args...) -> u64
```

#### Standard Input
Use "stdin" method in "io" module to get an input stream instance. You can also directly use it.     
Actually methods defined in input stream package is hard to use, so we also provide some macros to simplify.
```
get! = (obj){} // Delimated by space
getln! = (obj){}
get_fmt! = (fmt, args...){}
```
These macros are also in general namespace.