---
title: "Admin Page for the Restaurant Web App with Next.js and Spring Boot"
date: 2023-11-17
draft: false
description: "All the shortcodes available in Blowfish."
slug: "admin-dashboard-restaurant"
tags: ["next-js", "spring-boot-3", "spring-security-6", "google-cloud"]
showEdit: false
showAuthor: true
---

{{< lead >}}
This article will focus on what the admin users can do on the Restaurant Web App. For demo, please use these credentials to login.

username - **bruce** <br>
password - **iambatman**

* [Demo](https://restaurant-web-app-frontend.vercel.app/admin/dashboard) 
* [Code for NextJS](https://github.com/Wai30Yan/restaurant-web-app-frontend)
* [Code for Spring Boot](https://github.com/Wai30Yan/restaurant-web-app-backend)

{{< figure
    src="admin1.png"
    alt="rest-1"
    default=true
>}}

The journey begins with a secure login. Only authorized administrators can access the Admin Page. Once you've proven your credentials, the backend sends a secure JWT (JSON Web Token), unlocking the gateway to culinary control.


### Admin Features Demostration

* After login, admin will be welcomed with the table displaying the list of booking made by guest users.

{{< figure
    src="admin2.png"
    alt="rest-1"
    default=true
>}}

* The admin can see more information such as ordered items, data and time about individual bookings

{{< figure
    src="admin3.png"
    alt="rest-1"
    default=true
>}}

* Admin can navigate to menu item page by using buttons in the navigation bar. There, admin can create or delete the items.

{{< figure
    src="admin4.png"
    alt="rest-1"
    default=true
>}}

* In the modal, admin can fill up the form and include a JPG photo (only).

{{< figure
    src="admin5.png"
    alt="rest-1"
    default=true
>}}

* After item is added successfully, **react-query** will validate the data and notify the admin. The page doesn't need to be refresh to see the changes.

{{< figure
    src="admin6.png"
    alt="rest-1"
    default=true
>}}

* Deleting the item also works the same way. But there is more happening in the background which is explained in later part.

{{< figure
    src="admin7.png"
    alt="rest-1"
    default=true
>}}

* Lastly, the admin will be taken to logout page, JWT token will be deleted from localstorage and won't have access to data when the Logout button is clicked.


### The Tech Ensemble Behind the Scenes
The part will only explain about the Spring Boot backend as the frontend was explained in the other article.

#### Java Spring Boot 3 and Spring Security 6: The Backend Guardians
Building the web server to handle HTTP requests was quite easy and straight forward as Spring Boot was already working on the API routes under the hood. The hard part, for someone new to Spring Boot, was working with Java Bean and understanding dependency circle.


#### Files Storing in MongoDB
I decided to use MongoDB instead of S3 bucket for storing the photo. So, I had to learn how MongoDB store files first which was the easy part. Then, I just need to associate the menu-items documents with the right photos.<br> <br>And moreover, I had to make sure when admin deletes a menu item the right photo is deleted from the storage and also remove the item and update the total price from the bookings accordingly.


#### Authenticated API endpoints
Admin API endpoints can only be process if the HTTP header contains JWT token. As I didn't know some of the functions in Spring Security 5 were deprecated, learning Spring Security 6 took a few days and I was watching Spring Security 5 tutorials for days.

#### Deploying the Backend Server
I used Google Cloud for the deployment with `.jar` file produced with `maven` which I found it is way simpler and straightforward compare to AWS ElasticBeanstalk. <br><br>
Initially, I was trying to deploy with AWS. But at the end I found out choosing ElasticBeanstalk was not the right choice for a personal portfolio project. I'll write a separate article for this topic in near future. 