---
title: "Simple REST API server with Go"
date: 2023-02-09
draft: false
description: "All the shortcodes available in Blowfish."
slug: "simple-rest-api"
tags: ["golang", "rest-api", "backend"]
showEdit: false
showAuthor: true
showHero: false
---

{{< lead >}}
Representational State Transfer (`REST`) is a popular architecture in software development. It is used for communication between applications e.g. frontend and backend. Hypertext Transfer Protocol (`HTTP`) and JavaScript Object Notation (`JSON`) are used for communication.

The frontend/client send HTTP `requests` to the backend/server. The backend send `responses`. Both request and response use JSON object as message. JSON objects are very easy to read as they are formatted in `key-value` pairs.
{{</ lead >}}

### What we'll do?

In this article, we'll build a RESTful server without using third party libraries which is the beauty of `Go` as the standard library can do a lot of things. Some of the popular Go libraries are [Gin Web Framework](https://gin-gonic.com/), [Fiber](https://gofiber.io/) and [Gorilla](https://www.gorillatoolkit.org/)

You can find the Github repo [here](https://github.com/Wai30Yan/go-projects-for-personal-blogs/blob/main/simple-rest-api/server.go).

### What does a server do?
The server will handle incoming HTTP requests and perform `CRUD` (create, read, update, delete) operations. The server will have `endpoint` to operate HTTP requests. Finally, we will create a `Person` struct for data.

In this tutorial, we will define the `endpoints` as:
* `/get` to retrieve all person
* `/person/{id}` to retrieve a specific person based on `id`
* `/add` to create a new `Person`
* `/update/{id}` to update an existing `Person` in the list
* `/delete/{id}` to delete an existing `Person` in the list

### Declaring `Person` object for data

```go
type Person struct {
    ID      int     `json:"id"`
    Name    string  `json:"name"`
    Age     int     `json:"age"`
}

var People []Person
```
In the code shown above, we declare `Person` object and `People` as slice of `Person` (array of people). You can consider slice as array in `Go`.
The syntax ``` `json:"id"` ``` is to serialize the incoming `JSON` object. It is used for converting `JSON` objects to `Go struct` and vice versa. It is important to use the exact format and no spacing unless it is in double quote.

### Implementing the HTTP web server
The server will be implemented in the main function. For the purpose of dealing with multiple routes (different endpoints), a multipluxer is created using the standard library `mux := http.NewServeMux()` and used it in the server. 
```go
var PORT = ":8080"
func main() {
    mux := http.NewServeMux()

	s := &http.Server{
        Addr:         PORT,
        Handler:      mux,
        IdleTimeout:  10 * time.Second,
        ReadTimeout:  time.Second,
        WriteTimeout: time.Second,
	}
}
```
Don't forget to change the PORT number if you have services running on `:8080`.

### Starting the server
The code below will start the server on `localhost:8080`. However, you will see nothing at the moment as there is not any handler function that sends a response. 
```go
    fmt.Println("Servering running at", PORT)
    _ := s.ListenAndServe()
```
You can start the server with the command `go run {filename}.go`. 

### Retrieving all person (Read operation)
Retrieving all person is the simplest job as it does not have params or query in the URL. Just a simple `HTTP GET request` will do the job.

```go
func getPeople(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Endpoint Hit: getAllPerson")
	json.NewEncoder(w).Encode(People)
}
```
Let's breakdown the code `json.NewEncoder(w).Encode(People)`.
* `json` is the standard library package. `NewEncoder()` and `Encode()` are functions that come with it.
* `Encoder()` send back a response to where the request came from. See `w`, `ResponseWriter` as the arguement.
* `Encode()` serialized the `People` slice to include it in HTTP response. See `People` we declared early as the arguement.

### Retrieve a specific person (Read operation)
This operation is similar to the pervious one except the URL needs to have ID so that the server knows which `Person` to send back.

```go
func getPerson(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Endpoint Hit: getPerson")
	vars := strings.Split(r.URL.Path, "/")
	param := vars[len(vars)-1]
	id, _ := strconv.Atoi(param)
	for _, person := range People {
		if person.ID == id {
			json.NewEncoder(w).Encode(person)
		}
	}
}
```

The URL `http://localhost:8080/person/1` will return `Person` with `ID: 1`.
First, the function will extract `1` from the URL and convert it to integar. Then, use that value to find the `Person` with the same `ID` and send back a response with it.

### Updating a person (Update Operation)
Update function works same way as `getPerson()`, it extracts the `ID` from the URL. And, create a `updatedPerson` using that ID. Then, add the updated value from `HTTP request body` to the `updatedPerson` object.

```go
func updatePerson(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Endpoint Hit: updatePerson")
	vars := strings.Split(r.URL.Path, "/")
	param := vars[len(vars)-1]
	id, _ := strconv.Atoi(param)

	updatedPerson := Person{
		ID: id,
	}
	json.NewDecoder(r.Body).Decode(&updatedPerson)
	for i, person := range People {
		if person.ID == id {
			People[i] = updatedPerson
			json.NewEncoder(w).Encode(People[i])
			return
		}
	}
}
```
And finally, loop through the slice, look for the element with the same ID to replace with the Person object wiht updated values.

### Deleting a person (Delete Operation)
Deleting the element works the same way as update operation. 
* extract `ID`
* loop through the slice
* find the right element
* and remove the element

```go
func deletePerson(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Endpoint Hit: deletePerson")
	vars := strings.Split(r.URL.Path, "/")
	param := vars[len(vars)-1]
	id, _ := strconv.Atoi(param)

	for key, person := range People {
		if person.ID == id {
			People = append(People[:key], People[key+1:]...)

		}
	}
}
```
However, it is more like updating the existing slice. When it found the right element to remove, it cut the slice in half and concatanates the two pieces back together.

### Testing the APIs
If you don't have a frontend, you can use a VScode extension `Thunder Client` to test the APIs. It is convenient as you can perform them in the same window unlike `Postman` application.

Before sending the HTTP requests, let's add some dummy data, `Person` objects, in `People` slice. Since we are not using a database in this tutorial, we will hardcode it. Add the following code before creating the server.

```go
	People = []Person{
		{1, "Wai Yan", 27},
		{2, "John", 48},
		{3, "Jane", 35},
	}
```
This will add three `Person` objects to `People` slice.


Then we implement the HTTP handlers in the `main` function for different endpoints as we defined periviously. In the below code, the strings are endpoints and `http.HandlerFunc` is calling the respestive functions to operate CRUD.

```go
	mux.Handle("/get", http.HandlerFunc(getPeople))
	mux.Handle("/person/", http.HandlerFunc(getPerson))
	mux.Handle("/add", http.HandlerFunc(addPerson))
	mux.Handle("/update/", http.HandlerFunc(updatePerson))
	mux.Handle("/delete/", http.HandlerFunc(deletePerson))
```





{{< alert >}}
This tutorial doesn't use a database. So, if you restart the server, the `People` slice will be reset.
{{</ alert >}}