Not too long ago I discovered the work of a very smart and snarky cloud economist by the name of Corey Quinn. There’s a lot to admire in how Corey delivers content, his perspective on the industry, and his sense of humor. Having said that, I’ve been on the k8s learning curve for a little while now, and [Corey’s pretty bearish on the topic.](https://twitter.com/QuinnyPig/status/1121462122741940226)

Corey is one of many cloud computing advocates taking this position, offered here as an example. He’s not the only one, and I even see Kelsey Hightower, one of the original evangelists for k8s, urging some caution. “Up and Running”, a book Kelsey co-authored, was my introduction to k8s, and I’ve tried to pay [close attention to Kelsey ever since.](https://twitter.com/kelseyhightower/status/1245886920443363329)
	
I think all of the above is fair, and I think the point about k8s complexity is hard to argue with. It’s easy for somebody to read expert opinions about this technology and get the wrong signal, because you might be starting with the wrong question. In this post, I’m going to urge you not to begin with the question “do I need docker or kubernetes?” Instead, begin with the question “could we get value from the technical and management practices required to express a Dockerfile? A k8s podspec?”

## The iceberg of business value

![Highly Scientific Chart](https://github.com/CharlesDLandau/serve-md/img/tools_automation.PNG)

As my question suggests, I have come to believe that the business process required to write a Dockerfile is more valuable than the tooling itself, and can require much more effort than the actual investment in Docker. Is there scripting in place to automate the build and run processes in a reproducible manner? Are all the dependencies fully documented? Have you followed the [12 Factor App](https://12factor.net/) framework or similar? Addressing these questions is where most of the value comes from, and Docker happens to be a particularly successful technology that helps organizations deliver after they’ve done so.

The same can be said about your services. The business and technical processes can go beyond expressing their build and run steps in a Dockerfile and can be extended to expressing their services as k8s podspecs.

Lastly, don’t trivialize that this is a genuine question about value! Maybe it makes more sense for your team to go the way of the [Majestic Monolith](https://m.signalvnoise.com/the-majestic-monolith/). Go where the value is regardless of hype. If the technology you want to work with isn’t valuable to the shop you work in, perhaps that’s a signal for you to consider other employment, not a signal for the business to change value streams.

## Closing thoughts

As I mentioned, I’m still on the learning curve for these technologies, so please take these words with a grain of salt. My hope is that this post has given you an alternative point of view on value creation in the container and container orchestration space. It’s hard to stay grounded when you’re learning about very hyped technologies, and refocusing on business value is how I’ve learned to cope.
