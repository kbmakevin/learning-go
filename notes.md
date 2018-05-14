# Go Getting Started
- fully compiled language
    - like C++ performance
- simple and fun to use
    - borrowed from python
    - only 25 keywords
- concurrency
    - built to be concurrent
- statically typed
- cute, kind of quirky, lots of fun to play with
    - like to go gopher

- main module, main function defines entry point of the application
- functions are first class citizens in GO
    - do not need to define them within a class
- go doesn't let you have any unused imports/vars, will prevent code from running
    - cleaner code

- first program
```
package main

import (
	"fmt"
)

var (
	message string
)

func main() {
	println(message)
}

func init() {
	message = "Hello Go"
}
```
- slices are very similar to arrays
- make function to create a slice or map

- can put expressions in switch case, like if/else branches
- don't need brackets around condtiions like other languages
- can use for loop to act as while lloop
    - simply do not include any expressions
    - ```for {...}```
- foreach loop for collections
```
s:=[]string{"foo","bar","buz"}

for idx, v := range s {
    println(idx,v)
    <!--
        output:
        0 foo
        1 bar
        2 buz
     -->
}

<!-- can also do this for maps, k,v key value instead of index, value -->
```
## functions and parameters
- passing in by value
```
func sayHello(message string){
    println(message)
}
```
- passing in by reference
```
func sayHello(message *string){
    println(*message)
}
```
- variadic parameters
    - accepts 0 or more values of a specified type
```
func sayHello(messages ...string) {
    for _, message:=range messages{
        println(message)
    }
}
```
## Return Values
- single return value
```
func add(terms ...int) int{
    result:=0
    for _, term:=range terms{
        result+=term
    }
    return result
}
```
- multiple return values
```
func add(terms ...int) (int,int) {
    result:=0
    for _, term:=range terms{
        result+=term
    }
    return len(terms), result
}
```
- named return values
    - increase redability of code
```
func add(terms ...int) (numTerms int,sum int) {
    for _, term:=range terms{
        sum+=term
    }
    numTerms=len(terms)
    return
}
```
## Anonymous Functions
- func are first class citizens in go, dont need class
- func are independent types; e.g.
```
addFunc:=func (terms ...int) (numTerms int,sum int) {
    for _, term:=range terms{
        sum+=term
    }
    numTerms=len(terms)
    return
}
```

## Object-Oriented Programming
- diff in go than other oop langauges
    - uses STRUCTS
        - structs !== classes
- structs, container that holds arbitrary data types called fields
```
package main
func main(){
    foo:=myStruct{}
    foo.myField="bar"
    println(foo.myField)

    <!-- OR -->
    <!-- order of initialized properties = order definition of fields in struct -->
    moo:=myStruct{"bar"}
}

type myStruct struct{
    myField string
}
```
- creates obj on local execution stack
- better to create large obj on heap instead (use new keyword)
```
foo:= new(myStruct)
foo.myField="bar"
```
- using new keyword, we create a reference type, the other way creates value type
- go keeps track of references for us, will not garbage collect until it knows that there is nothing still referencing it (even if the function finishes executing!)
- note we cannot use initizlizer {} with the new keyword
- instead of using new keyword, there is another way to do the same thing! (use ampersand sign)
```
foo := &myStruct{"bar"}
```
## Constructor Functions
```
type myStruct struct {
    myMap map[string]string
}

func newMyStruct() *myStruct {
    result := myStruct{}
    result.myMap = map[string]string{}

    return *result
}
```
## Methods
- need to specify struct before func name
```
type messageprinter struct {
    message string
}

func (my *messagePrinter) printMessage(){
    println(mp.message)
}
```
## Object Composition
- inheritance is not used in go, composition is used instead
```
func main(){
    emp := enhancedMessagePrinter{}
    emp.message="foo"
    emp.printMessage()
}

type messageprinter struct {
    message string
}

func (my *messagePrinter) printMessage(){
    println(mp.message)
}

type enhancedMessagePrinter struct {
    messagePrinter
}
```
## Asynchronous Programming
- goroutines and channels built-in to the language

### Goroutines
- app has multiple things to do at the same time
- concurrency != paralellilism
- concurrency, one cpu/thread, diff tasks run at different times?
    - go run time will schedule each task to run at certain amt of time and then switch tasks, eventually everything gets done but only one thing gets done at a time
- parallel uses multiple threads/cpu run diff tasks at same time
    - scheduler assigns each task to a CPU and allows them to run to completion
- to make a function call into a go routine, simply add the keyword **go** in front of the function call
    - this tells the app to run the function conrurently
- goroutines do not directly map to processor threads in the operating system
    - tends to be expensive
- instead goroutines start very small, in order to allow apps to work with thousands of them at a time
- by default go programs only run on one cpu
- `import "runtime"` to take adv of concurrency
- `runtime.GOMAXPROCS(8)` would make it run on 8 CPUs
- a problem which may occur from this is output may not match what we are expecting
    - e.g. if we are outputing the diff letters of alphabet, and use 8 cpus, the order will not neccessarily be in order a, b, c, d, e, f, g, ..., z because they will output depending on aailability of CPU
### Channels
- in many languages, multiple threads need to be managed in an application
    - fortunately, easier in go
- channels r constructs in go that lets msgs to be passed b/t two goroutines in a manner that ensures that the data is safely transferred b/t them
    - only one goroutine at a time owns the msg/its contents and can work with them
- channels ensure that msgs are delivered safely and in the order they were generated
```
...
func main(){
    runtime.GOMAXPROCS(8)

    ch := make(chan string)

    go abcGen(ch)
    go printer(ch)
    ...
}

func abcGen(ch chan string){
    for l := byte('a'); l <= byte('z'); l++ {
        ch <- string(l)
    }
}

func printer(ch chan string){
    // infinite for loop
    for {
        println(<- ch)
    }
}

```
- <- is the receive operator
- string(foo) is casting foo into type string

---

# The Go Programming Language
## Background Info
- Go is:
    - compiled (into machine code that runs on the processor),
    - garbage-collected (memory is managed for you, very fast - latency free),
    - concurrent
- Go's origins
    - 2007-2009 go in the works
    - 2012 go 1 released
- static language - goes all the way down to compiled machine code
    - but maintains ease of use as dynamic languages like python/ruby
- type-safe (an int will behave as int) and memory-safe
    - don't need to worry about these things
- latency free garbage collection
- fast compile time
- no inheritance!
    - prefer composition
    - complexity is introduced with inheritance, can not easily figure out where stuff is coming from
- no method overloading
    - usually confusing; would slow down compiling
- user defined types, but not within classes
- syntax similar to C but is different from C and C++
    - similarity is that it IS a SYSTEMS level language
    - this means it can replace C and C++
        - doesnt really use GUI much but good for things that need to work on variety of platforms, low level system e.g. web server, etc.
## Overview
- semi-colons are inserted by the compiler, we don't put semi-colons into the code
- packages similar to namespaces
    - way to modularize code
- imports is able to when to go for local packages or remote packages

## Variables, Types and Pointers
### basic types
- bool
- string
- int
- uint
- byte
- rune (int32), like a char
- float,
- complex (e.g. numbers that end with imaginary numbers)

### other types
- array
- slice
- struct
- pointer
- function
- interface
- map
- channel

NOTE:
- we can only declare functions using the `:=` inside of the main function

### Pointer Basics
- a type of var that contains memory address of another var
- integer pointer can only point to integers, same with other types etc.
    - pointers have their own types
```
message := "Hello Go World!"
var greeting *string = &message
```
- use * to denote a pointer
- use & to get mem addr of a variable
- if you did `*greeting = "hi"`, you would change content of message as well

## User Defined Types
- no classes in go, we use user defined types instead!
```
type Salutation struct {
    name string
    greeting string
}
```
- no getters and setters, simply access them as properties of the var you created from the user defined type
- note: we don't explictly define visibiltiy
    - anything that is a capital is PUBLIC, visible outside of the current package
    - anything lower case is not visible outside of the package
    - this keeps things simpler in go
- constants are different in go than other languages
```
const (
    PI = 3.14
    Language = "Go"
    A = iota
    B
    C
)
```
- iota represents successive untyped integer constants
- if you dont define value of constant, it assumes the value of the expression of the previous constant
- e.g. in here, A = 2, B = 3, C = 4

## Go Functions
- multiple return values
- use it like any other type
- function literals
    - can declare functions right inside other functions
    - takes context of where it is defined (i.e. inside the function) and it remembers it when it is used, and has access to that
        - this is called a closure

### Multiple Return Values
```
func CreateMessage(name, greeting string) (string, string) {
    return greeting + " " + name, "HEY!" + name
}

message, alternate := CreateMessage(salutation.name, salutation.greeting)

// We can ignore one of the return values by using _
_, alt2 := CreateMessage(salutation.name, salutation.greeting)
```

### Named Returns
- can make changes to the returns as though they were params and then simply use return statement and it will return their last contained value
```
func CreateMessage(name, greeting string) (message string, alternate string){
    message = greeting + " " + name
    alternate = "HEY!" + name
    return
}
```

### Variadic functions
- variable # of params of a certain type
    - has to come as last param, using ...
```
func CreateMessage(name string, greeting ...string) (message string, alternate string){
    //if we don't know the number of arguments we can use len()
    fmt.Println(len(greeting))
    message=greeting[0] + " " + name
    alternate=greeting[1] + " " + name
    return
}
```

### Function Types
- can pass in function as an arg to another function
    ```
    func Greet (salutation Salutation, do func(string)){
        ...
    }
    func Print(s string){
        fmt.Print(s)
    }
    ...
    Greet(s, Print)
    ```
- can make function types reusable by declaring them
    ```
    type Printer func(string)()

    func Greet(salutation Salutation, do Printer) {
        ...
    }
    ```
### Closure
- using context in which the func was created, it imprints that onto the function which is created
- we can create a function that creates other  functions that vary by parameter(s)
    ```
    type Printer func(string)()
    func Greet(salutation Salutation, do Printer) {
        ...
    }
    func CreatePrintFunction(custom string) Printer {
        return func(s string) {
            fmt.Println(s + custom)
        }
    }
    ...
    Greet(s, CreatePrintFunction("!!!"))
    ...
    ```

## Branching
### if statements
```
if [optional statement]; condition {
    block
}
```
-