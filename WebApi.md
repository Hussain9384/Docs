# Docs
  * Ref https://dotnettutorials.net/course/asp-net-core-web-api-tutorials/

* Http 
  * URL: Each Request must have a unique URL
  * Verb (Method): Each Request must have an HTTP Verb.
  * Header(s): Each Request can contain one or more Headers.
  * Body: Each request can have a body. The body contains the data that we want to send to the server.
* HTTP Response Components
  * HTTP Status Code: It must have a Status Code.
  * Response Headers: It can have one or more response headers.
  * Data: Response can have data i.e. return to the client.    
* HTTP Methods
  * GET : Fetch the Data
  * POST : Create New Resource
  * PUT : Update Resource
  * PATCH : Update Part of the Resource
  * DELETE :  Delete the Resource (Soft / Hard)

* HTTP Status Codee
  * 1XX: Informational Response (Ex: 100, 101, 102, etc.)
  * 2XX: Successful, whenever you get 2XX as the response code, it means the request is successful. (Ex. 200, 201, 203, etc.)
  * 3XX: Redirection, whenever you get 3XX as the response code, it means it is re-directional i.e. some re-directional is happening on the server. (Ex. 300, 301, etc.)
  * 4XX: Client Error, whenever you get 4XX as the response code, it means there is some problem with your request. (Ex: 400, 404, etc.)
  * 5XX: Server Error. Whenever you get 5XX as the response code, it means there is some problem in the server. (Ex: 500, etc.).

  * 100: 100 means Continue. 
  * 200: 200 means OK. 
  * 201: 201 means a new resource created. 
  * 204: 204 means No Content. 
  * 301: 301 means Moved Permanently. 
  * 302: 302 means Found. 
  * 400: 400 means Bad Request. 
  * 401: 401 means Unauthorized.
  * 404: 404 means Not Found. 
  * 405: 405 means Method Not Allowed. 
  * 500: 500 means Internal Server Error. 
  * 503: 503 means Service Unavailable. 
  * 504: 504 means Gateway Timeout.

* Environment Variables : (ASPNETCORE_ENVIRONMENT : "Development")
```
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
```
![image](https://user-images.githubusercontent.com/71544024/154121702-1238389e-41ab-49f7-860f-75ae40bd9ccf.png)

```
namespace MyFirstWebAPIProject
{
    public class Startup
    {
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {

            services.AddControllers();
            services.AddSwaggerGen(c =>
            {
                c.SwaggerDoc("v1", new OpenApiInfo { Title = "MyFirstWebAPIProject", Version = "v1" });
            });
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
                app.UseSwagger();
                app.UseSwaggerUI(c => c.SwaggerEndpoint("/swagger/v1/swagger.json", "MyFirstWebAPIProject v1"));
            }

            app.UseHttpsRedirection();

            app.UseRouting();

            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllers();
            });
        }
    }
}
```
* CreateDefaultBuilder
  * By using the CreateDefaultBuilder method, we will get inbuilt support for Dependency Injection. This is one of the most popular design patterns used in almost all modern web applications and you get this by default when you add the CreateDefaultBuilder method. Also, enables scope validation on the dependency injection container when EnvironmentName is ‘Development’.
  * It load app Configurations from ‘appsettings.json’ and ‘appsettings.[EnvironmentName].json’. Configurations are basically the settings that our application uses.
The CreateDefaultBuilder method also set the ContentRootPath to the result of GetCurrentDirectory() method. The GetCurrentDirectory method belongs to the System.IO.Directory namespace.
  * The CreateDefaultBuilder method also configures the logging factory to log to the console, debug, and event source output. Nowadays, logging is mandatory for any kind of application. So, by using the CreateDefaultBuilder method, we also get logging support by default.
  * It also loads application Configuration from User Secrets when EnvironmentName is ‘Development’ using the entry assembly as well as load Configuration from environment variables.
  * It also loads host Configuration from supplied command line args. This is only applicable with the version which takes string array as an argument.

* ConfigureWebHostDefaults
  * Along with the CreateDefaultBuilder method, we also need to add the ConfigureWebHostDefaults method. This method Configures a HostBuilder with defaults for hosting a web app. Following is the signature of this method.
  * ![image](https://user-images.githubusercontent.com/71544024/154123668-e6a72286-fa55-4be3-84f7-ec0eb3a45b07.png)
  * Parameter:
  * builder: The IHostBuilder instance to configure.
  * configure: The configure callback
  * Returns: A reference to the builder after the operation has completed.
  * Sets Kestrel Server as the Web Server and configure it using the application’s configuration providers. When we run the application using CLI, the kestrel web server is used.
  * Enables IIS Integration, etc.
  * It adds the Host Filtering middleware
  * Also adds the ForwardedHeaders middleware if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
  * It configures WebRootFileProvider to include static web assets from projects referenced by the entry assembly during the development
  * The ConfigureWebHostDefaults method provides the following support. 
  * Build Method: The Build Method runs the given actions to initialize the host. This can only be called once. It returns an initialized Microsoft.Extensions.Hosting.IHost object.
  * Run Method: It Runs the application and blocks the calling thread until the host shutdown.
  * By calling the UseRouting() method on the IApplicationBuilder object (i.e. app) we are configuring the Routing middleware in our application 
```
app.UseEndpoints(endpoints =>
    {
        endpoints.MapGet("/", async context => {
            await context.Response.WriteAsync("Hello From ASP.NET Core Web API");
        });
    });
```
  * Differences between AddMvc, AddControllers, AddRazorPages, and AddControllersWithViews:
  * AddControllers: The AddControllers method is used only for Web APIs. That means if you want to create a Web API Application where there are no views, then you need to use the AddController extension method.
  * AddControllersWithViews: The AddControllersWithViews method is used to support MVC Based Applications. That means If you want to develop a Model View Controller (i.e. MVC)     * application then you need to use AddControllersWithViews() extension method.
  * AddRazorPages: The AddRazorPages method is used for the Razor Pages application. That means if you want to work with the Razor Page application, then you need to use the       * AddRazorPages() extension method
  * AddMvc: The AddMvc method adds all the services that are required for developing any type of application. That means we can say if we are using the AddMvc method, then we can develop Web API, MVC, and Razor Pages applications.
  * Note: Adding AddMvc() method will add extra features to your application even though they are not required which might impact the performance of the application.
* Points to Remember while working with Controller class in ASP.NET Core Web API:
  * The Controller class in ASP.NET Core Web API must have a “Controller” suffix 
  * The Controller class must be inherited from the ControllerBase class. If you are coming from an ASP.NET Core MVC background, then, in that case, the Controller class is inherited from the Controller class
  * We need to decorate the controller class with the ApiController attribute.
  * We also need to use Attribute Routing to access the resource using URI.
* Controller and ControllerBase
  * The Controller class that we used in ASP.NET Core MVC has support Views, ViewBag, ViewData, TempData, etc. But here in ASP.NET Core Web API, we don’t require such concepts. So, we skip controller class and use the ControllerBase class.

  * The point that you need to remember is the ControllerBase class is also serves as the base class for the Controller class. That means behind the scene the Controller class is inherited from the ControllerBase class.
* ApiController Attribute in ASP.NET Core Web API:
  * It Indicates that a type and all derived types are used to serve HTTP API responses. Controllers decorated with this attribute are configured with features and behavior targeted at improving the developer experience for building APIs. When decorated on an assembly, all controllers in the assembly will be treated as controllers with API behavior. Some of the features are as follows:
  * AmbiguousMatchException: The request matched multiple endpoints. Matches:
Attribute Routing Requirement
Handle the client error i.e. 400 HTTP status code.
Multipart/form-data request inference
Binding the incoming data with the parameters using the Model Binding concepts  
So, if you are using the Controller class in your ASP.NET Core MVC application, it means you are also using ControllerBase class. But in Web API, we simply need to use the ControllerBase class.  
* Middleware in ASP.NET Core Web API:
  * Middleware is a piece of code that is used in the HTTP Request Pipeline. An ASP.NET Core Web API Application can have n numbers of middleware. So, depending upon the requirement, we can configure n numbers of middleware in the application request processing pipeline.
  * The order of middleware matters a lot in the execution. That means in the order they are configured into the request processing pipeline; in the same order, they are going to be executed when a request comes. Each middleware in the ASP.NET Core Web API Application performs the following tasks.
  * Chooses whether to pass the HTTP Request to the next component in the pipeline. This can be achieved by calling the next() method within the middleware.
Can perform work before and after the next component in the pipeline.
  * ASP.NET Core provides some built-in middleware that is ready to be used, even if you want then you can also create your own custom middleware. The most important point that you need to keep in mind is, in ASP.NET Core a given Middleware component should only have a specific purpose i.e. single responsibility.
* Middleware Examples:
  * Routing: If you want to implement Routing in your application, then you need to use Routing Middleware in the HTTP Request Processing pipeline.
  * Authentication: If you want to authenticate the user then you need to use Authentication Middleware.
  * Authorize: The Authorize Middleware is used to Authorize the users while accessing a specific resource.
  * Log: If you want to log request and response while processing, then you need Middleware.
  * Exception Middleware: You can also use Middleware to handle the exception globally.
  * Note: The Middleware in ASP.NET Core Web API Application is used to set up the HTTP Request processing pipeline. If you have prior experience of the previous .NET Framework then you may know, HTTP Handlers and HTTP Modules which are basically used to set up the request processing pipeline. It is this pipeline that will determine how the HTTP request and response are going to be processed.

* Run, Use, Next, and Map Methods in Middleware:
  * In order to work with ASP.NET Core Middleware Components, we need to learn about few methods are as follows:
  * Run() Method: The Run() Extension Method is used to complete the Middleware Execution.
  * Use() Method: The Use() Extension Method is used to insert a new Middleware component to the Request Processing Pipeline.
  * Next() Method: The Next() Extension Method is used to call the next middleware component in the request processing pipeline.
  * Map() Method: The Map() Extension Method is used to map the Middleware to a specific URL.
  * In our next article, I am going to the Run, Next, and Use Extension Methods in ASP.NET Core Web API Application with Examples. Here, in this article, I try to explain ASP.NET Core Web API Middleware Components and I hope you enjoy this Middleware in ASP.NET Core Web API article.
```
app.Use(async (context, next) =>
    {
        await context.Response.WriteAsync("Use Middleware1 Incoming Request \n");
        await next();
        await context.Response.WriteAsync("Use Middleware1 Outgoing Response \n");
    });
    app.Use(async (context, next) =>
    {
        await context.Response.WriteAsync("Use Middleware2 Incoming Request \n");
        await next();
        await context.Response.WriteAsync("Use Middleware2 Outgoing Response \n");
    });
  ========================  
    
app.Map(“/testmap”, MapCustomMiddleware);
private void MapCustomMiddleware(IApplicationBuilder app)
        {
            app.Use(async (context, next) =>
            {
                await context.Response.WriteAsync("Specific URL Logic Middleware using Map Method \n");
            });
        }
        
 =========================
 
 using Microsoft.AspNetCore.Http;
using System.Threading.Tasks;
namespace MiddlewareInASPNETCore
{
    public class MyCustomMiddleware1 : IMiddleware
    {
        public async Task InvokeAsync(HttpContext context, RequestDelegate next)
        {
            await context.Response.WriteAsync("Custom Middleware Incoming Request \n");
            await next(context);
            await context.Response.WriteAsync("Custom Middleware Outgoing Response \n");
        }
    }
}

Inject the service to the built-in dependency injection container
services.AddTransient<MyCustomMiddleware1>();
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
    services.AddTransient<MyCustomMiddleware1>();
}
Step2: Registering the Custom Middleware in the HTTP Request Processing Pipeline

app.UseMiddleware<MyCustomMiddleware1>();

```

* How does the Routing work in ASP.NET Core Web API?
  * When we run the application, then it will create the Route table and the Route table will contain the mapping information between the URL and the Resource
  * Enabling Routing in ASP.NET Core Web API Application:
  * UseRouting(): The UseRouting Middleware only enables the Routing for your application. This will not map any URL to any resource.
  * UseEndpoints(): This middleware will map the URL to the resource. But the most important point that you need to remember is, the action methods are not only the resource 

![image](https://user-images.githubusercontent.com/71544024/154134019-67360a9c-ef02-4025-bf40-7acd9c47892b.png)

* Understanding Route Attribute in ASP.NET Core:
  * Using the Route Attribute, we can define the Routes in ASP.NET Core Application either at the Controller level or at the action method level. Before using the Route        *   * Attribute lets us have a look at the signature of the Route Attribute. The following image shows the definition of the Route Attribute.
```
using Microsoft.AspNetCore.Mvc;
namespace RoutingInASPNETCoreWebAPI.Controllers
{
    [ApiController]
    public class EmployeeController : ControllerBase
    {
        [Route("Emp/All")]
        public string GetAllEmployees()
        {
            return "Response from GetAllEmployees Method";
        }
        [Route("Emp/ById")]
        public string GetEmployeeById()
        {
            return "Response from GetEmployeeById Method";
        }
    }
}
```
![image](https://user-images.githubusercontent.com/71544024/154134635-e4bfc016-3bec-47ec-b9a6-8e0c8901c64f.png)
![image](https://user-images.githubusercontent.com/71544024/154135098-a5febb2a-8e5b-471e-947f-47433690480a.png)
![image](https://user-images.githubusercontent.com/71544024/154135178-6dcf2374-d1dc-4800-a605-a5de9bce5535.png)
![image](https://user-images.githubusercontent.com/71544024/154135235-8915d11d-ac11-4d2e-87e4-509d16580133.png)
* Token Replacement in ASP.NET Core Web API Application

```
[ApiController]
    [Route("[controller]")]
    public class EmployeeController : ControllerBase
    {
        [Route("[action]")]
        public string GetAllEmployees()
        {
            return "Response from GetAllEmployees Method";
        }
 Advantages of Tokens in Attribute Routing:
The main advantage of using Token Replacement in ASP.NET Core Application is that, if you rename the controller’s name or the action method name then you do not have to change the route templates. The application is going to works with the new controller and action method names.

* Do we need to write the action token on each action method? 
*NO

using Microsoft.AspNetCore.Mvc;
namespace RoutingInASPNETCoreWebAPI.Controllers
{
    [ApiController]
    [Route("[controller]/[action]")]
    public class EmployeeController : ControllerBase
    {
        public string GetAllEmployees()
        {
            return "Response from GetAllEmployees Method";
        }
        public string GetAllDepartment()
        {
            return "Response from GetAllDepartment Method";
        }
    }
}

[ApiController]
    [Route("[controller]/[action]")]
    public class EmployeeController : ControllerBase
    {
        [Route("{Id}")]
        public string GetEmployeeById(int Id)
        {
            return $"Response from GetEmployeeById Method, Id : {Id}";
        }
    }
 
 How to override the Controller level Route Attribute at the action method level?
 [Route("~/department/all")]
public string GetAllDepartment()
{
    return "Response from GetAllDepartment Method";
}

```

* Route Constraints
  * Type: int, double, bool, float, datetime, etc [Route(“{EmployeeId:int}”)]
  * Min: min(number) [Route("{EmployeeId:int:min(1000)}")]
  * Max: max(number) [Route("{EmployeeId:int:max(1000)}")]  | [Route("{EmployeeId:int:min(100):max(1000)}")]
  * Range: range(10. 15) [Route("{EmployeeId:int:range(100,1000)}")]
  * Alpha: alpha [Route(“{EmployeeName:alpha}”)]
  * MinLength: minlength(5)  [Route("{EmployeeName:alpha:minlength(5)}")]
  * MaxLength: maxlength(10) [Route("{EmployeeName:alpha:maxlength(5)}")]
  * Length: length(10) [Route("{EmployeeName:alpha:length(5)}")]
  * Required: required 
  * Regex: regex(expression)  [Route("{EmployeeName:regex(a(b|c))}")]
* So, the Specific return type that we can return from an ASP.NET Core Web API Controller action method are as follows:
  * Any primitive data types from action methods such as int, string, bool, etc.
  * Any complex data object such as Employee, Student, Product, etc.
  * Collection of objects (like List<T> etc)
  * IEnumerable<T>
  * IAsyncEnumerable<T>, etc.
  * Benefits of using Specific Return Type in ASP.NET Core Web API:
  * While using the swagger or similar type of application, there is no need to define ProducesResponseType because we have defined the return type explicitly.
  * The drawback of using Specific Return Type in ASP.NET Core Web API:
  * You cannot return multiple types of data, let’s say NotFound, OK, Redirect, etc. which are very common in ASP.NET Core Web API to indicate the status of the request.
  * Benefits of using IActionResult type in ASP.NET Core Web API
  * It allows us to return multiple types of data along with the status code, this is very important for RESTful APIs
  * The drawback of using IActionResult type in ASP.NET Core Web API
  * As it returns multiple types of data, the swagger would not be able to identify the output, so we need to use the ProducesResponseType explicitly as shown below
```
 [Route("{Id}")]
[ProducesResponseType(StatusCodes.Status200OK, Type = typeof(Employee))]
[ProducesResponseType(StatusCodes.Status404NotFound)]
public IActionResult GetEmployeeDetails(int Id)
{
    var listEmployees = new List<Employee>()
                    {
                        new Employee(){ Id = 1001, Name = "Anurag", Age = 28, City = "Mumbai", Gender = "Male", Department = "IT" },
                        new Employee(){ Id = 1002, Name = "Pranaya", Age = 28, City = "Delhi", Gender = "Male", Department = "IT" },
                        new Employee(){ Id = 1003, Name = "Priyanka", Age = 27, City = "BBSR", Gender = "Female", Department = "HR"},
                    };

    var employee = listEmployees.FirstOrDefault(emp => emp.Id == Id);

    if (employee != null)
    {
        return Ok(employee);
    }
    else
    {
        return NotFound();
    }
}
```
* Advantages of ActionResult<T> over ActionResult in ASP.NET Core Web API:
  * ActionResult<T> offers the following benefits over the IActionResult type:

  * The [ProducesResponseType] attribute’s Type property can be excluded. For example, [ProducesResponseType(200, Type = typeof(Employee))] is simplified to    [ProducesResponseType(200)]. The action’s expected return type is instead inferred from the T in ActionResult<T>.
Implicit cast operators support the conversion of both T and ActionResult to ActionResult<T>. T converts to ObjectResult, which means return new ObjectResult(T); is simplified to return T.
![image](https://user-images.githubusercontent.com/71544024/154139768-a62f3546-fbb6-43b2-97a9-a08d781d7015.png)
![image](https://user-images.githubusercontent.com/71544024/154139788-c432890d-1d1b-4b33-8c42-a69d75f83a14.png)

 
 
# Identity Server

https://youtu.be/HJQ2-sJURvA
Install templates : dotnet new -i identityserver4.templates

