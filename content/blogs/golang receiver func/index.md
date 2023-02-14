---
title: "Struct method aka Reciever function in Go"
date: 2023-02-07
draft: false
description: "All the shortcodes available in Blowfish."
slug: "struct-method"
tags: ["go", "reciever-func", "struct-method"]
showEdit: false
showAuthor: true
showHero: false
---

Object Oriented Programming is a little different in Go. Instead of `class`, Go has `struct` which is like an object.

### Create a `struct`

First you create a `struct` object like this:

```go {linenos=true}
type Person struct {
    name string
    age  int
}
```
### Declaring struct method

Then, you pass the `struct` in a function like this:

```go {linenos=true}
func (p *Person) addInfo(name string, age int) {
    p.name = name
    p.age = age
}
```

### Finally, calling the method using the struct

Then you can call the function using the `struct` as a reciever like this:

```go
func main() {
    p = Person{}    
    fmt.Println(p) // output: { 0}
    p.addInfo("Wai Yan", 27)    
    fmt.Println(p) // output: {Wai Yan 27}
}
```

### Why use reciever function?

The function can only be called using dot operator with `p` struct which is in a way an object. 