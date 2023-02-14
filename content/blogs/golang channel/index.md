---
title: "Concurrency with Goroutines"
date: 2023-02-07
draft: false
description: "All the shortcodes available in Blowfish."
slug: "concurrency"
tags: ["go", "go-routines", "concurrency"]
showEdit: false
showAuthor: true
showHero: false
---

Go routines are very useful when you want the program to run fast. However, do not mistake concurrency with parallelism.

To create a goroutine, simple use `go` keyword
```go {linenos=true}
func aFunc() {
    fmt.Println("Hello World")
}

go aFunc()
```

