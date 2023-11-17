---
title: "Building a Restaurant Web App with Next.js and Spring Boot 3"
date: 2023-11-17
draft: false
description: "All the shortcodes available in Blowfish."
slug: "fullstack-web-app"
tags: ["next-js", "spring-boot-3", "spring-security-6", "google-cloud"]
showEdit: false
showAuthor: true
---

{{< lead >}}
Embarking on a journey through the digital culinary landscape, I present to you a showcase of innovation and flavor—my Next.js restaurant web app. This project seamlessly marries the frontend prowess of Next.js with TypeScript, orchestrating a delightful dance of flavors complemented by the robust backend duo of Java 17, Spring Boot 3 and Spring Security 6, all orchestrated by MongoDB.

* [Demo](https://restaurant-web-app-frontend.vercel.app) 
* [Code for NextJS](https://github.com/Wai30Yan/restaurant-web-app-frontend)
* [Code for Spring Boot](https://github.com/Wai30Yan/restaurant-web-app-backend)

### Two Worlds, One App: Guest and Admin
As the heart of this gastronomic voyage, our web app accommodates two distinct realms—the guest and the admin. While guests savor the immersive dining experience, administrators wield the power to curate the culinary offerings.

This article will focus on what the Guest user can do. The admin features will be explained in a different article. 

### Booking Elegance
Our guests embark on a journey of ease and elegance as they make reservations effortlessly. The booking system is designed with simplicity in mind, allowing users to select menu items and complete the reservation process seamlessly. 
* When users choose an item, the navigation bar will be updated with the total price of the selected items.
* The items can also be removed completely by clicking Remove. The button will only be enable if there is at least one item chosen.

{{< figure
    src="rest1.png"
    alt="rest-1"
    default=true
>}}



* Guests can also filter menu items based on categories main courses, side dishes, desserts, and beverages—right from the homepage.


{{< figure
    src="rest2.png"
    alt="rest-1"
    default=true
>}}

* After choosing the item, the user can fill up the information to make a reservation by clicking the button at right top corner.

{{< figure
    src="rest3.png"
    alt="rest-1"
    default=true
>}}

* And to continue with the booking process, the users will be taken to checkout page where they can review and finally submit the order.

{{< figure
    src="rest4.png"
    alt="rest-1"
    default=true
>}}

* Upon a successful booking process, the users will be taken back to homepage and a notification will pop up at the bottom.
{{< figure
    src="rest5.png"
    alt="rest-1"
    default=true
>}}


### The Tech Ensemble Behind the Scenes
#### Next.js and TypeScript: A Symphony of Frontend Excellence
The frontend of our web app is an orchestration of Next.js and TypeScript, combining the power of a React framework with the static typing prowess of TypeScript. This dynamic duo creates a smooth and responsive user interface that leaves a lasting impression.

#### Zustand: Crafting the Shopping Cart Experience
Enter Zustand, our secret ingredient for state management. It elegantly handles the intricate dance of quantities as guests add items to their bookings, akin to a curated shopping cart experience.

#### React Query: A Dynamic Data Affair
Our data fetching choreography is set in motion by React Query, steering away from the conventional getStaticProps. This dynamic approach ensures real-time updates, delivering a seamless experience for our guests.

#### Java Spring Boot 3 and Spring Security 6: The Backend Guardians
On the backend stage, Java Spring Boot 3 and Spring Security 6 stand as guardians, ensuring the security and integrity of our digital culinary haven. User authentication, role-based access control, and a secure foundation define the backend narrative.

#### MongoDB: The Culinary Database
Our digital kitchen relies on MongoDB to store and retrieve data efficiently. From user information to bookings and menu items, MongoDB ensures a seamless and scalable data storage solution.