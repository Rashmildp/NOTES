
# ASP.NET CORE FUNDAMENTALS ❤️🔥

## What is ASP.NET Core, and how is it different from ASP.NET?

ASP.NET Core is a new, open-source, and cross-platform framework for building modern web applications. It is a successor to ASP.NET, but it has been re-designed from the ground up to be faster, more flexible, and more modular.

## What are the benefits of using ASP.NET Core?

Some of the benefits of using ASP.NET Core include:
1.  Cross-platform compatibility
2.  Improved performance
3.  Support for modern development patterns and practices
4. A more modular architecture
5.  Integration with modern front-end frameworks and tools
6. Built-in support for cloud deployment
## What is the difference between MVC and Razor Pages in ASP.NET Core?

MVC (Model-View-Controller) is a design pattern that separates an application into three components: the Model, which represents the data and business logic, the View, which renders the HTML, and the Controller, which handles user interactions. Razor Pages is a new feature in ASP.NET Core that provides a simpler, more straightforward approach to building web applications. With Razor Pages, you can build an application by creating a hierarchy of Razor Pages, each of which represents a single URL.
## What is Middleware in ASP.NET Core?

Middleware is a component that sits between the server and the application in the request/response pipeline. It can modify incoming requests, perform custom logic, and return responses to the client. Middleware is a key feature in ASP.NET Core and allows developers to add custom functionality to their applications without having to modify the underlying code.
```bash
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.Use(async (context, next) =>
    {
        await context.Response.WriteAsync("Hello World From 1st Middleware!");

        await next();
    });

    app.Run(async (context) =>
    {
        await context.Response.WriteAsync("Hello World From 2nd Middleware"); 
    });
}
```
## What is the role of Startup.cs in an ASP.NET Core application?

Startup.cs is a special class in ASP.NET Core that defines the configuration of the application. 
It is where you configure the services that the application will use, set up the middleware pipeline, and specify the default page to be displayed when the application is started. The Startup class is automatically invoked when the application starts, and it is an important part of the overall configuration and bootstrapping process.

## What is Entity Framework Core, and how does it relate to ASP.NET Core?

Entity Framework Core is a modern data access framework that is part of the ASP.NET Core ecosystem. It provides an easy-to-use, code-first approach to working with relational databases and supports LINQ (Language Integrated Query), making it easier to work with data in a strongly-typed, type-safe way. Entity Framework Core can be used in any .NET application, but it is particularly well-suited to ASP.NET Core, where it can be used to easily retrieve, update, and store data in a database.

## What are some best practices for building scalable ASP.NET Core applications?

Some best practices for building scalable ASP.NET Core applications include:

1. Keeping the codebase organized and modular
2. Choosing the right data storage strategy
3. Implementing efficient caching mechanisms
4. Taking advantage of asynchronous programming
5. Implementing logging and monitoring
6. Adhering to security best practices.

## Model , Service and Controller
1. **Model**: Represents the data and business logic of the application. A model can be a simple data structure, such as a  class, or it can be a more complex object that implements business logic. 

For example, a Product model might include properties like Name, Price, and Description, and might include methods like CalculateDiscountedPrice
```bash
 public class Todo
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Description { get; set; }
        public DateTime Created { get; set; }
        public DateTime Due { get; set; }
        public TodoStatus Status { get; set; } // New , Inprogress, Completed
    }
```
2. **Service**: A service is a component that provides a specific function or service to the application. Services are usually created to encapsulate logic that is shared across multiple parts of the application, and can be injected into other components via Dependency Injection (DI). 

```bash
 public class BlogService
    {
        public List<Post> GetAllPosts()
        {
            var blogData = new HardCodedData();

            return blogData.Posts();
        }

        public Post GetPost(int id)
        {
            var blogData = new HardCodedData();
            return blogData.Posts().SingleOrDefault(p => p.Id == id);
        }
    }
```
Can It do also using dependancy injection priciples.
First need to create Interface to store all the functions.
```bash
    public Interface IBlogInterface{
          public List<Post> GetAllPosts();
          public Post GetPost(int id);
    }
```
Now the service class looks like this
```bash
 public class BlogService : IBlogInterface
    {
        public List<Post> GetAllPosts()
        {
            var blogData = new HardCodedData();

            return blogData.Posts();
        }

        public Post GetPost(int id)
        {
            var blogData = new HardCodedData();
            return blogData.Posts().SingleOrDefault(p => p.Id == id);
        }
    }
```
3. **Controller**: Handles incoming HTTP requests, processes data, and returns a response. Controllers are the glue that connects models and views, and are responsible for updating the model, and returning the appropriate view. Controllers can also use services to perform specific functions. For example, a ProductController might include methods for displaying a list of products, displaying a detail view for a specific product, and processing form submissions to update or create new products.
```bash
{
    [Route("api/[controller]")]
    [ApiController]
    public class TodosController : ControllerBase
    {
        private TodoService _todoService;

        public TodosController()
        {
            _todoService = new TodoService();
        }

        [HttpGet("{id?}")]
        public IActionResult GetTodos(int? id)
        {
            var myTodos = _todoService.AllTodos();

            if (id is null) return Ok(myTodos);

            myTodos = myTodos.Where(t => t.Id == id).ToList();

            return Ok(myTodos);
        }

        
    }
```
But it can write this way also
```bash
{
    [Route("api/[controller]")]
    [ApiController]
    public class TodosController : ControllerBase
    {
        private readonly ITodoRepository _todoService;

        public TodosController(ITodoRepository repository)
        {
            _todoService = repository;
        }

        [HttpGet("{id?}")]
        public IActionResult GetTodos(int? id)
        {
            var myTodos = _todoService.AllTodos();

            if (id is null) return Ok(myTodos);

            myTodos = myTodos.Where(t => t.Id == id).ToList();

            return Ok(myTodos);
        }

        
    }
}

```
Startup class for dependancy inject
```bash

        public IConfiguration Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // services.AddScoped<IBlogService, BlogHardCodedService>();
            services.AddHttpClient<IBlogService, BlogHttpService>(client =>
            {
                client.BaseAddress = new Uri("https://jsonplaceholder.typicode.com");
            });
            services.AddControllersWithViews();
        }
```
## **THREADS**
```bash
using System;
using System.Threading;

namespace ThreadsExample
{
    public class Program
    {
        static void Main(string[] args)
        {
            // Thread is an execution part of the program
            // We can use multiple threads to perform.
            // Different tasks of are project at the same time.
            // Current thread is running main thread.
            Thread mainThread = Thread.CurrentThread;
           // mainThread.Name = "Current Thread";
            //Console.WriteLine(mainThread.Name);
            Thread thread1 = new Thread(() => CountDown("First Thread"));
            Thread thread2 = new Thread(()=>CountUp("Second Thread"));
            thread1.Start();
            thread2.Start();
            Console.WriteLine(mainThread.Name+" is completed");


        }
        public static void CountDown(String name)
        {
            for(int i = 10; i>  0; i--)
            {
                Console.WriteLine("Timer #1 "+i +" Seconds");
            }
            Thread.Sleep(1000);

        }
        public static void CountUp(String name)
        {
            for (int i = 0; i < 10; i++)
            {
                Console.WriteLine("Timer #2 " + i + " Seconds");
            }
            Thread.Sleep(1000);

        }
    }
   
}
```
**Async and Await are C# keywords used for asynchronous programming in ASP.NET Core.**

Asynchronous refers to a type of processing or communication that does not block the execution of other tasks or operations. In other words, **it allows multiple tasks to be executed in parallel, without waiting for one task to finish before starting the next one.**

For example, in an asynchronous system, if task A takes a long time to complete, task B can start and run simultaneously, rather than waiting for task A to finish before starting. This leads to improved performance and responsiveness, as well as increased scalability, as the system can handle multiple tasks at the same time.

Asynchronous programming is widely used in modern web development, particularly in high-traffic scenarios, to ensure the best performance and user experience.

Async: The async keyword is used to declare a method or a lambda expression as asynchronous, which means that the method can be executed _**asynchronously and does not block the execution of the rest of the application.**_

Await: The await keyword is used to wait for the completion of an asynchronous operation. When an asynchronous method is executed and reaches an await keyword, it will pause its execution until the awaited task is completed and then resume where it left off.

**By using async and await, developers can write asynchronous code in a more straightforward and readable way, making it easier to build scalable and high-performance ASP.NET Core applications.**
Without using async and await 
     ```bash
        static void Main(string[] args)
            Method();
            Console.WriteLine("Main Thread");
            Console.ReadLine();


        }
      public static async void Method()
        {
          await  Task.Run(new Action(LongTask)); // wait until long task finsihes
            Console.WriteLine("New Thread");
        }
        public static void LongTask()
        {
            Thread.Sleep(20000);
            Console.WriteLine("Long Task Finshed !");
        }
        public static void CountDown(String name)
        {
            for(int i = 10; i>  0; i--)
            {
                Console.WriteLine("Timer #1 "+i +" Seconds");
            }
            Thread.Sleep(1000);

        }
    ```
   
