---
title: 'JsonResult Serializer Settings in .NET Core 6'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
hero_image: annie-spratt-rnxM6fx3U5Y-unsplash.jpg
---

If you are returning a JSON representation of an object from an MVC controller it may be necessary to determine how the JSON is formatted.

This can be done using the `JsonSerializerSettings` object like so.

===

>    public async Task<JsonResult> AuditableResources(DateTime? startDate, DateTime? endDate, string resourceType)
>    {
>    	JsonSerializerSettings jsonSerializerSettings = new JsonSerializerSettings()
>    	{
>    		ReferenceLoopHandling = ReferenceLoopHandling.Ignore,
>    		ContractResolver = new CamelCasePropertyNamesContractResolver()
>    	};
>    
>    	List<Resource> resources = new List<Resource>();
>    
>    	// Get resources here...
>    
>    	return new JsonResult(resources, jsonSerializerSettings);
>    }
    
This used to work as is with version 2 of .NET Core but with the addition of the `System.Text.Json` namespace in >v3 this will no longer work and the below error will be recieved.

>    System.InvalidOperationException: Property 'JsonResult.SerializerSettings'
>        must be an instance of type 'System.Text.Json.JsonSerializerOptions'.

In order to fix this you need to explicitly tell the controller that Newtonsoft should be used to format JSON as stated here.

This can be done by adding the package `Microsoft.AspNetCore.Mvc.NewtonsoftJson` to your project and adding the following code to the `ConfigureServices` method of your startup file.

>    public void ConfigureServices(IServiceCollection services)
>    {
>        services.AddControllers()
>            .AddNewtonsoftJson();
>    }