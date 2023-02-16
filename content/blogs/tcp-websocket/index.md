---
title: "(Work in Progress) Building TCP Websockets with Go"
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
Websockets are very useful to have when you want real time communication like messaging app or an online game server. There are different types of websockets, TCP, UDP and UNIX. In this tutorial, we will go through a basic TCP websockets, both Client and Server. 
{{< /lead >}}

## Currently, it's only code. Explaination will be added soon.
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
        fmt.Println("Please provide a port number\n")
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

I'm using command-line arguement for PORT number. But you can declare and assign a value in the code if you want. When you run the `go` file, you use the command `go run main.go`. `main.go` is the first arguement with `index 0`. Just include the port number at the end like this: `go run main.go 1234`. 

This code is `listener, err := net.Listen("tcp", PORT)` is responsible for declaring a listener. The listener waits for incoming connections from clients. It is part of `net` package from standard library. The `Listen` func accept two parameters, `protocol` and `port number`.

In the example, I used "tcp". There are other options like "udp" and "unix".

### Accepting incoming TCP connections
To establish the communication, the listener needs to accept the incoming connection. The `listener.Accept()` returns `net.Conn` which represents a connection and an error. The `conn` object can then be used for reading and writing data.

```go
conn, err := listener.Accept()
if err != nil {
    fmt.Println(err)
    return
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

fmt.Println("client: " + text)
```


### Responding with an integer
```go
response := "hello from server\n"
conn.Write([]byte(string(response)))
```

### Building a TCP Client

```go
func main() {
    arguments := os.Args
    if len(arguments) == 1 {
        fmt.Println("Please provide host:port.")
        return
    }
	connect := arguments[1]
}
```

### Establishing connection with the server
```go
tcpAddr, err := net.ResolveTCPAddr("tcp4", connect)
if err != nil {
    fmt.Println(err)
    return
}
conn, err := net.DialTCP("tcp4", nil, tcpAddr)
```

### Sending and Recieving texts
```go
for {
    reader := bufio.NewReader(os.Stdin)
    fmt.Print(">> ")
    text, _ := reader.ReadString('\n')
    fmt.Fprintf(conn, text+"\n")
    message, _ := bufio.NewReader(conn).ReadString('\n')
    fmt.Print("->: " + message)
    if strings.TrimSpace(string(text)) == "STOP" {
        fmt.Println("TCP client exiting...")
        return
    }
}
```


### Handling Multiple Connections Concurrently