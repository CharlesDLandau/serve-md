#### Design Principles for World Class Reinforcement Learning on AWS DeepRacer

![The DeepRacer track at AWS Summit NYC](/img/deepRacer072019Track.jpg)

Yesterday was a really fun day at AWS Summit in New York, and I wanted to sit down and record my thoughts. We also happened to place 5th in the AWS DeepRacer competition there. So, in this post I'm going to give some TLDRs and "see also"s about DeepRacer, share the design principles that enabled us to succeed, and then wrap up with my general thoughts about the conference. I promise that there will be no puns in this post.


##### DeepRacer and RL

**DeepRacer, briefly:** It's a racing league. The participants are coaches, who "train" reinforcement learning models to drive the car. Races are held at AWS summits across the globe, and there are also races held only on simulators ("virtual races"). You do the training on simulators provided by AWS. Standing is an empirical demonstration that your shop has (literally!) world class data science chops -- and big name organizations are participating accordingly. As far as I am concerned, everyone who manages to get the car around the track AT ALL has accomplished a very hard task and I respect their skills.

![The DeepRacer standings for the NYC race.](/img/deepRacer072019LeaderBoard.jpg)

[Check out all the leaderboards here.](https://aws.amazon.com/deepracer/schedule-and-standings/)

**Reinforcement Learning, briefly:** Instead of optimizing a model for predicting labels (supervised learning) or a model that finds patterns in unlabeled data (unsupervised learning), **RL** takes an "action space" and "environment" as input, and it optimizes on a "reward function."

DeepRacer does a great job of introducing themselves and RL in their materials, so if you want to go beyond TLDR, then... see also: [https://aws.amazon.com/deepracer/](https://aws.amazon.com/deepracer/)

And if you get bored in DeepRacer, check out OpenAI, who have a huge footprint in RL: [http://gym.openai.com/](http://gym.openai.com/)

`**`*I should note I haven't personally played with OpenAIGym much*


##### The Problem

In DeepRacer, the *environment* is the race track, the *action space* are all the combinations of speed and steering angle , and the reward function is provided by you. The model is provided by AWS SageMaker, the flagship MLaaS offering on AWS. They didn't quite confirm when I asked them if it was just an optimized TensorFlow neural net, but I think it is. You can't touch the model directly, but there are some "hyperparameters" that impact how the training works, and you can edit those within ranges.

So in general, our task is accomplished by defining:

* The reward function
* How much throttle / how many throttle settings can the model select.
* How sharply can it turn / how many angles in that range can it select.
* ...and hyperparameters like..
1. *learning rate* (how the model updates) and...
2. *entropy* (how often the model takes a random action to ensure all the actions are getting trained on) and...
3. *training time* (how long the model trains **and therefore how much it costs you to train it**)

Given the problem, these are three design principles that I've adopted:

##### Principle One: Anthropomorphize

Your reward function is probably the most important thing you're doing in this process. The reward function has predefined inputs documented by AWS, like "distance from center line" or "speed". So what would you do if you were teaching somebody to race a car? You give them a high five when they complete a race, and you dress them down for stupid stuff like sitting still or crashing. For everything else, you advise. "Go faster" you say. "Turn like so" you say. Your model is the racer, you are the coach, and your reward function is how you "talk" to it.

##### Principle Two: KISS

Keep It Simple Stupid. What does that look like in this context?

**One:** Focus on your reward function. Write simple functions that extract signals about what's happening with the car, such as "going fast" or "crashed". DeepRacer has boilerplate code to help you do this, but they present those as full reward functions. **They're not.** Those are signal extraction functions -- your reward function interprets the totality of the signals. Simple signal extraction functions, simple reward function.

**Two:** Standardize on hyperparameters and training times. I found that the default hyperparameters were chosen sensibly. I standardized on 120 minute training sessions.

**Three:** Resist the "big red button" and "min/max"ing. As practicioners, it's natural to marvel at the power of the levers at hand. Resist the urge to max out your training time (or whatever) on a promising model. GOTO 1.

##### Principle Three: Go Fast

This should be self explanatory, so why am I mentioning it? The model is solving a complex problem involving steering, speed, but also longevity and stability. DeepRacer poses all those problems to you, but it's fundamentally a race. In some other context, like with real cars, you wouldn't accept a model that crashes. **This isn't that.** Go as fast as you can without crashing all the time. Yes, punish crashing. Yes, steering is important. Still, you are training a racer and racers wanna go fast. When your focus is on one of the other parts of the problem, keep it in the back of your mind.

##### Bonus Principle: Have Fun

There are a lot of companies, some of them pretty large, that are represented in the top teir of this race. Some of them have a car and a track of their own and run in-house competitions in parallel to the global DeepRacer league. **Resist the urge to be discouraged by this.** This is very similar to how traditional racing leagues play out...**but unlike those leagues the barrier to entry is very low.** The cost of a training hour is about 3USD. On a couple hundred bucks we were able to acheive world class performance in reinforcement learning and give teams from Fortune 100 companies like Capital One a run for their money. **So relax and have fun.** Otherwise what's the point of all this [gamification](https://en.wikipedia.org/wiki/Gamification)?

I think if you follow these priciples you can make a race-worthy model and have an awesome time doing it.

![Inserting the model into the car](/img/deepRacer072019InsertingModel.jpg)

Putting the model in the car.

##### Congratulations to the medalists, [Atchima, Michael and Nelson](https://twitter.com/AWSonAir/status/1149442689940819969)!



##### Other Thoughts on the Summit:

1. We met a BUNCH of cool people.
2. We learned about a ton of cool services and technologies (Honorable mentions include [AWS CDK](https://aws.amazon.com/cdk), [Kong](https://konghq.com/kong/), [DataDog](https://www.datadoghq.com/))
3. We had BUCKETS of fun. (Ok I had to have one pun.)
