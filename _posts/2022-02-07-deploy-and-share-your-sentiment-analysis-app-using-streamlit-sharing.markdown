---
layout: post
title:  "Deploy and share your sentiment analysis app using Streamlit Sharing"
date:   2022-02-07 10:10:00 +1100
categories: writing
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

### 🤗 Transformers by Hugging Face
[🤗 Transformers](https://huggingface.co/transformers/) provides general-purpose architectures (BERT, GPT-2, etc.) for Natural Language Understanding (NLU) and Natural Language Generation (NLG) with over 32+ pre-trained models in 100+ languages. In only a few lines of code, you can have a state of the art sentiment analysis model downloaded and ready to go.

First, you need to install one of, or both, TensorFlow 2.0 and PyTorch. Please refer to the [TensorFlow installation](https://www.tensorflow.org/install/pip#tensorflow-2.0-rc-is-available) page and [PyTorch](https://pytorch.org/get-started/locally/#start-locally) installation page regarding the specific install command for your platform.

When installed, run the following command:
{% highlight python %}
pip install transformers
{% endhighlight %}

## Step 2: Project Set-Up
All your code will need to be contained within a Python file (.py). I personally use Visual Studio Code that comes installed in Anaconda, however, you can also download your Jupyter Notebook as a .py file.

I’ve called my file **sentiment_analyser.py**.

Now, import the libraries you downloaded in Step 1:
{% highlight python %}
import streamlit as st
from transformers import pipeline
{% endhighlight %}

In the same directory, create a text file called **requirements.txt** and copy the version of the libraries you installed in Step 1. When it comes time to deploy your app, Streamlit Sharing will install the libraries in this file.
{% highlight python %}
streamlit==0.86
transformers==4.8.1
torch==1.9.0
{% endhighlight %}

At this point, you should also create a **GitHub repo**. If you’ve never used GitHub before, I recommend checking out their [documentation](https://docs.github.com/en/get-started/quickstart/create-a-repo) to set up your first repo.

## Step 3: Create the User Interface Using Streamlit
Finally, something fun! We will now start by creating the structure of your Streamlit web application.

First, let’s add a title and some descriptive text by adding the following code to your **sentiment_analyser.py** file:
{% highlight python %}
st.title('Sentiment Analyser App')
st.write('Welcome to my sentiment analysis app!')
{% endhighlight %}

To run your app locally (before we deploy it), navigate to your development directory in your terminal and run the following command:
{% highlight python %}
streamlit run sentiment_analyser.py
{% endhighlight %}

If all goes to plan, you should have your Streamlit app popping up in a new browser window.

| ![Streamlit first app](/assets/streamlit-first-app.png) | 
|:--:| 
| Your first Streamlit application |

We will now build a simple form that contains a text area for user input and a submit button. Our ‘*user_input*’ variable will be passed into our sentiment analysis model in Step 4.

{% highlight python %}
form = st.form(key='sentiment-form')
user_input = form.text_area('Enter your text')
submit = form.form_submit_button('Submit')
{% endhighlight %}

After refreshing your application, it should now look like this:

| ![Streamlit first app refreshed](/assets/streamlit-first-app-refreshed.png) | 
|:--:| 
| Streamlit application with user input form |

## Step 4: Classify User Input Using 🤗 Transformers
Hugging Face has an [excellent tutorial](https://huggingface.co/course/chapter1/3?fw=pt) on their 🤗 Transformer library. For this tutorial, we will be using the pipeline object that will connect the default sentiment analysis model with its necessary preprocessing and postprocessing steps.

If you were to run the following code in a Jupyter Notebook or .py file:
{% highlight python %}
classifier = pipeline("sentiment-analysis")
classifier("I've been waiting for a HuggingFace course my whole life.")
{% endhighlight %}

It would output the classified sentiment label (‘POSITIVE or ‘NEGATIVE’), along with a confidence score.

{% highlight python %}
[{'label': 'POSITIVE', 'score': 0.9598047137260437}]
{% endhighlight %}

## Step 5: Linking Streamlit and the Sentiment Analysis Model
By now, you should see that we already have the basic building blocks of our application. To finish our app, let’s classify our ‘user_input’ variable using the 🤗 Transformer library to give our sentiment label and confidence score:
{% highlight python %}
classifier = pipeline("sentiment-analysis")    
result = classifier(user_input)[0]    
label = result['label']    
score = result['score']
{% endhighlight %}

However, we only want to run out model after the user submitted the form. We can also display the result in a green banner (`st.success`) for ‘POSITIVE’ sentiment and in a red banner (`st.error`) for ‘NEGATIVE’ sentiment:
{% highlight python %}
if submit:
    classifier = pipeline("sentiment-analysis")
    result = classifier(user_input)[0]
    label = result['label']
    score = result['score']
if label == 'POSITIVE':
        st.success(f'{label} sentiment (score: {score})')
    else:
        st.error(f'{label} sentiment (score: {score})')
{% endhighlight %}

Once complete, **push your changes to your GitHub repo**.

If you want to see all the code in my **sentiment_analyser.py** file, please check out my [GitHub repo](https://github.com/rtkilian/streamlit-huggingface/blob/main/sentiment_analyser.py).

## Step 6: Deploy Your App to Streamlit Sharing
You should now have a functioning application on your local machine. But we want to be able to share our application just by sharing a single URL using Streamlit Sharing.

Once you have received your invitation to register for Streamlit Sharing, you can create a ‘New app’ on your app dashboard. Enter your GitHub username/repo, the name of your branch and the name of the Python file that contains your code. It should look something like this:

| ![Streamlit sharing configuration](/assets/streamlit-sharing-config.png) | 
|:--:| 
| Streamlit Sharing configuration |

Once you hit ‘Deploy!’, Streamlit will look for your requirements.txt file in your repo and install all the required libraries. After it has finished installing, you should receive a message that your application has been deployed successfully and you should be provided with a URL. For example, you can find my app [here](https://share.streamlit.io/rtkilian/streamlit-huggingface/main/sentiment_analyser.py).

Congratulations! You have just deployed your sentiment analysis application that you can now share.

## Conclusion
In this tutorial, I have shown you how you can build a simple interface using Streamlit to capture user input, classify the sentiment using Hugging Face’s 🤗 Transformer library, and deploy your application using Streamlit Sharing. We have only scratched the surface of what is possible using Streamlit (and Hugging Face). But now you should have the basic tools to share your prototypes with your colleagues, friends and family.

I encourage you to experiment. How can you make this app faster? What other NLP models can you deploy?

Share your apps with me on [**Twitter**](https://twitter.com/rtkilian) or add me on [**LinkedIn**](https://www.linkedin.com/in/rtkilian/). More than happy to answer any questions you may have!