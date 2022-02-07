---
layout: post
title:  "Deploy and share your sentiment analysis app using Streamlit Sharing"
date:   2022-02-07 10:10:00 +1100
categories: streamlit
---
*This article was originally posted on [Medium](https://medium.com/@rtkilian/deploy-and-share-your-sentiment-analysis-app-using-streamlit-sharing-2ba3ca6a3ead).*

| ![Streamlit logo](/assets/streamlit.jpeg) | 
|:--:| 
| Source: [Streamlit](https://streamlit.io/) |

Imagine this, you’ve spent the last two weeks developing a killer sentiment analysis model on your local machine. You’ve experimented with five different models, tuned your hyperparameters to perfection and tested the performance on a range of scenarios.

But now a non-technical Product Manager wants to review your model. What do you do?

You could run a live demo? Your calendars don’t line up for weeks.
You could share your virtual environment? They don’t know how to code.
You could get a developer to deploy the model? Their task list is two weeks long.

Unfortunately, this is a scenario that many Data Scientists find themselves in. **But thanks to [Streamlit Sharing](https://streamlit.io/sharing) you can now deploy and share your model in less than 30-minutes without help from a developer.**

## The Goal of this Tutorial
In this tutorial, I will show you how to deploy a [sentiment analysis](https://en.wikipedia.org/wiki/Sentiment_analysis) model using Streamlit Sharing. A user will be able to enter their text and the model will classify it as ‘POSITIVE’ or ‘NEGATIVE’ sentiment, along with a confidence score.

To see a **live demo** of my application on Streamlit Sharing: [click here](https://share.streamlit.io/rtkilian/streamlit-huggingface/main/sentiment_analyser.py).
To view my **source code** on GitHub: [click here](https://github.com/rtkilian/streamlit-huggingface).

| ![Streamlit sentiment demo app](/assets/streamlit-app-demo.png) | 
|:--:| 
| A screenshot of the app you will build in this tutorial |

## Step 1: Installing Libraries
### Streamlit
[Streamlit](https://streamlit.io/) is an open-source Python library that makes it easy to create and share beautiful, custom web apps for machine learning and data science.

To install:
{% highlight python %}
pip install streamlit
{% endhighlight %}

You should also request an invitation for [**Streamlit Sharing**](https://streamlit.io/sharing) which allows you to deploy, manage, and share your apps with the world, directly from Streamlit.

It may take up to 2-days to receive your invitation. However, I received my invitation within 24-hours, even after requesting it on the weekend.
