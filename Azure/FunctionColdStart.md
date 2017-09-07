# Understanding cold start of Azure Functions

"Serverless" style of application development brings a lot of convenience but comes with some trade-offs. In this article we'll examine the issue of cold-start with [Azure Functions](https://azure.microsoft.com/en-us/services/functions/). 

### What is "cold-start"? 

Cold start is a condition when a web app gets its first request, before the application code had a chance to initialize. Depending on complexity of the application, initializing a cold app may take a long time. As a result, the first request will experience increased latency. That's why in the good old days (before the cloud) people used to "warm-up the web site" after deploying new version of the code to their web server. But generally speaking, cold start can happens anytime (not only after deployment), for example after server restart or if the app got unloaded for various reason.

Similarly, an Azure Function may be in a cold state, which may cause a delay in execution of such Function. This may be a problem, if the Function is part of an interactive flow where a user is actually waiting for results of a button click, etc. On the other hand, if the Function is used in background processing of non-interactive events, it may not be a big deal for it to take extra few seconds to execute.

### How bad is it?

Cold vs. warm execution times can vary by 3 orders of magnitude. In my completely unscientific experimentation I observed cold start times of over 10 seconds, with subsequent warm calls to the same function as fast as 20 milliseconds (which is pretty much my latency to the Datacenter, according to [Azure Latency test](http://www.azurespeed.com/)). Of course, 10 seconds delay would probably be unacceptable in most user-facing scenarios. Moreover, such delay may cause a timeout and error, which is even worse user experience.

### Why does a Function get cold?

There are several factors that may be contributing to Function cold-start:
* How often is a Function invoked?
* Is the load pattern spiky or smooth?
* What language is the Function written in?
* What App Service Plan does the Function run under?

Let's consider these in more detail.

### Idle timeout

One of the more obvious reasons for a Function getting cold is being unloaded from the system after some idle timeout. The exact length of this timeout is not documented, but the internet wisdom says it's 5 minutes. In my experiments I actually saw 20 min idle timeout, and this is consistent with idle timeout for Azure Websites to which Functions are closely related.

Idle timeout can be prevented by enabling "Always On" mode in the Application Settings of the Function App. This setting is only available for Functions running under an [App Service Plan](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale) (Basic and above). In fact, it is enabled by default under those plans. Naturally, when you are paying for a monthly Service Plan for your Function App, Azure can keep your functions warm all the time. At that point it's a matter of deciding what makes more sense from cost perspective in your particular scenario. 

One word of caution: it's not possible to change a Function App from Consumption mode to App Service Plan. This requires recreating the whole Function App. There are also other subtle differences between Consumption and App Service Plan (see [here](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits), for example), so please choose wisely.

What if you want to have the best of both worlds: convenience of Consumption plan, and the warm response time for your Functions? One commonly suggested workaround is to have a timer-triggered warm-up calls every few minutes. This is not very elegant and does incur extra cost, but it is an option to consider.

### Spiky traffic

Let's say you have a Function on Consumption plan and you are able to keep it warm all the time, either using timer or due to natural traffic coming in regularly. You may still experience cold start in case of sudden spikes in the traffic. In this case Function App will try to do automatic scale-out and bring in additional instance to execute the Function. Those new instances will be cold and some of the requests may be hit with elevated latency.

### Choice of language

I came across a suggestion to pre-compile C# Functions (see [here](https://marketplace.visualstudio.com/items?itemName=AndrewBHall-MSFT.AzureFunctionToolsforVisualStudio2017)) in order to improve their cold start times. While it may be a good practice from the development perspective (checked-in code, unit testing, etc.), it didn't improve the cold-start response in my experience. I tried C# script, compiled C#, or JS functions -- they all exhibited cold start of up to 12 seconds, and the same 20 minute idle timeout.

### Conclusion

Initial request penalty is a "cold reality" (pun intended) of the serverless world today. Perhaps the cold start problem will be eliminated as the technology matures. At least for now, Azure gives us a choice of mitigation techniques such as the "Always On" mode, for example. The hope of this article is that our fellow developers building on serverless architecture will never be caught off-guard by issues of "cold start".
