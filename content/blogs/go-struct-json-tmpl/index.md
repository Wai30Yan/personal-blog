---
title: "(In Progress) Practicing Go with Struct, Json, API and Templates"
date: 2023-02-23
draft: false
description: "All the shortcodes available in Blowfish."
slug: "go-fun"
tags: ["go", "json", "struct", "templates"]
showEdit: false
showAuthor: true
showHero: false
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
    src="joke.png"
    alt="joke"
    default=true
>}}

### Declaring struct base on JSON
```go
type Joke struct {
	Category string `json:"category"`
	Type     string `json:"type"`
	Joke     string `json:"joke"`
	SetUp    string `json:"setup"`
	Delivery string `json:"delivery"`
	ID       int    `json:"id"`
	Safe     bool   `json:"safe"`
}
```

### Write a function that fetch jokes

```go
func getJokes() Joke {
	response, err := http.Get(url)
	if err != nil {
		log.Fatal(err)
	}
	defer response.Body.Close()

	var joke Joke	
	json.NewDecoder(response.Body).Decode(&joke)

	return joke
}
```

### Declare template variables
#### Text template
```go
var templ = `{{range .}}-------------------------
Category: {{.Category}}
Type: {{.Type}}
{{if .Joke}}Joke: {{.Joke}} {{else}}SetUp: {{.SetUp}}
Delivery: {{.Delivery}} {{end}}
ID: {{.ID}}
Safe: {{.Safe}}
{{end}}`
```
#### HTML template
```go
var templ = `{{range .}}
<h3>Category: {{.Category}}</h3>
<h5>Type: {{.Type}}</h5>
{{if .Joke}}<p>Joke: {{.Joke}}</p> {{else}}<p>Setup: {{.SetUp}}</p>
<p>Delivery: {{.Delivery}}</p> {{end}}
<p>ID: {{.ID}},  Safe: {{.Safe}}</p>
{{end}}`
```

### Parsing and Executing the templates

```go
	jk, err := template.New("joke").Parse(templ)
	if err != nil {
		log.Fatal(err)
	}
	report.Execute(os.Stdout, jokes)
```
