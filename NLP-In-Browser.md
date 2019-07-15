#### Natural Language Processing... in the Browser???

Not too long ago I was looking for a way to explore React Hooks and Material UI v4 and just generally brush up on some frontend basics as they are in current year. I came to JS by way of Python and I thought to myself "hey maybe I can `npm install` some of the data-sciency stuff I'm used to `pip install`-ing."

![Bad idea](https://media.giphy.com/media/3o7bugfnfUypgWzwrK/giphy.gif)

Anyway it turns out you can. In this post I'm going to:

1. Briefly introduce core concepts
2. Show how [compromise.js](https://github.com/spencermountain/compromise) enables us to do some basic NLP in a React app.
3. Cover pros and cons of this approach

You can see a demo using a bare-bones React chat client here: [https://chatter-nlp.charlesdlandau.net](https://chatter-nlp.charlesdlandau.net).

And you can see the source code for demo here: [https://github.com/CharlesDLandau/chatter_nlp](https://github.com/CharlesDLandau/chatter_nlp)

Here's a capture of it in the messaging view:

![Message view](https://serve-md.charlesdlandau.net/img/chatterNLPBlogMessageView.jpg)

And here is the analysis view:

![Analysis view](https://serve-md.charlesdlandau.net/img/chatterNLPBlogAnalysisView.jpg)

##### 1. Core Concepts

*Natural Language Processing (NLP)* tries to extract meaning, semantics, sentiment, tags, named entities, and more from text. I'm oversimplifying but I have a good excuse I swear. Chatbots, speech recognition, and search are some of the use cases for NLP.

*Tags* in NLP represent parts of speech like "verb" or "article", but you can also call more specific designations a tag, like "WeekDay". Compromise ships with a nice set of tags [https://observablehq.com/@spencermountain/compromise-tags](https://observablehq.com/@spencermountain/compromise-tags) and extensibility for adding new ones.

*Corpus* the body of text being analyzed. For example, if you were doing NLP and analysis on a book, that book is your corpus.

*Documents* are each unit of text being analyzed. For example, in the demo chat app, each message constitutes a document.

*TF-IDF* is a method for measuring which words are most meaningful in a corpus. It's the product of how often a word appears in a single document versus how often it appears all documents. The measure is ["highest when the term occurs many times within a small number of documents"](https://nlp.stanford.edu/IR-book/html/htmledition/tf-idf-weighting-1.html)

##### 2. Show and tell

Mostly, the demo app is responsible for passing around a `messages` array. The array gets initialized at the top of the component hierarchy, used for demoing NLP processes, and parsed for dataviz.

````
const App = (props) => {

  const [messages, setMessages] = useState(dummyMessages)
  const [user, setUser] = useState("red");

  // Append new messages in a user-aware manner
  const mountMessage = (e, contents) =>{
      var text = contents
      var msgUpdate = messages
      msgUpdate.push({text:text, time: new Date().toLocaleString(),
      author:user})
      setMessages(msgUpdate)
      if (user === 'red'){
        setUser('blue')
      }else{
        setUser('red')
      }
      e.preventDefault()
    }


  return (...);

}
````

In this demo I didn't really care about the logic of multiple users, or named users, or really anything other than having two users, so "red" and "blue" pass around the `user` state, and `messages` contain pretty much all the data we care about.

Eventually, all the analysis happens in a class `TextAnalysis`.

````
import nlp from 'compromise';

class TextAnalysis{
	constructor(docs){
		this.docs = docs
		this.mergedDocs = nlp(
			this.docs.map(obj => obj.text).join()
		)
	}
...

};
````
Mostly, `TextAnalysis` is consumed via its `.cardData` method, which returns hardcoded objects like:


````
{
  title: "Parts of Speech",
  chartData: {
    labels: ["Noun", "Verb", "Adjective"],
    series:[
    this.mergedDocs.match('#Noun'
      ).out('array').length,
    this.mergedDocs.match('#Verb'
        ).out('array').length,
    this.mergedDocs.match('#Adjective'
        ).out('array').length
    ]},
  chartType: 'Pie',
  chartOpts: {
    chartPadding: 30,
    labelOffset: 30,
    labelDirection: 'explode'
  }
}
````
What's going on here?

`compromise` analyzed all the text from all the messages in the `constructor` and stored it in `this.mergedDocs`. So, many of the methods of a `compromise` object are exposed by `this.mergedDocs`, including `.match()` for matching tags.

So, we can populate the `chartData` with the number of matches for parts of speech:
````
[
this.mergedDocs.match('#Noun'
  ).out('array').length,
this.mergedDocs.match('#Verb'
    ).out('array').length,
this.mergedDocs.match('#Adjective'
    ).out('array').length
]
````

Note the `.out` method exposed by `compromise`, this is typically how we extract parsed data from analyzed documents. [It supports parsing to text, arrays, html, normalized text, and even csv among others](https://observablehq.com/@spencermountain/compromise-api).

These and `chartOpts` and `chartType` get passed on to [Chartist](https://gionkunz.github.io/chartist-js/), which we're using for dataviz.

````
// Parses a single object from TextAnalysis.cardData()
function AnalysisCard(props){
  var { data } = props
  const classes = useStyles();

  return (
    <Grid item>
        <Card className={classes.card}>


        <CardHeader className={classes.cardHead} title={
          <Typography style={
            {textOverflow:'ellipsis', whiteSpace:'nowrap'}
          }
           variant='subtitle2'>
          {data.title}</Typography>
        } />


        <ChartistGraph
        data={data.chartData}
        type={data.chartType}
        options={data.chartOpts} />
        </Card>
    </Grid>
  )
}
````
That's all it took!

...almost. Compromise doesn't seem to ship with a TF-IDF vectorizer (I'm spoiled by Scipy). So, within `TextAnalysis` we can implement our own...

````
tf(d, occ){
  // Takes a document and N occurrences of a term
  // Returns the term frequency (tf)
  // tf = (occurrences of search term/N terms)
  return (occ/nlp(d.text).terms().out('array').length)
}

idf(t){
  // Takes a term
  // Returns the inverse document frequency (idf)
  // idf = log_e(N documents/N documents containing
  // the search term)

  var nDocs = this.docs.length
  var nMatches = this.docs.filter(
    doc=>{
      var matched = doc.text.match(t)
      if(matched){
        return true}
      else{
        return false}
      }
  ).length

  var result = nDocs / nMatches
  if (!isFinite(result)){
    return 0
  }else{
  return Math.log(result)
  }
}

tfIdf(doc){
  // Takes a document from this.docs
  // Returns a sorted array of objects in the form:
  // {term:<String>, weight:<Float>}
  // This is a vector of terms and Tf-Idf weights



  var tfIdfVector = nlp(doc.text).terms().out('freq').map((d)=>{
    var t = d['normal']


    var tf = this.tf(doc, d['count'])

    var idf = this.idf(t)

    return {term: t, weight:tf*idf}
    }
  )

  var sortedTfIdfVector = tfIdfVector.sort((obj0, obj1)=>{
    var w0 = obj0.weight
    var w1 = obj1.weight
    if (w0 < w1){
      return 1
    }
    if (w0 > w1){
      return -1
    }
    return 0
  })

  return sortedTfIdfVector

}
````

(This felt more than a little hacky, so if anybody critiques my implementation that would be quite welcome.)

With that, we can also chart the top weighted words for a random message!

![Plotted TFIDF](https://serve-md.charlesdlandau.net/img/chatterNLPBlogTFIDF.jpg)


##### Pros and cons

I don't know if you should do this, or at least if you do this you should really think hard about why.

**Cons**
1. You're using the user's browser to do the analysis. The same browser that's serving them that beautiful user experience you've been slaving over. Doing this in a way that definitely won't degrade the UX isn't impossible.
2. Compromise is ~200kb and the lead author [says you probably can't shake that tree](https://github.com/spencermountain/compromise#--of-course-theres-a-lot-more-stuff). That's not huge but it's not small either.
3. Is data preprocessing already a goal for the frontend? Is your organization going to make it one? Does this require dropping a bunch of code from your team into a codebase mostly maintained by another team? Have you taken their temperature about that yet?
4. One of the benefits of doing preprocessing in the backend is that you can operate on your whole dataset -- in the browser we can only calculate TFIDF using the messages *in the browser*, in the backend we could get a more useful weight using all the messages.

**Pros**
1. You're using the user's browser to do the analysis. Maybe that analysis costs a lot to run on AWS or elsewhere...
2. All the insights can be fed back into client and shared with the user (e.g. the analysis view in our demo).
3. More analysis in the browser means you could potentially find a way to do more filtering in the browser, ultimately leading to fewer calls to your API.

##### Further reading:

**Compromise**: [https://github.com/spencermountain/compromise](https://github.com/spencermountain/compromise)
**Chartist**: [https://gionkunz.github.io/chartist-js](https://gionkunz.github.io/chartist-js)
**Demo source**: [https://github.com/CharlesDLandau/chatter_nlp](https://github.com/CharlesDLandau/chatter_nlp)


##### Feedback welcome!

I took on this mini-project as a way to experiment with something funky. I'm sharing it here because I'm interested in people's reactions and to always learn more. Thanks for reading!
