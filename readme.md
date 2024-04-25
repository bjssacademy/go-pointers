# Go Pointers

In Go, a *pointer* is a *variable* that stores the *memory address* of another variable. 

## WTF is a memory address?

Good question.

Imagine your computer's memory as a large city, and each *byte* of memory is like a house in that city. Now, each house (byte) has its unique address, just like each house in a city has its own street address.

When you declare a variable in Go, it's like building a new house in the city. The variable's value is stored inside that house. Now, if you want to tell someone where that variable (house) is located in the memory (city), you give them its address, just like you would give someone the street address of a house.

### Why is this important?

TBC

## Okay, so let's continue about pointers

When you pass a variable to a function in Go, it's passed *by value* by default, meaning a *copy* of the variable is created. 

However, when you pass a *pointer* to a function, you're passing the *memory address* of the variable, allowing the function to *directly access and modify the original variable's value*.

## I'm sorry, what? By Value vs Pointer?

Okay, so in Go, when you pass a variable to a function, you can't change the original value without returning something from the function and updating the original variable.

Let's look at this code:

```go
package main

import "fmt"

func main() {
    x := 10
    fmt.Println("Before calling changeValue:", x)
    changeValue(x)
    fmt.Println("After calling changeValue:", x)
}

func changeValue(num int) {
    num = 20
    fmt.Println("Inside changeValue:", num)
}
```

You can run this code here: https://goplay.tools/snippet/lH5nK2xeUJt

In this example, `x` is passed to the `changeValue` function *by value*. This means a copy of `x` is made, and any changes made to `num` inside the `changeValue` function do not affect the original `x` variable outside the function. So, the output will be:

```
Before calling changeValue: 10
Inside changeValue: 20
After calling changeValue: 10
```

Now, if we provide a *pointer* we can update the value outside of the function (known as a side-effect):

```go
package main

import "fmt"

func main() {
    x := 10
    fmt.Println("Before calling changeValue:", x)
    changeValue(&x)
    fmt.Println("After calling changeValue:", x)
}

func changeValue(ptr *int) {
    *ptr = 20
    fmt.Println("Inside changeValue:", *ptr)
}

```

In this example, `x` is passed to the `changeValue` function *by reference*, using a *pointer* (we user the ampersand `&` to say we "want to provide the memory address of the original to be acted upon"). 

This means the memory address of `x` is passed to the function, allowing it to directly modify the value of `x`. So, any changes made to `x` inside the `changeValue` function affect the original `x` variable outside the function. The output will be:

```
Before calling changeValue: 10
Inside changeValue: 20
After calling changeValue: 20
```

## Using Pointers in Go

In Go, the asterisk (`*`) and ampersand (`&`) have specific meanings when used with pointers

### Asterisk (*)

The asterisk is used to declare a pointer variable and to dereference a pointer.

When declaring a pointer variable, you use the asterisk before the type to indicate that the variable is a pointer to that type. For example:

```go
var ptr *int // ptr is a pointer to an integer
```

When you have a pointer variable, you can use the asterisk to access the value stored at the memory address pointed to by the pointer. This operation is called *dereferencing*. For example:

```go
x := 10
ptr := &x      // ptr stores the memory address of x
fmt.Println(*ptr) // dereferencing ptr to access the value of x
```

### Ampersand (&)

The ampersand is used to *get* the memory address of a variable.

When you place the ampersand before a variable, you get the *memory address* where that variable is stored. 

For example:

```go
x := 10
ptr := &x // ptr stores the memory address of x
```

A *memory address* is different to the value contained within. As an example, if you printed out `ptr` it would look like:

```
0xc000012028
```

You need to *dereference* it to get the value it contains at that address.

## Back To Our Houses

When you use the ampersand (`&`) operator in Go to get the memory address of a variable, it's like asking for the street address of a house. You get back a value that represents the location of the variable in memory.

Then, when you use a pointer variable (declared with an asterisk (`*`)), it's like having a piece of paper with the street address written on it. You can use that piece of paper to find and access the value stored in the variable's memory location, just like you can use the street address to find and access a house in the city.

So, memory addresses in programming are like street addresses in a city — they uniquely identify the location of data in the computer's memory.

## Other Languages

Some languages, especially OO-based ones, pass by reference by default, as side-effects and mutation are a key part of them (C#, Java). So it's important to know if you are passing a copy, or access to the original!