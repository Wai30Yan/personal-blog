---
title: "Restaurant Web Application"
date: 2023-11-17
draft: false
description: "All the shortcodes available in Blowfish."
slug: "fullstack-web-app"
tags: ["next-js", "spring-boot-3", "spring-security-6", "google-cloud"]
showEdit: false
showAuthor: true
---

{{< lead >}}
This is a fun article for those who are interested in learning Go. This tutorial will show how to parse JavaScript Object Notation(JSON) with Go `struct` type, Text or HTML Templates with Go standard library. [Github](https://github.com/Wai30Yan/go-projects-for-personal-blogs/tree/main/net) repo for this article.
{{</ lead >}}

### Introduction
Go has four types of data structure, arrays, slices, maps and struct. In this tutorial, we will use `struct`. To get a json object, we'll use my favorite api, [JokeAPI](https://sv443.net/jokeapi/v2/). I use this whenever I am learning new technologies such as `React`, `Flutter`, or backend development. 

Then, we will extract the json object from the http response and decode it to the `struct`. Finally, we'll use `template` libraries to save these jokes as both text and HTML files.

### JSON returned by the API
```go
var url string = "https://v2.jokeapi.dev/joke/Programming"
```

{{< figure
    src="featured.png"
    alt="rest-1"
    default=true
>}}
