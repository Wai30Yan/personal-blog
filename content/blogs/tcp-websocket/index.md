---
title: "Building TCP Websockets with Go"
date: 2023-02-14
draft: false
description: "tutorial about how to build TCP websockets in Go"
slug: "web-socket"
tags: ["go", "concurrency", "web-sockets", "tcp"]
showEdit: false
showAuthor: true
showHero: false
---
{{< lead >}}
Websockets are very useful to have when you want real time communication. There are different types of websockets, TCP, UDP and UNIX. In this tutorial, we will go through a basic TCP websockets, both Client and Server. The client will be able to send text to the server (like a messaging app) and the server responses with an integer. Then, to spice things up, we will use concurrency so that the server can handle multiple socket connections from multiple clients.
{{< /lead >}}

### TCP server

First, let's start with building a TCP websocket server. Create a file name `server.go`. Then, add the following code.

```go
package main

import (
    "os"
)

func main() {
    arguments := os.Args
    if len(arguments) == 1 {
        fmt.Println("Please provide a port number")
    } 

    PORT := ":" + arguments[1]
    listener, err := net.Listen("tcp", PORT)

    if err != nil {
        fmt.Println(err)
        return
    }

    defer listener.Close()
    
}
```

### Accepting incoming TCP connections

```go
    for {
        conn, err := listener.Accept()
        if err != nil {
            fmt.Println(err)
            return
        }
    }
```

### Reading incoming texts

```go
    netData, err := bufio.NewReader(conn).ReadString('\n')
    if err != nil {
        fmt.Println(err)
        return
    }
    text := strings.TrimSpace(netData)

    if text == "STOP" {
        break
    }

    fmt.Println(text)
```


### Responding with an integer
```go
    response := "hello from server\n"
    conn.Write([]byte(string(response)))
```

### Building a TCP Client


### Handling Multiple Connections Concurrently