---
layout: post
title:  "First Controller with Dependency Injection"
date:   2016-07-30 11:11:1 +0700
categories: dotnet
---

This post will describe how to create simple controller and integrate dependency injection in .NET Core<br>
Before continue please make sure you pass [first part][dotnet-start-mvc-core] before continue.

B'coz I love Dependency Injection (DI), So this blog will show how to create /api/hello/{name} in DI Style.<br> 
Let's start by add mandatory dependency into project.json

```
  "dependencies": {
    "Microsoft.AspNetCore.Hosting": "1.0.0-*",
    "Microsoft.AspNetCore.Mvc": "1.0.0-*",
    "Microsoft.AspNetCore.Mvc.Core": "1.0.0-*",
    "Microsoft.AspNetCore.Server.IISIntegration": "1.0.0-*",
    "Microsoft.AspNetCore.Server.Kestrel": "1.0.0-*",
    "Microsoft.Extensions.Logging.Console": "1.0.0-*"
  },
```

Then create HelloController.cs and IHelloWorld.cs.

{% highlight ruby %}
using Microsoft.AspNetCore.Mvc;

namespace YoWeb.Module.Hello
{
    [Route("api/[controller]")]
    public class HelloController : Controller
    {
        private readonly IHelloWorld HelloWorld;

        public HelloController(IHelloWorld helloWorld)
        {
            HelloWorld = helloWorld;
        }

        [HttpGet("{name}", Name = "SayHi")]
        public IActionResult SayHi(string name)
        {
            return new ObjectResult(HelloWorld.Say(name));
        }
    }
}
{% endhighlight %}

{% highlight ruby %}
namespace YoWeb.Module.Hello
{
    public interface IHelloWorld
    {
        string Say(string name);
    }
}
{% endhighlight %}

Then, Implement IHelloWorld by create HelloWorld

{% highlight ruby %}
using YoWeb.Module.Hello;

namespace YoWeb.Facade
{
    public class HelloWorld : IHelloWorld
    {
        public string Say(string name) 
        {
            return $"Hello {name}";
        }
    }
}
{% endhighlight %}

Finally, Update Startup.cs to Support MVC & HelloWorld DI

{% highlight ruby %}
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;
using YoWeb.Facade;
using YoWeb.Module.Hello;

namespace YoWeb
{
    class Startup
    {
        public void ConfigureServices(IServiceCollection services) 
        {
            services.AddMvc();

            services.AddLogging();

            services.AddSingleton<IHelloWorld, HelloWorld>();
        }
        public void Configure(IApplicationBuilder app)
        {
            app.UseMvcWithDefaultRoute();
        }
    }
}
{% endhighlight %}

Try run http://localhost:8000/api/hello/yo to test

Full Source Code of this blog on [Github/nuboat][dotnet-core-exam]

Check out the full Tutorial of [.NET Core][dotnet-core-toutrial]

Wrote by [Peerapat A][nuboat-linkedin]

[dotnet-start-mvc-core]: https://www.pullmasi.com/dotnet/2016/07/27/start-dotnet-core-mvc.html
[dotnet-core-exam]: https://github.com/nuboat/yoweb/tree/first_controller
