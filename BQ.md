# Job Recommendation
**Content-based**: Given item profiles (category, price, etc.) of your favorite, recommend items that are similar to what you liked before. 

##### Java Servlet and Tomcat:
I used Java to build a few servlets. The first one is a SearchItems API that provides the functionality to search around, The second servlet that allows a user to set or unset their favorite records. The last one that recommends similar items to the user. And I deploy these servlets on tomcat.
Java servlet: java class to interact with remote server to handle reponse and request preocudure calls
Tocat: environment to run web service and implement servlet

##### What’s the RESTful API and what’s the benefit of REST?
(REST) is an architectural approach to designing web services. 

**Benefits of REST**
Operations are directly based on HTTP methods, so that server doesn’t need to parse extra thing
URL clearly indicates which resources a client wants, easy for clients to understand.
The server is running in stateless mode, improving scalability. (stateless means server does not need to retain session information. Those information are sent to the server by the clients with every packet of information. Reduce server load caused by retention of session information)

 Implemented **Java servlets** to handle post and get reqeust given parameters (lat, lon, keyword), and make requests to search from a 3rd party service GitHub via **GitHubAPI**, result return as json array, use **Jackson** to parse jason and store in **model entity class** Item. Item implemented using **Bilder pattern** to aoivd errors when dealing with many parameters. Github only returns a description of a job position but we need to generate a tag or a keyword. We then use keywords to improve recommendation results. We **extract keywords/categories** from the job description for recommendation by using **MonkeyLearn API**, which implemnts **TF-IDF**, which is one of the very common algorithms to extract keywords from text.

**Why choose Monkey Learn API**
One of the few keyword extraction APIs that are free
Very detailed documentation and provide many client libraries
However, it has data throttling so don’t try to do a load test on it or play it too frequently. If your account got blocked, apply for a new one. 

**TF-IDF**
TF-IDF algorithm tells you the ordered keywords in a text document (keyword extraction). TF (Term frequency) is the frequency of a word in a document. However, ‘a’, ‘the’, ‘is’ are common in every document and their TF values are high in almost every document which will overwhelm some real keywords.  IDF (Inverse Document Frequency) is the importance of a word in general. IDF values are very low for these meaningless words. Final score is 
Final score = TF * IDF

Once a user favorite an item, we wiil store the item into MySQL database. MySQL is an open-source relational database management system (RDBMS).

# Spring Shopping Cart
**Presentation tier:**  It provides the application’s user interface. For example, how to register, login, display products, checkout in the browser and how to provide an easy way for users to send requests to the backend.
Language: **HTML, CSS, Javascript**
**Logic tier:** Maintain the business logic of the application, sitting between presentation tier and data tier, receive requests from presentation tier, make correct database operation, and return the final result back to the presentation tier.
We are using **Spring MVC, Spring security and Spring webflow in our project**
**Data tier:**
tell the database system how to store our data. For example, what does each table look like, what’s the relationship between each other.
Language: SQL
We are using **Hibernate** framework to operate the database 

### Spring
The Spring Framework is divided into modules. At the heart are the modules of the core container, including a configuration model and a dependency injection mechanism. 
##### Dependency Injection

Instead of maintaining dependencies by the PaymentAction object, it can be injected by someone else(spring framework). This is called Dependency Injection
**IoC Container**
The IoC container will create the objects, wire them together, configure them, and manage their complete life cycle from creation till destruction. These objects are called **Spring Beans**.
The container gets its instructions on what objects to instantiate, configure, and assemble by reading the configuration metadata provided. The configuration metadata can be represented either by XML, **Java annotations**, or Java code
**Annotation-based configuration**
When the spring container initializes, it searches the root and sub folders that are defined at base-package, instantiating the bean which has the @Component annotation.

### Hibernate
**ORM**: conversion tool to convert object to database table
class <--> table
object <--> row
fields <--> column

**Important interfaces of Hibernate framework**
SessionFactory: SessionFactory is an immutable thread-safe cache of compiled mappings for a single database. We initialize the sessionFactory once and then reuse it. SessionFactory instance is used to get the Session objects for database operations.
Session: Session object is the interface between java application code and hibernate framework and provides methods for CRUD operations.
Transaction: Transaction is an object used by the application to specify atomic units of work. 

In other words, SessionFactory maintains Session. We request a Session from SessionFactory. Session performs transactions to operate data in DBMS.

**Entity replationship**
One-to-one: customer and cart
Many-to-one: cartitem to cart
one-to-many: customer to salesorder

### Spring Web MVC
Spring MVC components
DispatcherServlet(前端控制器)
HandlerMapping(映射处理器): Map a request to a handler that helps the DispatcherServlet to invoke, for example, a controller can be a handler.
Controller(处理器): A component to express request mappings, request input.
ModelAndView(模型和视图): Represents a model and view returned by a handler, to be resolved by the DispatcherServlet.
ViewResolver(视图解析器): a mapping between view names and actual views.
首先用户发送请求-->前端控制器(DispatcherServlet), 前端控制器根据请求信息(如URL)来决定选择哪一个控制器进行处理并把请求委托给它. 
控制器(controller)接收到请求后, 首先它会在自己内部寻找一个合适的方法来处理请求(用@RequestMapping将方法映射到请求上), 然后传入收到的请求并且调用业务对象进行处理; 处理完毕后返回一个ModelAndView(模型数据和逻辑视图名). 
前端控制器根据返回的逻辑视图名, 选择相应的视图进行渲染, 并把模型数据传入以便视图渲染. 
前端控制器再次收回控制权, 将响应返回给用户.
**Annotation used for defining RESTful API**
@Controller
Use @Controller to mark a class its role as a web component, so the spring mvc will register the methods which annotated the @RequestMapping.
@RequestMapping
Use the @RequestMapping annotation to map requests to controllers methods. It has various attributes to match by URL, HTTP method, request parameters, etc.
@RequestParam
Use the @RequestParam annotation to bind Servlet request parameters (that is, query parameters) to a method argument in a controller.
Return types
ModelAndView: 用来存储处理完后的结果数据, 以及显示该数据的视图. controller调用模型层处理完用户请求后, 把结果数据存储在该类的model属性中, 把要返回的视图信息存储在该类的view属性中, 然后把ModelAndView返回给Spring MVC框架. 框架通过调用配置文件中定义的视图解析器,对该对象进行解析, 最后把结果数据显示在指定的页面上.
void: A method with a void return type is considered to have fully handled the response.
String: A view name to be resolved with ViewResolver.
@ResponseBody: The return data will be sent as json format.


**Benefits of Spring MVC**
清晰的角色划分: 前端控制器(DispatcherServlet), 视图解析器(ViewResolver), 处理器(Controller). 通过 DispatchServlet 将控制器层和视图层完全解耦.
SpringMVC既可以返回合适的页面, 也可以响应RESTful请求.

**Spring Web Flow**
What is a flow?
A flow encapsulates a sequence of steps that guides users through the execution of some business logic, such as checkout.
What is a flow made up of?
In Spring Web Flow, a flow consists of a series of steps called "states". Entering a state typically results in a view being displayed to users so that they can interact with the system or some backend logic is performed. The result of one state can trigger transitions to other states.
How to compose a flow?
Flows are authored by using a simple XML-based flow definition language

The components of a flow
In Spring Web Flow, flow is defined by three primary elements: states, transitions, and flow data.

States-- states are points in a flow where something happens.
Transitions-- transitions connect the states within a flow.
Flow data-- the current condition of the flow.

一个完整的flow请求响应过程:
首先flow处理请求由DispatcherServlet转发给FlowHandlerMapping开始处理;
FlowHandlerMapping读取flow registry, 知道flow是如何定义、在哪里定义的, 这相当于一种映射过程；
FlowHandlerMapping将具体的处理工作交给FlowHandlerAdapter, FlowHandlerAdapter调用请求flowexecutor执行具体的flow请求.










