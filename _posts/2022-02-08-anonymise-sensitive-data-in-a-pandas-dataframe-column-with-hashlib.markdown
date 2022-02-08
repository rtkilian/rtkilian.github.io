---
layout: post
title:  "Anonymise sensitive data in a pandas DataFrame column with hashlib"
date:   2022-02-08 18:00:00 +1100
categories: writing
---
*This article was originally posted on [Towards Data Science](https://towardsdatascience.com/anonymise-sensitive-data-in-a-pandas-dataframe-column-with-hashlib-8e7ef397d91f).*

| ![Hacker stock photo](https://images.unsplash.com/photo-1510511459019-5dda7724fd87?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070&q=80) | 
|:--:| 
| Photo by [Markus Spiske](https://unsplash.com/@markusspiske) on [Unsplash](https://unsplash.com) |

A common scenario encountered by Data Scientists is sharing data with others. But what should you do if that data contains personally identifiable information (PII) such as email addresses, customer IDs or phone numbers?

A simple solution is to remove these fields before sharing the data. However, your analysis may rely on having the PII data. For example, customer IDs in an e-commerce transactional dataset are necessary to know which customer bought which product.

Instead, you can anonymise the PII fields in your data using **hashing**.

## What is hashing?

Hashing is a one-way process of transforming a string of plaintext characters into a unique string of fixed length. The hashing process has two important characteristics:
1. It is very difficult to convert a hashed string into its original form
2. The same plaintext string will produce the same hashed output

For these reasons, developers will store your hashed password in the website’s database.

## A simple example using hashlib
[haslib](https://docs.python.org/3/library/hashlib.html) is a built-in module in Python that contains many popular hash algorithms. In our tutorial, we’re going to be using SHA-256 which is part of the [SHA-2](https://en.wikipedia.org/wiki/SHA-2) (**S**ecure **H**ash **A**lgorithm **2**) family of algorithms.

Before we can convert our string, in this example an email address, to a hashed value, we must first convert it into bytes using UTF-8 encoding:

{% highlight python %}
import hashlib
# Encode our string using UTF-8 default 
stringToHash = 'example@email.com'.encode()
{% endhighlight %}

We can now hash it using SHA-256:

{% highlight python %}
# Hash using SHA-256 and print
print('Email (SHA-256): ', hashlib.sha256(stringToHash).hexdigest())
{% endhighlight %}

Output:

{% highlight Bash %}
Email (SHA-256): 36e96648c5410d00a7da7206c01237139f950bed21d8c729aae019dbe07964e7
{% endhighlight %}

That’s it! Our fake email address has been successfully hashed.

## A complete example using pandas and hashlib

Now that we can apply hashlib to a single string, it’s fairly straightforward to scale this example to a pandas DataFrame. We’re going to use credit card customer data, available on [Kaggle](https://www.kaggle.com/sakshigoyal7/credit-card-customers), which was originally made available by [Analyttica TreasureHunt LEAPS](https://leaps.analyttica.com/).

**Scenario**: you need to share a list of credit card customers. You want to retain the field ‘CLIENTNUM’ as a customer can have multiple credit cards and you want to be able to uniquely identify them.

{% highlight python %}
import pandas as pd
# Read only select columns using pandas
df = pd.read_csv('data/BankChurners.csv', usecols=['CLIENTNUM', 'Customer_Age', 'Gender', 'Attrition_Flag', 'Total_Trans_Amt'])
df.head()
{% endhighlight %}

| ![pandas dataframe to anonymise](/assets/dataframe-pandas.png) | 
|:--:| 
| pandas DataFrame to anonymise |

After converting our ‘CLIENTNUM’ column to a string data type, we can then use pandas `.apply()` to hash all the strings in the column:

{% highlight python %}
# Convert column to string
df['CLIENTNUM'] = df['CLIENTNUM'].astype(str)
# Apply hashing function to the column
df['CLIENTNUM_HASH'] = df['CLIENTNUM'].apply(
    lambda x: 
        hashlib.sha256(x.encode()).hexdigest()
)
{% endhighlight %}

| ![Output of pandas anonymisation](/assets/dataframe-pandas-anonymised.png) | 
|:--:| 
| pandas DataFrame with anonymised column |

## Conclusion
After completing this tutorial you should have a basic understanding of what a hash algorithm is. We saw how to use **hashlib** to hash a single string and how this can be applied to a pandas DataFrame column to anonymise sensitive information.

Do you have any questions? [**Tweet**](https://twitter.com/rtkilian) me or add me on [**LinkedIn**](https://www.linkedin.com/in/rtkilian/).

You can find all the code used in this post on [**GitHub**](https://github.com/rtkilian/data-science-blogging/blob/main/hashlib_pandas.ipynb).