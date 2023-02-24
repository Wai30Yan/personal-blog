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
A `struct` is like an object. It has fields and data types. The fields name are declared with capital letters to export them. To associate each field with json field, literal string is used. 
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
**Notice** One of the fields, `Joke string`, was named the same as the `type Joke struct`. It is a bad practice & poor choice of word. But I decided to keep it there to remind myself this type of things can happen. Plus, I wanted to keep the same field name as the json object returned by API call.

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
	// output: {Programming twopart  Why do programmers prefer using the dark mode? Because light attracts bugs. 232 true}
}
```

### Calling the `getJokes()` in main()
To fetch 10 jokes, let's call the function in a forLoop. And append each joke into jokes slice.

```go
	var jokes []Joke
	for i := 0; i < 10; i++ {
		joke := getJokes()
		jokes = append(jokes, joke)
	}
```


### Declare template variables
The joke json objects decoded in `getJokes()` are not neatly formatted. It is also unclear what field each json object contains. Depending on the type of joke, it can contain `"joke"` or `"setup"` & `"delivery"` fields. To format them, we'll use templates.

Go's standard library comes with a package for that. There are two types of templates, `text/template` & `html/template`. The later can be used for simple frontend application. 

Inside the template, you will see double brackets, `{{ }}`, which are called `action`. They are very useful for accessing the object being passed to the template as dynamic data. That object can be access as `"{{.}}"`, a period. In our case, the dot will represent `Joke struct`. Then, we can access the fields of the object with dot notation like this `{{.Category}}`. 

__*IMPORTANT*__ make sure the spelling is correct.


#### Text template
```go
var templ = `{{range .}}-------------------------
Category: {{.Category}}
Type: {{.Type}}
Joke: {{.Joke}} 
SetUp: {{.SetUp}}
Delivery: {{.Delivery}}
ID: {{.ID}}
Safe: {{.Safe}}
{{end}}`
```
#### HTML template
```go
var templ = `{{range .}}
<h3>Category: {{.Category}}</h3>
<h5>Type: {{.Type}}</h5>
<p>Joke: {{.Joke}}</p> 
<p>Setup: {{.SetUp}}</p>
<p>Delivery: {{.Delivery}}</p> 
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
### Control flows inside templates

You can also execute logical flow inside `action` such as forLoop and if/else statements. The previous text template is good, but can be improved. Depending on the type of joke, some of the fields are blank. If the joke type is single, `SetUp` and `Delivery` fields are empty.

Let's fix that with if/else statement inside the template.

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

It's simple, if the object has `Joke` field, render it. Else, render `SetUp` & `Delivery`.