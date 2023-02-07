---
title: "Reciever function in Go"
date: 2023-02-07
draft: false
description: "All the shortcodes available in Blowfish."
slug: "reciever-func"
tags: ["golang", "reciever-func"]
showEdit: false
showAuthor: true
showHeadingAnchors: false
---

Object Oriented Programming is a little different in Go. Instead of `class`, Go has `struct` which is like an object.

### Create a `struct`

First you create a `struct` object like this:

```go {linenos=true}
type Person struct {
    name string
    age  int64
}
```
### Write the reciever function

Then, you pass the `struct` in a function like this:

```go {linenos=true}
func (p *Person) aFunc(age int, name string) {
    p.age = age
    p.name = name
}
```

### Finally, calling the reciever func using the struct

Then you can call the function using the `struct` as a reciever like this:

```go
func main() {
    p = Person{0, ""}
    p.aFunc(27, "WaiYan")
}
```