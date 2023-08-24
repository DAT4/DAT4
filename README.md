# Projects 

This document is a collection of all the projects that I am working on or interested in contributing to. The first section consist of project made by others, and the second section is of my own projects.

## Others project

There are 3 project that I have been following and I am quite interested in at the moment.

### Matrix

The goal of matrix is to make a universal protocol for all messaging platforms, that makes it possible for different platforms to communicate while owning their own data.

I like matrix because I think they can make a change that will impact the world in a positive way.

I am following them on Github, and on Youtube where I can see the progress they have.

I would love to work with some of the stuff that Matrix is working on.

### Gokrazy

Gokrazy is similar to another project called u-root. It is taking the linux kernel and building Go applications on top of it (as the opperation system)

I have followed the maintainer of Gokrazy for some time, and I like how they are doing everything in golang.

I think it could be nice to use something like golang to build an alternative to Android and make a minimalist phone that is only using matrix for comunication and nothing else. I have already informed the maintainer of the library about my idea. He said he doesn't mind.

### netlink/wifi

Netlink wifi is a nice project in golang where they try to make a library with the functionality as `iw`

I like the project because they take something from linux that noone understand and makes it readable. The project is still very tiny and I am hoping to be able to contribute. 

I contacted the maintainer and asked for advice, and he told me just to reverse engineer the linux kernel. 

## My Projects

This is an overview of the projects I have been working on. I have a lot of projects, many small and a few larger. And I would like to create an overview to go back and see them and maybe finish some of them.

### My Workstation

I am using Arch linux at the moment with DWM that I have cuztomized, and NVIM with NVChad and some plugins for working with Golang, Python and Rust. I am considering changing to NixOS to have a more clean install. And I also don't think I need that much updates all the time.

For UML and diagrams I am using plantuml, and for any writing I use markdown.

### Cloud

The goal with the cloud is to have a server system with the following functionality:

- Git server (Gitea)
- CI (Drone)
- DevOps (Kubernetes, Traefik, ArgoCD)
- Matrix server (Chat, Notification)
- Taiga (Project Management)
- Postgres Database

Then all my projects would be uploaded on the git server with their deployment configurations, so that they can be deployed to the kubernetes cluster.

I would like to use Matrix, and Taiga more when working in teams.

I am reading about Ansible now, and I am considering using something like Ansible or Terraform, to make it possible to setup everything with one command. I am also very interested in making backups, which I don't do at the moment. I think i could use Rsync to send the data to a backup machine. 

I am also considering using OpenStack, but I did it before and I think I remember that it was using a lot of resources. And it was not really necessary with the current setup I have. So it can wait for now I guess. A reason to use OpenStack might be the integration with Ansible or Terraform.

### Auth server

The goal of the auth server is to make a service for user management and authentication that can be used by other services.

The auth server is probably on of the projects I have been working on the most, also because it can be used by many of the other project that I have been working on.

- The auth server is connected to a database with users. 
- Users has username and password. 
- When a user logs in he will get a JWT token pair, consisting of Auth token and Refresh token. 
- The tokens are each signed with their own ED25519 key, which means the auth token is signed with another key than the refresh token.
- The public key of the auth token is shared pulicly, so that other services can fetch it online.
- The other services can use that public key to verify the token that the users login with.

I have written a Go library for the JWT verification which can be imported by the services using the auth service.

A goal with the auth service is to run it as a OAuth server as well, and I am working on another library for OAuth and am currently testing it in combination with a crypto library I am also working on.

Other project that could use the auth server is:

- Martin Chat
- Door Opener
- Multiplayer websocket game
- Moro
- DSA
- Cafe App

### Martin Chat

The goal of this project is to make a chat application for anyone to use to get in contact with me.

I am tryig to learn what is important in a chat application and I am looking a lot at the Matrix protocol and how it is implemented, to avoid making unnecessary mistakes.

One of the most complex things I found so far in a chat application is the synchronization of messages amongst users and devices.

I have implemented the backend in Golang using Postgres SQL as a database. 

The front end is implemented for Android, but I am now considering implementing it in Rust with WebAssembly (yew and tailwind) because then more people can use it and hopefully push notifications are gonna be easier to implement.

I had some problems on Android because I had to use Firebase to get push notifications and I didn't want to use Firebase. So I started studying what other Android apps like Signal and Element(matrix) was doing, and I found that Matrix found an interesting way around it, but I didn't get to find out how I could do something similar for my own app. 

### Door Opener

The goal of this project was to hack the buzzer of an old door phone system with a raspberry pi and a relay and control it from the outside with a phone.

The project has been going on for years, and has gone through many transformations.

The backend was implemented in python at first and then later in golang. Now I am considering implementing it in Rust. 

The front end, or the client has been implemented as a SMS service at first, because I didn't have a smart phone at the time. Later I implemented a simple version with React for web, but the best versions I made was for android with jetpack compose and GRPC for transport. Now I am considering implementing it in Rust with WebAssembly (yew) so that people who don't have Android can get to open the door as well.

Other transports used was SMS, TCP, HTTP, and websocket, GRPC had the best performance which was important because I implemented a haptic feedback that made the buzzer stop when the user released the button on the phone.

I didn't have any security on the backend in the beginning, I was just using a VPN server on the local network, that I connected to from the phone and then I had access. Later my neighbors also wanted to have access. So I started implementing an authentication system, and telemetry for the system, which made it quite a bit more complex. The different parts were implemented as individual microservices.

### OAuth Server

The oauth server is a library that can be used by the auth server. The goal of the oauth server is to make it possible to integrate with oauth clients and use my auth server to authenticate to systems that don't know about my auth server.

The auth server is already creating jwt tokens based on a public/private key encryption, So any other system that knows about the public key could safely trust the tokens from the auth server and integrate authentication without having to use oauth. However if an owner of a system wants to use the auth server to login to a service that supports oauth flow. He can then use that. A service like that might be.

- Gitea
- Gitlab
- Matrix (for instance DSA chat for members)
- Others...

The way the oauth server library is implemented is just by reading the RFC of the oauth protocol and then implementing it with http requests in golang.

I am working on a feature where I am encrypting all http trafic between the client and server. And I need to find a way to still keep this encryption and have oauth working for any client who is not using encryption.

### OAuth Client captive portal (for OpenWRT or Linux)

The goal for oauth client captive portal is to only let specific users access the internet of a router, and make use of an oauth flow to let the user log in. 

There is a lot of things to consider security wise when building a system like this:

So far I have only made a http proxy that checks if the user is authorized for every request. Later I want to use netlink to filter all trafic for the user based on their authenticity, and I want to use asynchronous key validation for requests. Maybe in an interval. I also want to consider speed and optimize as much as possible while still keeping a good level of security.

### Matrix Notify

The goal of this simple tool is just to send messages to a matrix chat based either from a shell script or within a program written in golang. 

I am using it in ArgoCD and DroneCI to notify when a build has failed, but it could also be used in many other scenarios:

- Network security incidents .
- Network monitoring milestones notifications.
- Anything where a notification is nice to get as fast as possible.

### Monopoly Golang

The goal is to create a library that makes a modular version of monopoly where the game can grow based on the amount of fields, and each field can be custom made, with name price rent.

### IP server

This is a website with an enpoint that informs the client of the clients own public ip address. 

It also make use of a geo location database that shows the location of the ip address.

### DTU Intranet Scraper

This tool can log in to the dtu intranet, and access both the old inside.dtu.dk and the new learn.inside.dtu.dk which can be controlled using the d2l api.

I have used this tool to scrape ects + grade from all the courses and automatically calculate the GPA for a student.

Another use case was to attain student information and use it to add the students to a 3 week course - I had administative privileges to be able to do this.

### Cryptography Middleware Golang and Rust

This is a tool to encrypt messages, it will sign and encrypt any slice of bytes in a session which is a relationship with another client using the same encryption tool. 

There is also a middleware for a http server which can easily be applied in golang. And there is a middleware for a http client for both rust and golang.

The tool is very well tested and has a lot of examples.

It is used together with the auth server and the oauth server, and will be used for the martin chat as well.

The reason for making this is to learn more about how to apply the different signing and encryption algorithms, as low level as possible, without going into the implementation of the algorithms.

Algorithms used are elliptic curve P256, DH and DSA for signing and AES for encryption.

Probably the flow is very similar to TLS, it could be a good idea to read up on TLS to see if it is the same.

### Multiplayer websocket game 

This is a game where multiple players can move around on the screen in real time from anywhere in the world. 

Some challenges when building this is the synchronization of data.
The data consist of the positions of players and the actions they can do in the game. Actions can be move or write message.

Another challenge which is not implemented yet is to distribute the game servers over multiple physical servers, and making the players join the game with the players they want to play with.

Another challenge is to transport data as fast as possible, here it is good to consider the simples possible data structure to use for transport over the web. And also which protocol to use. Maybe quic is a good solution to try out.

Some features that could be nice to implement is moving the map to explore suroundings, and to be able to "teleport" to a new map when going into a house or something. Here it could be good to implement which players you should be aware of.

### Moro

Moro was a business idea some students from CBS had, and I helped them out for 1,5 years. They wanted an app that would make it possible for users to get an overview of event happening around Copenhagen, they had a well developed design, and ideas about how to categorize events so that the users could filter events after different criterias.

I started making an android App and a backend in graphql with mongodb as a database. Later the backend was changed to normal REST API, and the database was changed to SQL, because it made more sense to have a relational database since the events had relations to categories and venues.

Some swedish developers made the app in react native and I just focused on the back end and scrapers.

I spend a lot of time making scrapers for the venues, the scrapers were made in python. I found patterns for making scrapers after some time and the code became very clean. I also found that different venues were using the same framework for their websites, so I could apply the same scrapers to them.

After 1,5 years I decided to stop working with Moro because I didn't feel they included me enough in the project. And after some time they also stopped the project.

I still like the idea and am thinking about doing something similar again.

### Advent of Code

I like the code challenges in advent of code or similar. Especially for functional programming with Haskell, and to challenge myself to make as simple as possible solutions.

### DSA

DSA is the Danish Slackline Association. It is an organization for slackliners to meet and slackline together.

The organization needs a website to show information about the organization, and to recruit new members. 

The members need to be able to comunicate with eachother.

DSA needs a user system for the members of DSA, and the members of DSA should pay for the membership once every year.

As a paying member you can log in to the chat.
As a paying member you can sign up for the indoor highlines, and payments for this will be reoccuring.
As a paying member you can sign up for special events, like urban highline events and workshops.

I would like to implement the user and log in system using my authentication server. The payment logic should be implemented with another micro service which has another database for users that are linked to the users in the auth server.

Overview of services:

- Auth service
  - Anyone can register, but they need to authenticate with phone or nemID.
  - A user can login to make payments.
- Payment subscription service
  - A registered user can make payments for membership
  - A registered user with valid membership can make payments for URC
- Event/Workshop service
  - A registerd user with valid membership can sign up for events
  - An admin can add events
- Matrix chat
  - A registered user with valid membership can login to the matrix chat.



> **QUESTION ABOUT ABOVE:** When validating OAuth for the matrix chat, where should the check for active member happen? Should auth know about payments, or should the oauth server be implemented sepparately just for matrix and have a hard coded check for payment? I think something like the last will be the best solution.

### JWT decode

JWT decode is a simple tool to send a jwt token and get back the readable data.

### Keylogger linux

Keylogger for linux is a tool that reads every single keystroke on stdin

### Madplan REMA1000

Madplan for REMA1000 is a scraper that gets specific products from REMA1000 and calculates the price and calories for a bulk shopping.

### Data Management FSB

Data management is a scraper that logs into a FSB account and fetches all the appatments a user is signed up for. Then it makes it possible to easily filter and sort appartments to get an overview of how it is going on the wait list.

It could be fun to implement a dashboard that shows over time how the number in the queue is moving up and down, and for instance the top 10 of appartments overall, and maybe also after a filter is applied.

Another nice thing would be a check for new appartments that has not yet been signed up for.

### TAAssistant

TAAssistant was a chat bot for discord implemented in Golang. It was used to authenticate and verify the identity of students in courses I was TA in during covid. It helped me control the online comunication, and was working very well. Later I was told that I cannot use this because of GDPR, and the comunication became a mess because the students didn't have to show their real identity. This was also a good thing in some cases because students that might not speak up, did it. But it was much nicer in my oppinion when there was control.

### Planday Scraper

This is a scraper that logs in to plan day and automatically writes down my availability following a given weekly pattern.

### Concurrency shopping center visual

I made a shopping mall simulation in a course where customers would come into a shop with a random amount of money and time to spend, and they would then wait in line at the cash register and pay for the groceries. In the end of the day the shop would close and the cash register would all collect the money and count it.

I would like to make a graphical version of this where random customers are being generated at the door of the shop and then will need some random things in the shop. They will then use some path logic to find the things they need in the shop and go to the cash register, wait in line, pay for groceries and leave the shop.
The shop should have a specific amount of groceries when opening, and if a customer cannot find the grocery he wants he should continue without it, but it should be logged.
There should be multiple empoyees and they should refill groceries, and be in the cash register, they will get groceries from a door somewhere and maybe a truck could come and refill groceries. Groceries could also get old. There should be multiple cash registers and when there is more than 10 customers in queue another till can open, depending on avaiable empoyees.

For the graphical part ebiten can be used. or if it should be rust then it should be bevy. for Concurrency CSP pattern should be used, and each entity in the program should be in their own "thread", comunicating resources through channels.

### Nebula Hack

This tool will try to log in to nebula with different usernames and passwords given from lists. If a login attempt is successful username and password will be saved in a file.

### DBA Scraper

This tools will check for new items on dba given a specific search query. It can also send automatic messages to the owner of the item or validate their identity using krak for the phone number.

### FWD + FWDCTL

FWD is a tcp proxy which is running as a service on linux. You can apply an interceptor on the data and modify or log the traffic. FWDCTL is comunicating with the running service over GRPC and can add or remove proxies and interceptors

### Cafe App

Cafe app is an app for booking tables in a cafe. It is one of the first projects I have thought about. The cafe or restaurant, has some tables which is identified with a number.

There should be an interface for customers wanting to book a table. Here they will write how many they are and then they will automatically get assigned a table for the amount of people.

There should be an interface for admins who can add or remove tables and reservations.

There should be an interface for empoyees who need to see which tables are reserved for which name.

There might be some custom metadata for the tables, like which area they belong to, if it it indoor of outdoor, handicap friendly, children area or smoking section.
