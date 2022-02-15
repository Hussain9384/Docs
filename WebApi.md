# Docs
All the document

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

Now if you compare the above code with the Program class auto-generated by ASP.NET Core Web API Application, then you will find everything as the same.
# Identity Server

https://youtu.be/HJQ2-sJURvA
Install templates : dotnet new -i identityserver4.templates

