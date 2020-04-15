# REST APIs
The glue between the frontend and backend


### What is an API?
An Application Program Interface (API) is a set of defined functions that access data, perform operations, and decouple applications, e.g., a library. 

### What is REST?
Representational State Transfer (REST) is a software architecture for developing web services.

[What exactly is RESTful programming?](https://stackoverflow.com/questions/671118/what-exactly-is-restful-programming)

### What is a REST API?
Bringing these two concepts together, a REST API is a web service that has defined functions to retrieve data and perform operations. They decouple applications and works as a middle man between the frontend and the backend. 

![diagram](https://d1xple9gxb4tux.cloudfront.net/assets/images/article_images/c434d8fccf80bd57ef848ae24a9825ffd3322be7.png?1553504289)

[A Beginner’s Tutorial for Understanding RESTful API](https://mlsdev.com/blog/81-a-beginner-s-tutorial-for-understanding-restful-api)

### How are they made?
They can be developed in most of the popular programming languages and are usually paired with a web framework. Some examples of this are Java and Spring, Javascript and Node/Express, and Python and Flask.    

### Where do they live?
REST APIs live on the backend. This means they generally run on a remote server. 

### How do you access them?
Generally, REST APIs use the HTTP protocol to communicate with the outside world. Each action will be assigned a URL path and accessed by a GET/POST/PUT/DELETE method. A great tool for working with APIs is [Postman](https://www.getpostman.com/downloads/).

### What is Postman?
Postman is an API development app that presents an easy to read GUI for users to construct requests and read responses. 

## What is HTTPS and SSL
Https represents a secure alternative to http. It means all communication over http will be encrypted, protecting sensitive data. It works by using to keys to encrypt data, a public key and a private key. Anything encrypted by the public key can only be decrypted by the private key. When using https, the webserver should hold the private key, and the client will use the public key to encrypt data sent to the server.
When the client connects to a server, the server will send an SSL certificate, which contains the public key.

SSL (Secure Sockets Layer) and its successor, TLS (Transport Layer Security), are protocols for establishing authenticated and encrypted links between networked computers.

All communication in http is plain text, and is routed through multiple computers on the way to its destination. Any of those computers can pretend to be the website you actually want to visit. Hence SSL provides a way to verify that you are communicating
with the server you expect to, and no one can eavesdrop on the conversation.

SSL depends on trusted SSL providers. These are authorities that issue SSL certificates, which a server will distribute to clients that connect to it. Your web browser has a list of trusted SSL providers, and will issue a warning when it receives a certificate
from an organization not on that list.