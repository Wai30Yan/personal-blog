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
Websockets are very useful to have when you want real time communication like messaging app or an online game server. There are different types of websockets, TCP, UDP and UNIX. In this tutorial, we will go through a basic TCP websockets, both Client and Server. 
{{< /lead >}}

## Building TCP server

First, let's start with building a TCP websocket server. Create a file name `server.go`. Then, add the following code. You can find the Github repository [here](https://github.com/Wai30Yan/go-projects-for-personal-blogs/tree/main/tcp-socket).

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
```go
conn, err := listener.Accept()
```
To establish the communication, the listener needs to accept the incoming connection. The `listener.Accept()` returns `net.Conn` which represents a connection and an error. The `conn` object can then be used for reading and writing data.


### Reading incoming texts
We'll use `bufio` package for reading message. `conn` object we saw previously can be used as io.Reader or io.Writer which can be passed into `NewReader()` to read the incoming message. It is also a good habit to remove extra white spaces.
```go
netData, err := bufio.NewReader(conn).ReadString('\n')

text := strings.TrimSpace(netData)

if text == "STOP" {
    break
}

fmt.Println("client: " + text)
```
The server will stop the socket connection if the client sent `STOP` as message (case sensitive).

### Responding 
The server sends a response when the `conn` object call `Write()`. But, the text has to be converted to `byte`. In the `byte()`, you can pass in the message as string. You can also pass `json` using json package. 

In the example, the server is responding with the time and message the client sent.
```go
conn.Write([]byte("You sent "+ temp + " at " + time.Now().Format("15:04:05\n")))
```


## Building a TCP Client
The code for client is partially similar to that of server except that command-line arguement should include url address with port number `go run client.go localhost:8080`. 

*Notice, for the server, we only included port number `go run server.go 8080`*

The client also must use the same port number as the server as it needs to know at what port tcp server is running on.

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

To establish a connection, `ResolveTCPAddr()` from net package is called using the user input. If the host in the address parameter is not a literal IP address or the port is not a literal port number, ResolveTCPAddr resolves the address to an address of TCP end point.

Then the client dial the tcp server with `DialTCP()` using the address resolved by the previous function. Notice `"tcp"` as parameter. You can also use "tcp4", "tcp6" or "udp" as well. 

You can also use `Dial()` and you don't need to use `ResolveTCPAddr()` or `DialTCP()`. The client `Dial()` and the server accepts with listener `listener.Accept()`.

```go
tcpAddr, _ := net.ResolveTCPAddr("tcp", connect)
conn, _ := net.DialTCP("tcp", nil, tcpAddr)
```

### Sending and Recieving texts
Unlike the server, the client will both read and write messages. But same package `bufio` will be used for both purposes. 

This code will read the string the user adds. `NewReader(os.Stdin)` to accept and read user input. The second line is to send the user input to server. It uses `fmt` package with `conn` object as `io.Writer` to send the text.
```go
text, _ := bufio.NewReader(os.Stdin).ReadString('\n')
fmt.Fprintf(conn, text+"\n")
```
The `conn` object will then be used with `bufio` to read the messages coming back from the server. And print the respond with `fmt`.
```go
message, _ := bufio.NewReader(conn).ReadString('\n')
fmt.Print("->: " + message)
```

Full for loop

```go
for {
    fmt.Print(">> ")
    text, _ := bufio.NewReader(os.Stdin).ReadString('\n')
    fmt.Fprintf(conn, text+"\n")
    message, _ := bufio.NewReader(conn).ReadString('\n')
    fmt.Print("->: " + message)
    if strings.TrimSpace(string(text)) == "STOP" {
        fmt.Println("TCP client exiting...")
        return
    }
}
```


## Handling Multiple Connections Concurrently

We'll use concurrency in server to handle multiple clients. We just need to extract the for loop in `main()` and put it to a seperate function. Then, call this function as a goroutine in `main()`.