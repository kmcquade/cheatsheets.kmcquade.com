# Golang

##

## Variables <a href="variables" id="variables"></a>

### Built-in types <a href="built-in-types" id="built-in-types"></a>

* `bool`
* `string`
* `int`, `int8`, `int16`, `int32`, `int64`
* `uint`, `uint8`, `uint16`, `uint32`, `uint64`
* `uintptr`
* `byte`
* `rune`
* `float32`, `float64`
* `complex64`, `complex128`

### Declare a variable <a href="declare-a-variable" id="declare-a-variable"></a>

```
var name string
```

### Assign a variable <a href="assign-a-variable" id="assign-a-variable"></a>

```
name = "Bob"
```

### Declare and assign a variable (with type inference) <a href="declare-and-assign-a-variable-with-type-inference" id="declare-and-assign-a-variable-with-type-inference"></a>

```
name := "Bob"
```

### Declare a constant (can be string, char, or numeric types) <a href="declare-a-constant-can-be-string-char-or-numeric-types" id="declare-a-constant-can-be-string-char-or-numeric-types"></a>

```
const Answer = 42
```

### Convert between types <a href="convert-between-types" id="convert-between-types"></a>

```
var f float64 = float64(i)
f := float64
```

## Arrays and Slices <a href="arrays-and-slices" id="arrays-and-slices"></a>

### Declare a static array <a href="declare-a-static-array" id="declare-a-static-array"></a>

```
var coefs [128]int
```

### Dynamically allocate an array and get a pointer <a href="dynamically-allocate-an-array-and-get-a-pointer" id="dynamically-allocate-an-array-and-get-a-pointer"></a>

```
p := new([64]int)
```

### Create slice (dynamically sized array) with length and capacity of 7 <a href="create-slice-dynamically-sized-array-with-length-and-capacity-of-7" id="create-slice-dynamically-sized-array-with-length-and-capacity-of-7"></a>

```
s := make([]int, 0, 7)
```

### Create empty slice with capacity of 7 <a href="create-empty-slice-with-capacity-of-7" id="create-empty-slice-with-capacity-of-7"></a>

```
s := make([]int, 7)
```

### Convert entire array to slice (doesn’t copy the underlying array) <a href="convert-entire-array-to-slice-doesn-t-copy-the-underlying-array" id="convert-entire-array-to-slice-doesn-t-copy-the-underlying-array"></a>

```
a := a[:]
```

### Make a slice from a segment of array or existing slice <a href="make-a-slice-from-a-segment-of-array-or-existing-slice" id="make-a-slice-from-a-segment-of-array-or-existing-slice"></a>

```
s := a[1:4]
```

## Structs <a href="structs" id="structs"></a>

### Declare a struct type <a href="declare-a-struct-type" id="declare-a-struct-type"></a>

```
type Coordinate struct {
  Latitude float32
  Longitude float32
}
```

### Instatiate a struct <a href="instatiate-a-struct" id="instatiate-a-struct"></a>

```
e := Coordinate{
  Latitude: 1.0,
  Longitude: 2.0,
}
```

### Structs can be nested for composition <a href="structs-can-be-nested-for-composition" id="structs-can-be-nested-for-composition"></a>

```
type Region struct {
  NWCorner Coordinate
  SECorner Coordinate
}
```

## Functions <a href="functions" id="functions"></a>

### main function is a starting point of every program <a href="main-function-is-a-starting-point-of-every-program" id="main-function-is-a-starting-point-of-every-program"></a>

```
package main
func main() { ... }
```

### Assign anonymous function to variable <a href="assign-anonymous-function-to-variable" id="assign-anonymous-function-to-variable"></a>

```
pred := func() bool {
  return x > 200
}
```

### Define a simple function <a href="define-a-simple-function" id="define-a-simple-function"></a>

```
func functionName() { ... }
```

### Define a function with arguments <a href="define-a-function-with-arguments" id="define-a-function-with-arguments"></a>

```
func functionName(firstName string, age int) { ... }
```

### Define a function with a return type <a href="define-a-function-with-a-return-type" id="define-a-function-with-a-return-type"></a>

```
func howMuch() int {
  return 1
}
```

### Define function with multiple return types <a href="define-function-with-multiple-return-types" id="define-function-with-multiple-return-types"></a>

```
func getPerson() (string, string) {
  return "John", "Smith"
}
```

### Use named function results <a href="use-named-function-results" id="use-named-function-results"></a>

```
func getPerson() (name string, surname string) {
  name = "John"
  surname = "Smith"
  return
}
```

## Flow control <a href="flow-control" id="flow-control"></a>

### if-then-else <a href="if-then-else" id="if-then-else"></a>

```
if a <b {
  fmt.Println("smaller")
} else if a > b {
  fmt.Println("larger")
} else {
  fmt.Println("equals")
}
```

### C-style for loop <a href="c-style-for-loop" id="c-style-for-loop"></a>

```
for i := 1; i < 5; i++ {
    sum += i
}
```

### While loop (use `continue` to skip to next iteration or `break` to cancel\`) <a href="while-loop-use-continue-to-skip-to-next-iteration-or-break-to-cancel" id="while-loop-use-continue-to-skip-to-next-iteration-or-break-to-cancel"></a>

```
for i <10 {

}
```

### Iterative loop <a href="iterative-loop" id="iterative-loop"></a>

```
for {

}
```

## Methods <a href="methods" id="methods"></a>

### Method-by-value (copies the entire struct to stack, cannot mutate) <a href="method-by-value-copies-the-entire-struct-to-stack-cannot-mutate" id="method-by-value-copies-the-entire-struct-to-stack-cannot-mutate"></a>

```
package main

import "fmt"

type Person struct {
    firstName string
    lastName  string
}

func changeName(p Person) {
    p.firstName = "Bob"
}

func main() {
    person := Person {
        firstName: "Alice",
        lastName: "Dow",
    }

    changeName(person)

    fmt.Println(person)
}
```

### Method-by-reference (refers to origin struct through pointer - can mutate) <a href="method-by-reference-refers-to-origin-struct-through-pointer-can-mutate" id="method-by-reference-refers-to-origin-struct-through-pointer-can-mutate"></a>

```
package main

import "fmt"

type Person struct {
    firstName string
    lastName  string
}

func changeName(p *Person) {
    p.firstName = "Bob"
}

func main() {
    person := Person {
        firstName: "Alice",
        lastName: "Dow",
    }

    changeName(&person)

    fmt.Println(person)
}
```

### Call a method <a href="call-a-method" id="call-a-method"></a>

StackOverflow explanation [here](https://stackoverflow.com/questions/26142074/call-a-function-from-another-package-in-go#answer-45861276).

#### File 1: main.go (located in MyProj/main.go) <a href="file-1-main-go-located-in-myproj-main-go" id="file-1-main-go-located-in-myproj-main-go"></a>

```
package main

import (
    "fmt"
    "MyProj/functions"
)

func main(){
    fmt.Println(functions.GetValue())
}
```

#### File 2: functions.go (located in MyProj/functions/functions.go) <a href="file-2-functions-go-located-in-myproj-functions-functions-go" id="file-2-functions-go-located-in-myproj-functions-functions-go"></a>

```
package functions

// `getValue` should be `GetValue` to be exposed to other packages.
// It should start with a capital letter.
func GetValue() string{
    return "Hello from this another package"
}
```

## Learning Resources

{% embed url="https://gobyexample.com/" %}

{% embed url="https://quii.gitbook.io/learn-go-with-tests/" %}

## Notes

#### Short variable declarations

Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type.

Outside a function, every statement begins with a keyword (`var`, `func`, and so on) and so the `:=` construct is not available.

#### Constants

Constants are declared like variables, but with the `const` keyword.

Constants can be character, string, boolean, or numeric values.

Constants cannot be declared using the `:=` syntax.

### Characteristics of a Golang test function

* The first and only parameter needs to be `t *testing.T`
* It begins with the word Test followed by a word or phrase starting with a capital letter.
* (usually the method under test i.e. `TestValidateClient`)
* Calls `t.Error` or `t.Fail` to indicate a failure (I called t.Errorf to provide more details)
* `t.Log` can be used to provide non-failing debug information
* Must be saved in a file named `something_test.go` such as: `addition_test.go`

\




