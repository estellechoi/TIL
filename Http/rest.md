# REST, REST API and RESTful Web Services

> This markdown page is Korean/English mixed entirely depending on my personal need, not readers' possible expectation.

<br>

## What is REST ?

REST stands for <b>RE</b>presentational <b>S</b>tate <b>T</b>ransfer.

It is an architectural style for distributed hypermedia systems like [World Wide Web](https://ko.wikipedia.org/wiki/%EC%9B%94%EB%93%9C_%EC%99%80%EC%9D%B4%EB%93%9C_%EC%9B%B9). The idea is that simple HTTP is used to make calls between machines.

RESTful applications use HTTP requests to post data (create or update), read data (e.g., make queries), and delete data. Thus, REST uses HTTP for all 4 CRUD (Create/Read/Update/Delete) operations.

![REST HTTP Method](./../img/restHttpMethod.jpeg)

> HTTP is HyperText Transfer Protocol. It is the underlying protocol used by the World Wide Web and this protocol defines how messages are formatted and transmitted, and what actions Web servers and browsers should take in response to various commands.

REST has it’s own [6 guiding constraints](https://restfulapi.net/rest-architectural-constraints/) which must be satisfied if an interface needs to be referred as <strong>RESTful</strong>.

It was first presented by Roy Fielding in 2000 in his famous [dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm).

<br>

## Principles of REST

### 1. Client–server

By separating the user interface concerns from the data storage concerns, we improve the portability of the user interface across multiple platforms and improve scalability by simplifying the server components.

<br>

## Architectural Constraints

### 1. Uniform interface

API의 인터페이스를 결정하고, 정해진 인터페이스를 정확하게 따라야 한다. 서버 내의 각 리소스에 접근할 수 있는 URI는 오직 1 개여야 하며, 연관 데이터에 추가적으로 접근할 수 있는 방법을 제공해야 한다.

하나의 리소스가 너무 많은 데이터를 갖고있어서는 안된다. 관련 데이터가 필요하면 언제든 그 데이터에 접근할 수 있는 URI 링크를 포함해야 한다.

Also, the resource representations across the system should follow specific guidelines such as naming conventions, link formats, or data format (XML or/and JSON).

All resources should be accessible through a common approach such as HTTP GET and similarly modified using a consistent approach.

<br>

### 2. Client–server

Client application and server application MUST be able to evolve separately without any dependency on each other. A client should know only resource URIs, and that’s all. Servers and clients may also be replaced and developed independently, as long as the interface between them is not altered.

<br>

### 3. Stateless

Make all client-server interactions stateless. The server will not store anything about the latest HTTP request the client made. It will treat every request as new. No session, no history.

If the client application needs to be a stateful application for the end-user, where user logs in once and do other authorized operations after that, then each request from the client should contain all the information necessary to service the request – including authentication and authorization details. The client is responsible for managing the state of the application.

<br>

### 4. Cacheable

Resources MUST declare themselves cacheable. Caching can be implemented on the server or client-side.

Caching brings performance improvement for the client-side and better scope for scalability for a server because the load has reduced.

<br>

### 5. Layered system

REST allows you to use a layered system architecture where you deploy the APIs on server A, and store data on server B and authenticate requests in Server C, for example. A client cannot ordinarily tell whether it is connected directly to the end server, or to an intermediary along the way.

<br>

### 6. Code on demand (optional)

When you need to, you are free to return executable code to support a part of your application, e.g., clients may call your API to get a UI widget rendering code. It is permitted.

<br>

## Benefits of using REST Architecture

### Lightweight Web Services

The RESTful architecture was a reaction to the more heavy-weight SOAP-based standards. In REST web services, the emphasis is on simple point-to-point communication over HTTP using plain XML. In addition, RESTful permits many different data formats whereas SOAP only permits XML.

### Simplicity

One of the main reasons for REST popularity is the simplicity and ease of use, as it does an extension of native Web technologies such as HTTP.

### RESTful architecture is closer in design to the Web

RESTful is the architectural style of the web itself, so the developer with knowledge in web architecture will naturally develop in the RESTful architecture.

### Scalability

As RESTful forbids conversational state, which means we can scale very wide by adding additional server nodes behind a load balancer.

### Expose APIs as HTTP Services

When developers need the universal presence with minimum efforts, given the fact that RESTful APIs are exposed as HTTP Services, which is virtually present on almost all the platforms.

<br>

## REST API

### What is API ?

An API(Application Programming Interface) serves as an interface between different software programs and facilitates their interaction, similar to the way the user interface facilitates interaction between humans and computers.

> For API basics, watch [this video](https://www.youtube.com/watch?v=s7wmiS2mSXY) that explains API.

<br>

### REST API

![REST API](./../img/restapi.png)

A REST API defines a set of functions which developers can perform requests and receive responses via HTTP protocol such as GET and POST.

<br>

## RESTful Web Services

`RESTful` is typically used to refer to web services implementing REST architecture.

![RESTful Web Service](./../img/restful.png)

<br>

---

### References

- [restfulapi.net](https://restfulapi.net/)
- [What is REST?](https://www.codecademy.com/articles/what-is-rest)
- [Mastering REST Architecture — Introduction](https://medium.com/@ahmetozlu93/mastering-rest-architecture-introduction-28f7f2d67a7)
- [Mastering REST Architecture — REST Architecture Details](https://medium.com/@ahmetozlu93/mastering-rest-architecture-rest-architecture-details-e47ec659f6bc)
- [Mastering REST Architecture — REST API Details](https://medium.com/@ahmetozlu93/mastering-rest-architecture-rest-api-details-f560e4d93f2)
- [Mastering REST Architecture — Developing a REST API with Spring Boot Framework, Step#1: Creating the Project](https://medium.com/@ahmetozlu93/mastering-rest-architecture-developing-a-rest-api-with-spring-boot-framework-step-1-creating-d7796066862b)
- [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
- [Building REST services with Spring](https://spring.io/guides/tutorials/bookmarks/)
- [REST | wikipedia](https://ko.wikipedia.org/wiki/REST)
- [REST 아키텍처를 훌륭하게 적용하기 위한 몇 가지 디자인 팁](https://spoqa.github.io/2012/02/27/rest-introduction.html)
