---
layout: post
title:  "Start .NET Core MVC on OS X"
date:   2016-07-27 11:11:1 +0700
categories: dotnet
---

It is very easy to get started with [.NET Core][dotnet-core] on your platform of choice.
Select the package that match with your platform and install it.

You just need a shell, a text editor and 10 minutes of your time.<br>

Pre Requisites
====
```
~ brew update
~ brew install openssl
~ brew link --force openssl
```

Crate Project
====
```
~ mkdir yoweb
~ cd yoweb
~ dotnet new
```

Run
====
```
~ dotnet restore
~ dotnet run
```
Check the result.
{% highlight ruby %}
Hello World!
{% endhighlight %}

So, After complete setup, Please open project.json and add AspNetCore dependencie inside dependencies.

```
"Microsoft.AspNetCore.Server.Kestrel": "1.0.0"
```

Final project.json
{% highlight json %}
{
  "version": "1.0.0-*",
  "buildOptions": {
    "debugType": "portable",
    "emitEntryPoint": true
  },
  "dependencies": {},
  "frameworks": {
    "netcoreapp1.0": {
      "dependencies": {
        "Microsoft.NETCore.App": {
          "type": "platform",
          "version": "1.0.0"
        },
        "Microsoft.AspNetCore.Server.Kestrel": "1.0.0"
      },
      "imports": "dnxcore50"
    }
  }
}
{% endhighlight %}

Then, Create Startup.cs
{% highlight ruby %}
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;

namespace YoWeb
{
    class Startup
    {
        public void Configure(IApplicationBuilder app)
        {
            app.Run(context =>
            {
                return context.Response.WriteAsync("Hello from ASP.NET Core!");
            });
        }
    }
}
{% endhighlight %}

And Update Program.cs
{% highlight ruby %}
using Microsoft.AspNetCore.Hosting;

namespace YoWeb
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseKestrel()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}
{% endhighlight %}

Finally, restore and run, You will see log below. 

```
Project YoWeb (.NETCoreApp,Version=v1.0) will be compiled because Input items removed from last build
Compiling YoWeb for .NETCoreApp,Version=v1.0

Compilation succeeded.
    0 Warning(s)
    0 Error(s)

Time elapsed 00:00:03.7725448


Hosting environment: Production
Content root path: /Users/nuboat/Projects/YoWeb/bin/Debug/netcoreapp1.0
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Full Source Code of this blog on [Github/nuboat][dotnet-core-exam]

Check out the full Tutorial of [.NET Core][dotnet-core-toutrial]

Wrote by [Peerapat A][nuboat-linkedin]

[nuboat-linkedin]: https://th.linkedin.com/in/peerapat
[dotnet-core]: https://www.microsoft.com/net/core#macos
[dotnet-core-exam]: https://github.com/nuboat/yoweb
[dotnet-core-toutrial]: https://docs.asp.net/en/latest/intro.html
