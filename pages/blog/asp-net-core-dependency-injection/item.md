---
title: 'ASP.NET Core - Dependency Injection'
media_order: maksim-sislo-hs5IHFv3JvQ-unsplash.jpg
date: '20:57 13-03-2021'
taxonomy:
    category:
        - 'c#'
        - asp.net
        - 'dependency injection'
        - ioc
    tag:
        - IoC
        - 'C#'
        - 'Dependency Injection'
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: maksim-sislo-hs5IHFv3JvQ-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

ASP.NET Core is designed from scratch to support Dependency Injection. ASP.NET Core injects objects of dependency classes through constructor or method by using built-in IoC container.

===

### Built-in IoC Container
ASP.NET Core framework contains simple out-of-the-box IoC container which does not have as many features as other third party IoC containers. If you want more features such as auto-registration, scanning, interceptors, or decorators then you may replace built-in IoC container with a third party container.

The built-in container is represented by `IServiceProvider` implementation that supports constructor injection by default. The types (classes) managed by built-in IoC container are called services.

There are basically two types of services in ASP.NET Core:

* Framework Services: Services which are a part of ASP.NET Core framework such as `IApplicationBuilder`, `IHostingEnvironment`, `ILoggerFactory` etc.

* Application Services: The services (custom types or classes) which you as a programmer create for your application.
In order to let the IoC container automatically inject our application services, we first need to register them with IoC container.

### Registering Application Service
Consider the following example of simple `ILog` interface and its implementation class. We will see how to register it with built-in IoC container and use it in our application.

>     public interface ILog
>     {
>         void info(string str);
>     }
>     
>     class MyConsoleLogger : ILog
>     {
>         public void info(string str)
>         {
>             Console.WriteLine(str);
>         }
>     }

ASP.NET Core allows us to register our application services with IoC container, in the `ConfigureServices` method of the `Startup` class. The `ConfigureServices` method includes a parameter of `IServiceCollection` type which is used to register application services.

Let's register above `ILog` with IoC container in `ConfigureServices()` method as shown below.

Example: Register Service Copy

>     public class Startup
>     {
>         public void ConfigureServices(IServiceCollection services)
>         {
>             services.Add(new ServiceDescriptor(typeof(ILog), new MyConsoleLogger()));        
>         }
>     
>         // other code removed for clarity.. 
>     }

As you can see above, `Add()` method of `IServiceCollection` instance is used to register a service with an IoC container. The `ServiceDescriptor` is used to specify a service type and its instance. We have specified `ILog` as service type and `MyConsoleLogger` as its instance. This will register `ILog` service as a singleton by default. Now, an IoC container will create a singleton object of `MyConsoleLogger` class and inject it in the constructor of classes wherever we include `ILog` as a constructor or method parameter throughout the application.

Thus, we can register our custom application services with an IoC container in ASP.NET Core application. There are other extension methods available for quick and easy registration of services which we will see later in this chapter.

### Understanding Service Lifetime
Built-in IoC container manages the lifetime of a registered service type. It automatically disposes a service instance based on the specified lifetime.

The built-in IoC container supports three kinds of lifetimes:

* Singleton: IoC container will create and share a single instance of a service throughout the application's lifetime.

* Transient: The IoC container will create a new instance of the specified service type every time you ask for it.

* Scoped: IoC container will create an instance of the specified service type once per request and will be shared in a single request.

The following example shows how to register a service with different lifetimes.

Example: Register a Service with Lifetime Copy

>     public void ConfigureServices(IServiceCollection services)
>     {
>         services.Add(new ServiceDescriptor(typeof(ILog), new MyConsoleLogger()));    // singleton
>         
>         services.Add(new ServiceDescriptor(typeof(ILog), typeof(MyConsoleLogger), ServiceLifetime.Transient)); // Transient
>         
>         services.Add(new ServiceDescriptor(typeof(ILog), typeof(MyConsoleLogger), ServiceLifetime.Scoped));    // Scoped
>     }


### Extension Methods for Registration
ASP.NET Core framework includes extension methods for each types of lifetime; `AddSingleton()`, `AddTransient()` and `AddScoped()` methods for singleton, transient and scoped lifetime respectively.

The following example shows the ways of registering types (service) using extension methods.

Example: Extension Methods Copy
>     public void ConfigureServices(IServiceCollection services)
>     {
>         services.AddSingleton<ILog, MyConsoleLogger>();
>         services.AddSingleton(typeof(ILog), typeof(MyConsoleLogger));
>     
>         services.AddTransient<ILog, MyConsoleLogger>();
>         services.AddTransient(typeof(ILog), typeof(MyConsoleLogger));
>     
>         services.AddScoped<ILog, MyConsoleLogger>();
>         services.AddScoped(typeof(ILog), typeof(MyConsoleLogger));
>     }

### Constructor Injection
Once we register a service, the IoC container automatically performs constructor injection if a service type is included as a parameter in a constructor.

For example, we can use `ILog` service type in any MVC controller. Consider the following example.

Example: Using Service Copy

>     public class HomeController : Controller
>     {
>         ILog _log;
>     
>         public HomeController(ILog log)
>         {
>             _log = log;
>         }
>         public IActionResult Index()
>         {
>             _log.info("Executing /home/index");
>     
>             return View();
>         }
>     }

In the above example, an IoC container will automatically pass an instance of `MyConsoleLogger` to the constructor of `HomeController`. We don't need to do anything else. An IoC container will create and dispose an instance of `ILog` based on the registered lifetime.

### Action Method Injection
Sometimes we may only need dependency service type in a single action method. For this, use `[FromServices]` attribute with the service type parameter in the method.

Example: Action Method Injection Copy

>     using Microsoft.AspNetCore.Mvc;
>     
>     public class HomeController : Controller
>     {
>         public HomeController()
>         {
>         }
>     
>         public IActionResult Index([FromServices] ILog log)
>         {
>             log.info("Index method executing");
>     
>             return View();
>         }
>     }

### Property Injection
Built-in IoC container does not support property injection. You will have to use third party IoC container.

### Get Services Manually
It is not required to include dependency services in the constructor. We can access dependent services configured with built-in IoC container manually using `RequestServices` property of `HttpContext` as shown below.

Example: Get Service Instance Manually Copy

>     public class HomeController : Controller
>     {
>         public HomeController()
>         {
>         }
>         public IActionResult Index()
>         {
>             var services = this.HttpContext.RequestServices;
>             var log = (ILog)services.GetService(typeof(ILog));
>                 
>             log.info("Index method executing");
>         
>             return View();
>         }
>     }

It is recommended to use constructor injection instead of getting it using `RequestServices`.


### Source
[Tutorials teacher - ASP.NET Core - Dependency Injection](https://www.tutorialsteacher.com/core/dependency-injection-in-aspnet-core)

[Archive - https://archive.ph/6PV9V](https://archive.ph/6PV9V)
