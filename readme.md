## Project Goals

The goal of this worksheet is to do some testing to better parse and gain meaningful insight from a corpus of Yelp reviews for a particular business. Reviews is a great source of information if you're a business owner and you want to know how well certain products are doing or overall how your business is performing. Unfortunately, going through these reviews can be a painstaking effort - having to find which reviews mention a particular product and of these reviews, which are positive or negative, and what are common things that are being said in those positive or negative reviews. 

Most of the files are for testing with spaCy and regex. The cleaned up file that utilizes the testing is the Yelp - Nothing But Coffee [notebook](https://github.com/kitakoj18/yelp_analytics/blob/master/yelp_nothing_but_coffee.ipynb)

## Approaches

Typically what I've seen when it comes to topic modeling is standard tokenizing, stemming/lemmatizing, and removing stop words, which are then passed in through some unsupervised algorithm to identify common words that are mentioned, and then placing these words in some sort of word cloud. Although insight can be gained from this, I want to test other ways that will give us more than just single words that are mentioned. Rather, are there groups of words or short statements that we can extract and identify? 

Although n-grams is a possible method to approach this problem, I believe it not to be the most effective as we might miss words that should be grouped with each other but aren't because they are not directly adjacent to each other in the sentence. For example, if we use bigrams for the sentence, "There are a lot of plugs," we will get pairs ("there", "are") ("are", "a") ("a", "lot") ("lot", "of") ("of", "plugs"), without any text preprocessing. If that last pair was a common theme, we aren't really seeing the whole picture. Are there sufficient plugs or not enough of them? 

Originally, I wanted to use spaCy's functionality to parse word dependencies in a sentence. I was using this [article](https://medium.com/reputation-com-datascience-blog/keywords-extraction-with-ngram-and-modified-skip-gram-based-on-spacy-14e5625fce23) as a reference. The author of this article uses skip-grams derived from spaCy's word dependencies. 

As I was trying to test this method out, I realized that the approach could possibly be simplified - there are typical sentence structure patterns that reviews have. For example, a review might say something is some adjective in this exact structure. 'The earl gray latte is very delicious.' I try to identify these patterns and pass them through spaCy's rule-based matching to search for statements that follow these patterns, as these patterns are most likely describing something in the review related to a product or service of the business. 

Once I extracted statements that followed these selected patterns, I realized there could be some trimming before passing them through a model that would try to identify common themes in the reviews. This trimming involved mostly cutting out stopwords that wouldn't take away any meaning from the statement. 

My final dataset contained word pairs such as ('great', 'experience') and ('scarce', 'menu'), which I vectorized and passed through a Latent Dirichlet Allocation model to extract common themes. Here I realized that common 'themes' was not the right way to think about it. Reviews rarely have a specific theme or topic to them - more likely it's a piece of text with some sort of postiive or negative sentiment around it that talks about multiple aspects of the reviewer's experience and hence there's not a specific 'theme' that unsupervised models like LDA will help us find. Instead, they can help us see what common things are being said together in groups of reviews. Maybe one group of reviews talks about how the business has easy parking and number of tables available to study, while another group mentions how good a certain product is and the great staff. 

From here, I thought about other ways to simplify sorting through reviews for a business owner - maybe the modeling approach wasn't necessary or is something nice to have as an alternative. 

Using regular expressions, I identify reviews that mention certain products the owner might be interested in knowing more about and using spaCy, extract the sentences these mentions are in. Using TextBlob, the sentence or sentences talking about the product is given a sentiment score so that we can split up reviews that talk about the product positively and others that talk about it negatively. 

As next steps, I'm interested in trending out positive and negative reviews as some sort of analytics dashboard for the business user. Also, now that sentences that specifically talk about a product are extracted, I can take these sentences and pass them through some unsupervised model to see what common positive and negative things are being said about it.
