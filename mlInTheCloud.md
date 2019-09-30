# 🤖➯🌩 (Machine Learning In the Cloud)

I've been preparing a talk for my local ML usergitgroup and as a result I've had to synthesize a lot of ideas. In the talk I introduce a wide variety of "Machine Learning as a Service (MLaaS)" services and talk about how they fit in your toolkit. It's supposed to be a 45 minute talk, so the deck is pretty long... much longer than you would want for a blog post. In this post I'm going to:

1. Summarize the heck outta this talk.
2. Share the slides at the end.

_Note: I am NOT affiliated with ANY product mentioned ANYWHERE in this post._

## MLaaS...

...because what the world really needs is another aaS right?

Machine Learning as a Service is a tautology -- it's a label for any service that delivers machine learning. These services offer a variety of advantages to businesses and data scientists, such as:

* Near-State of the Art performance in some cases, without the up-front cost (e.g. training on many GPUs in parallel for many hours). Big Four firms (and their cloud plays) obviously have advantages when it comes to this space, especially in terms of their access to data. (See: [Google Natural Language]([https://cloud.google.com/natural-language/](https://cloud.google.com/natural-language/)))
* “No Data Science Knowledge Required” and “AutoML” services that enable rapid prototyping and other optimizations. (See: [H20 Driverless AI](https://www.h2o.ai/products/h2o-driverless-ai/](https://www.h2o.ai/products/h2o-driverless-ai/)))
* Platforms that enhance time-to-market, optimize cost, or offer simplified integrations with supporting services. (See: [Azure ML Studio]([https://studio.azureml.net/](https://studio.azureml.net/)))
*  APIs that map endpoints to ML behaviors like training or prediction. (See: [wit.ai](https://wit.ai))
* Embed ML capabilities into devices (See: [Hover2](https://gethover.com))

## Worth it?
There are several considerations to take into account when you're evaluating MLaaS versus "rolling your own" solution:

**Cost:** MLaaS can seem to offer a way to mitigate the cost of building a Data Science team. Data Scientists, Big Data Engineers, and ML Engineers attract great compensation and benefits, and building a competitive recruitment process for these teams can also be difficult.

_Related note: you may avoid a lot of upfront costs with some MLaaS offerings, but per-call or per-use costs tend to bake that in._

**Ownership:** In many cases taking an MLaaS path means that you don’t fully own and control your models, or that you’re locked in to a platform. Rolling your own can mean having a greater degree of control. On the other hand, a proprietary model from a big 4 shop may be exactly what your organization wants.

**Rapid Prototyping:** MLaaS offerings can enable a low-cost rapid prototyping approach, especially for organizations that do not have a mature rapid-prototyping practice (a skunkworks). In this way MLaaS can extend rather than displace data science capabilities.

**Culture:** Many organizations seek to build not only a data science team but a "culture of innovation" of which their data science practice is a part. Having a data science team creates an in-house nexus that can advocate for itself and identify lines of business that may not be apparent to other teams.

It's also worth noting that all the typical limitations of each delivery model apply:

**Typical limitations of SaaS:** you only get their features when they are up (SLAs) and supported (LTS), you are budgeted for them ($$$), et cetera
**Typical limitations of in-house production:** growing pains, recruitment costs, cruft...

## Service Categories

To give a better overview of the landscape I pulled out a few categories of services that are out there for you to use. These are **non-exhaustive and overlapping** categories that map to common use cases.
![ml_service_cats](https://serve-md.charlesdlandau.net/img/ml_service_cats.png)

### 1. NLP Services
NLP is an area where SOTA results are expensive, use cases may lend themselves to generic OOTB solutions, and large enterprise may simply have better access to data than market entrants. Some services in this category include:

* [AWS Comprehend and Comprehend Medical]([https://aws.amazon.com/comprehend/](https://aws.amazon.com/comprehend/))
* [GCP Actions for Google Home, Web Content, and Android]([https://developers.google.com/actions/](https://developers.google.com/actions/))

### 2. AutoML
AutoML and "No Data Science Required" services are developed using techniques like transfer learning, heuristics, Bayesian hyperparameter tuning, platform optimizations, and more. They're probably doing proprietary stuff that we may never even hear about. Services in this category include:

* [...AutoML]([https://cloud.google.com/automl/](https://cloud.google.com/automl/))
* [Watson]([https://www.ibm.com/watson](https://www.ibm.com/watson))

Services in the AutoML space also have begun trying to distinguish themselves using "explainable AI" features. These are accomplished with techniques like LIME, which perturb the inputs and use the perturbations to infer how much the model values that input towards the final prediction.

![frog_explanation](https://d3ansictanv2wj.cloudfront.net/figure4-99d9ea184dd35876e0dbae81f6fce038.jpg)

_Image from [Marco Tulio Ribeiro]([https://www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime](https://www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime)) and [Pixabay](https://pixabay.com/en/love-valentine-s-day-pose-heart-903178/)_

### 3. ML Data Services
There's a wide world of "managed Apache" out there, for example Spark as a Service products are available at all major cloud providers. One place that's looking pretty interesting right now is data labeling, with services like:

* [Scale AI]([https://scale.com/](https://scale.com/))
* [SageMaker Ground Truth]([https://aws.amazon.com/sagemaker/groundtruth/](https://aws.amazon.com/sagemaker/groundtruth/))

The idea behind these services is to use human labelers frequently until a model can begin to infer the labels. As the model gets better at labeling the data, only the observations where the model has low confidence get referred to human labelers.

### 4. ML as an API
This might be the category with the greatest overlap to other categories, but it's the easiest to grasp. Any API that serves predictions or training endpoints, fits in this category. Some notables include:

 * [Google Vision]([https://cloud.google.com/vision/](https://cloud.google.com/vision/))
 * [Azure Personalizer]([https://azure.microsoft.com/en-us/services/cognitive-services/personalizer/](https://azure.microsoft.com/en-us/services/cognitive-services/personalizer/))

### 5. Embedded AI
I'm definitely stretching the definition of an "embedded system" when I use this category, but the basic idea is just devices that ship with built-in ML features, or that plug into your devices to imbue them with ML magic. Such products include:

* [Alexa Auto]([https://www.amazon.com/alexa-auto](https://www.amazon.com/alexa-auto))
* [Pixel Buds]([https://store.google.com/product/google_pixel_buds](https://store.google.com/product/google_pixel_buds))

### Bonus: JaaS
![jaas_examples](https://serve-md.charlesdlandau.net/img/jaas_examples.png)

Jupyter as a Service products like Google Colab and Datalore have been around for a long time now, so if you haven't checked them out yet now's the time. They're very mature. They don't replace an IDE, but have some great features and can be a great option for when you don't actually need a full-fledged IDE. Tools like [JupyterHub]([https://jupyter.org/hub](https://jupyter.org/hub)) can create a unified experience with reproducible experiments available alongside their documentation.

## Slides
If you'd like to see the whole deck, complete with notes, [here you go!](https://docs.google.com/presentation/d/e/2PACX-1vSAasYe8juHia_Q1uWp_LelK0YXHwyuFhfVxZ_hEDzYIj6h2d6P8wFkLqgBIm-t_6KK6K8RgNaSwrzi/pub?start=false&loop=false&delayms=3000)