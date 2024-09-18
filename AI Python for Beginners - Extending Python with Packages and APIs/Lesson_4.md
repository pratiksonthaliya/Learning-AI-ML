# Lesson 4: Installing packages

In this lesson, you will learn how to install third-party packages using a command called `pip`.

Once you have installed a package, you can use functions from the package by importing them using the `import` command.

## Installing packages using `pip`

Run the cell below to install the `bs4` package:


```python
!pip install bs4
```

    Collecting bs4
      Downloading bs4-0.0.2-py2.py3-none-any.whl.metadata (411 bytes)
    Requirement already satisfied: beautifulsoup4 in /usr/local/lib/python3.9/site-packages (from bs4) (4.12.3)
    Requirement already satisfied: soupsieve>1.2 in /usr/local/lib/python3.9/site-packages (from beautifulsoup4->bs4) (2.5)
    Downloading bs4-0.0.2-py2.py3-none-any.whl (1.2 kB)
    Installing collected packages: bs4
    Successfully installed bs4-0.0.2
    
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m A new release of pip is available: [0m[31;49m24.0[0m[39;49m -> [0m[32;49m24.2[0m
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m To update, run: [0m[32;49mpip install --upgrade pip[0m


**Note:** You can safely ignore any warnings you see about upgrading pip.

bs4 is short for **Beautiful Soup 4**. You can check out the [Beautiful Soup documentation](https://pypi.org/project/beautifulsoup4/) if you want to learn more about the package, but it gives you tools to interpret HTML webpages inside Python programs.

Now that you have installed the bs4 package, you can use it in your programs!

First, you need to import the `BeautifulSoup` function you'll use from the `bs4` package, as well as some other packages:


```python
from bs4 import BeautifulSoup

import requests # let's you download webpages into python
from helper_functions import * 
from IPython.display import HTML, display
```

## Get data from the web

In this section, you'll "scrape", or download HTML data from a website, in this case from a [Batch newsletter](https://www.deeplearning.ai/the-batch/) published by DeepLearning.AI.

You'll use the `requests` Python package to download the data from the webpage and make it available in your program:


```python
# The url from one of the Batch's newsletter
url = 'https://www.deeplearning.ai/the-batch/the-world-needs-more-intelligence/'

# Getting the content from the webpage's contents
response = requests.get(url)

# Print the response from the requests
print(response)
```

    <Response [200]>


**Note:** The `<Response [200]>` you see is an indication from the requests library that your HTTP request was successful. You can ask the chatbot for details about other codes you might see.

Now that you have downloaded the content from the website, you can display it in the notebook using the following code:


```python
HTML(f'<iframe src={url} width="60%" height="400"></iframe>')
```

    /usr/local/lib/python3.9/site-packages/IPython/core/display.py:431: UserWarning: Consider using IPython.display.IFrame instead
      warnings.warn("Consider using IPython.display.IFrame instead")





<iframe src=https://www.deeplearning.ai/the-batch/the-world-needs-more-intelligence/ width="60%" height="400"></iframe>



Next, you'll use Beautiful Soup to extract all the text paragraphs from the HTML structure that you retrieved, and save it as a single string. Here is the code to do this:


```python
# Using beautifulsoup to extract the text
soup = BeautifulSoup(response.text, 'html.parser')
# Find all the text in paragraph elements on the webpage
all_text = soup.find_all('p')

# Create an empty string to store the extracted text
combined_text = ""

# Iterate over 'all_text' and add to the combined_text string
for text in all_text:
    combined_text = combined_text + "\n" + text.get_text()

# Print the final combined text
print(combined_text)
```

    
    🌟 New Course! Enroll in  Multimodal RAG: Chat with Videos
    Dear friends,
    Last year, a number of large businesses and individuals went to the media and governments and pushed the message that AI is scary, impossible to control, and might even lead to human extinction. Unfortunately they succeeded: Now many people think AI is scary. But when I speak with regulators, media, and private citizens, I like to bring the issue of whether AI is beneficial or harmful back to a very basic question: Are we better off with more, or less, intelligence in the world? 
    Intelligence is the ability to apply skills and knowledge to make good decisions. Yes, intelligence can be used for nefarious purposes. But over many centuries, a major driver of civilization's progress has been people getting smarter and more educated. Until now, human intelligence has been the primary form of intelligence available. But with artificial intelligence, we have the opportunity to bring much more intelligence into the world. I discussed this opportunity in a recent interview (paywalled) with Financial Times reporter Ryan McMorrow.
    Historically, intelligence has been very expensive to acquire. It costs a lot to feed, raise, and train a broadly knowledgeable and experienced human being! That's why it’s so expensive to hire intelligence, such as a highly skilled doctor to examine and advise you on a medical condition, or a patient tutor who can understand your child and gently coach them where they need help. But with artificial intelligence, we have the potential to make intelligence cheap for everyone, so you no longer have to worry about a huge bill for seeing a doctor or educating your child. 
    For society's biggest problems, such as climate change, intelligence — including artificial intelligence — also has a significant role to play. While having more intelligence in the world isn't the only thing (there are also nuances such as how to share the wealth it creates, how it will affect jobs, and how to keep it from being used for evil purposes), I believe we are much better off as a society with more intelligence, be it human or artificial intelligence. 
    In my recent talk at TED AI (you can watch the 12-minute presentation here), I touched on why I'm excited about AI and why I think many of the anxieties about it are misplaced. If you speak with someone who’s worried about AI, please forward the talk to them to see if it helps to reassure them. Or ask if they fundamentally believe we want more intelligence in the world. I find that answering this question can be a useful North Star for how we approach AI.
    Keep learning!
    Andrew
    P.S. Check out our new short course on “Building Applications with Vector Databases,” taught by Pinecone’s Tim Tully! Vector databases (DBs) are commonly associated with retrieval augmented generation (RAG) but actually have many uses in AI applications. In this course, you’ll learn about (i) a basic semantic search app that uses a vector DB to find similar documents, (ii) a RAG application querying datasets it was not trained on, (iii) recommender systems that combine semantic search and RAG, (iv) hybrid search, which lets you work with dense and sparse vectors simultaneously, (v) anomaly detection applied to network logs, and (vi) an image-similarity application with a fun example that determines which parent a child resembles more. Come learn how you can use vector DBs to build many different types of applications! Enroll here
    Stay updated with weekly AI News and Insights delivered to your inbox


For more details about how this code works, you can ask the chatbot:

<p style="background-color:#F5C780; padding:15px"> 🤖 <b>Use the Chatbot</b>:
<br><br>
What is the following code doing?
<br><br>
soup = BeautifulSoup(response.text, 'html.parser')<br>
all_text = soup.find_all('p')
</p>

## Extracting information from scraped website data using LLMs

You can pass the text you just extracted from the Batch newsletter website to an LLM and ask it to extract the most relevant information for you.

Start by writing the prompt and passing in the text you extracted:


```python
prompt = f"""Extract the key bullet points from the following text.

Text:
{combined_text}
"""
```

Then pass the prompt to the LLM:


```python

print_llm_response(prompt)
```

    - New course: "Multimodal RAG: Chat with Videos" available for enrollment.
    - AI concerns: Many perceive AI as scary and uncontrollable due to media influence.
    - Key question: Are we better off with more or less intelligence in the world?
    - Intelligence defined as the ability to make good decisions using skills and knowledge.
    - AI can make intelligence more accessible and affordable for everyone.
    - AI's potential role in addressing major societal issues like climate change.
    - Emphasis on the benefits of increased intelligence, both human and artificial.
    - Recent TED AI talk discusses excitement about AI and addresses common anxieties.
    - New short course on "Building Applications with Vector Databases" offered.
    - Course covers various applications of vector databases in AI, including semantic search and anomaly detection.


## One more example of installing packages

Throughout the courses so far, you've imported helper functions from a file called `helper_functions.py` using commands like `from helper_functions import get_llm_response`.

The DeepLearning.AI team has created a third-party package called `aisetup` that you can use to access the helper functions from the course in your own code outside of this learning platform.

To install it, run the following command:


```python
!pip install aisetup
```

    Requirement already satisfied: aisetup in /usr/local/lib/python3.9/site-packages (0.1.4)
    Requirement already satisfied: folium<0.18.0,>=0.17.0 in /usr/local/lib/python3.9/site-packages (from aisetup) (0.17.0)
    Requirement already satisfied: ipython==8.18.1 in /usr/local/lib/python3.9/site-packages (from aisetup) (8.18.1)
    Requirement already satisfied: ipywidgets<9.0.0,>=8.1.3 in /usr/local/lib/python3.9/site-packages (from aisetup) (8.1.5)
    Requirement already satisfied: matplotlib<4.0.0,>=3.9.2 in /usr/local/lib/python3.9/site-packages (from aisetup) (3.9.2)
    Requirement already satisfied: numpy==2.0.1 in /usr/local/lib/python3.9/site-packages (from aisetup) (2.0.1)
    Requirement already satisfied: openai<2.0.0,>=1.42.0 in /usr/local/lib/python3.9/site-packages (from aisetup) (1.43.0)
    Requirement already satisfied: python-dotenv<2.0.0,>=1.0.1 in /usr/local/lib/python3.9/site-packages (from aisetup) (1.0.1)
    Requirement already satisfied: requests<3.0.0,>=2.32.3 in /usr/local/lib/python3.9/site-packages (from aisetup) (2.32.3)
    Requirement already satisfied: decorator in /usr/local/lib/python3.9/site-packages (from ipython==8.18.1->aisetup) (5.1.1)
    Requirement already satisfied: jedi>=0.16 in /usr/local/lib/python3.9/site-packages (from ipython==8.18.1->aisetup) (0.19.1)
    Requirement already satisfied: matplotlib-inline in /usr/local/lib/python3.9/site-packages (from ipython==8.18.1->aisetup) (0.1.7)
    Requirement already satisfied: prompt-toolkit<3.1.0,>=3.0.41 in /usr/local/lib/python3.9/site-packages (from ipython==8.18.1->aisetup) (3.0.43)
    Requirement already satisfied: pygments>=2.4.0 in /usr/local/lib/python3.9/site-packages (from ipython==8.18.1->aisetup) (2.17.2)
    Requirement already satisfied: stack-data in /usr/local/lib/python3.9/site-packages (from ipython==8.18.1->aisetup) (0.6.3)
    Requirement already satisfied: traitlets>=5 in /usr/local/lib/python3.9/site-packages (from ipython==8.18.1->aisetup) (5.9.0)
    Requirement already satisfied: typing-extensions in /usr/local/lib/python3.9/site-packages (from ipython==8.18.1->aisetup) (4.11.0)
    Requirement already satisfied: exceptiongroup in /usr/local/lib/python3.9/site-packages (from ipython==8.18.1->aisetup) (1.2.2)
    Requirement already satisfied: pexpect>4.3 in /usr/local/lib/python3.9/site-packages (from ipython==8.18.1->aisetup) (4.9.0)
    Requirement already satisfied: branca>=0.6.0 in /usr/local/lib/python3.9/site-packages (from folium<0.18.0,>=0.17.0->aisetup) (0.7.2)
    Requirement already satisfied: jinja2>=2.9 in /usr/local/lib/python3.9/site-packages (from folium<0.18.0,>=0.17.0->aisetup) (3.1.4)
    Requirement already satisfied: xyzservices in /usr/local/lib/python3.9/site-packages (from folium<0.18.0,>=0.17.0->aisetup) (2024.9.0)
    Requirement already satisfied: comm>=0.1.3 in /usr/local/lib/python3.9/site-packages (from ipywidgets<9.0.0,>=8.1.3->aisetup) (0.2.2)
    Requirement already satisfied: widgetsnbextension~=4.0.12 in /usr/local/lib/python3.9/site-packages (from ipywidgets<9.0.0,>=8.1.3->aisetup) (4.0.13)
    Requirement already satisfied: jupyterlab-widgets~=3.0.12 in /usr/local/lib/python3.9/site-packages (from ipywidgets<9.0.0,>=8.1.3->aisetup) (3.0.13)
    Requirement already satisfied: contourpy>=1.0.1 in /usr/local/lib/python3.9/site-packages (from matplotlib<4.0.0,>=3.9.2->aisetup) (1.3.0)
    Requirement already satisfied: cycler>=0.10 in /usr/local/lib/python3.9/site-packages (from matplotlib<4.0.0,>=3.9.2->aisetup) (0.12.1)
    Requirement already satisfied: fonttools>=4.22.0 in /usr/local/lib/python3.9/site-packages (from matplotlib<4.0.0,>=3.9.2->aisetup) (4.53.1)
    Requirement already satisfied: kiwisolver>=1.3.1 in /usr/local/lib/python3.9/site-packages (from matplotlib<4.0.0,>=3.9.2->aisetup) (1.4.6)
    Requirement already satisfied: packaging>=20.0 in /usr/local/lib/python3.9/site-packages (from matplotlib<4.0.0,>=3.9.2->aisetup) (23.2)
    Requirement already satisfied: pillow>=8 in /usr/local/lib/python3.9/site-packages (from matplotlib<4.0.0,>=3.9.2->aisetup) (10.4.0)
    Requirement already satisfied: pyparsing>=2.3.1 in /usr/local/lib/python3.9/site-packages (from matplotlib<4.0.0,>=3.9.2->aisetup) (3.1.4)
    Requirement already satisfied: python-dateutil>=2.7 in /usr/local/lib/python3.9/site-packages (from matplotlib<4.0.0,>=3.9.2->aisetup) (2.9.0.post0)
    Requirement already satisfied: importlib-resources>=3.2.0 in /usr/local/lib/python3.9/site-packages (from matplotlib<4.0.0,>=3.9.2->aisetup) (6.4.4)
    Requirement already satisfied: anyio<5,>=3.5.0 in /usr/local/lib/python3.9/site-packages (from openai<2.0.0,>=1.42.0->aisetup) (3.7.1)
    Requirement already satisfied: distro<2,>=1.7.0 in /usr/local/lib/python3.9/site-packages (from openai<2.0.0,>=1.42.0->aisetup) (1.9.0)
    Requirement already satisfied: httpx<1,>=0.23.0 in /usr/local/lib/python3.9/site-packages (from openai<2.0.0,>=1.42.0->aisetup) (0.27.0)
    Requirement already satisfied: jiter<1,>=0.4.0 in /usr/local/lib/python3.9/site-packages (from openai<2.0.0,>=1.42.0->aisetup) (0.5.0)
    Requirement already satisfied: pydantic<3,>=1.9.0 in /usr/local/lib/python3.9/site-packages (from openai<2.0.0,>=1.42.0->aisetup) (2.8.2)
    Requirement already satisfied: sniffio in /usr/local/lib/python3.9/site-packages (from openai<2.0.0,>=1.42.0->aisetup) (1.3.1)
    Requirement already satisfied: tqdm>4 in /usr/local/lib/python3.9/site-packages (from openai<2.0.0,>=1.42.0->aisetup) (4.66.1)
    Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.9/site-packages (from requests<3.0.0,>=2.32.3->aisetup) (3.3.2)
    Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.9/site-packages (from requests<3.0.0,>=2.32.3->aisetup) (3.7)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.9/site-packages (from requests<3.0.0,>=2.32.3->aisetup) (1.26.18)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.9/site-packages (from requests<3.0.0,>=2.32.3->aisetup) (2024.2.2)
    Requirement already satisfied: httpcore==1.* in /usr/local/lib/python3.9/site-packages (from httpx<1,>=0.23.0->openai<2.0.0,>=1.42.0->aisetup) (1.0.5)
    Requirement already satisfied: h11<0.15,>=0.13 in /usr/local/lib/python3.9/site-packages (from httpcore==1.*->httpx<1,>=0.23.0->openai<2.0.0,>=1.42.0->aisetup) (0.14.0)
    Requirement already satisfied: zipp>=3.1.0 in /usr/local/lib/python3.9/site-packages (from importlib-resources>=3.2.0->matplotlib<4.0.0,>=3.9.2->aisetup) (3.19.2)
    Requirement already satisfied: parso<0.9.0,>=0.8.3 in /usr/local/lib/python3.9/site-packages (from jedi>=0.16->ipython==8.18.1->aisetup) (0.8.4)
    Requirement already satisfied: MarkupSafe>=2.0 in /usr/local/lib/python3.9/site-packages (from jinja2>=2.9->folium<0.18.0,>=0.17.0->aisetup) (2.1.5)
    Requirement already satisfied: ptyprocess>=0.5 in /usr/local/lib/python3.9/site-packages (from pexpect>4.3->ipython==8.18.1->aisetup) (0.7.0)
    Requirement already satisfied: wcwidth in /usr/local/lib/python3.9/site-packages (from prompt-toolkit<3.1.0,>=3.0.41->ipython==8.18.1->aisetup) (0.2.13)
    Requirement already satisfied: annotated-types>=0.4.0 in /usr/local/lib/python3.9/site-packages (from pydantic<3,>=1.9.0->openai<2.0.0,>=1.42.0->aisetup) (0.7.0)
    Requirement already satisfied: pydantic-core==2.20.1 in /usr/local/lib/python3.9/site-packages (from pydantic<3,>=1.9.0->openai<2.0.0,>=1.42.0->aisetup) (2.20.1)
    Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.9/site-packages (from python-dateutil>=2.7->matplotlib<4.0.0,>=3.9.2->aisetup) (1.16.0)
    Requirement already satisfied: executing>=1.2.0 in /usr/local/lib/python3.9/site-packages (from stack-data->ipython==8.18.1->aisetup) (2.0.1)
    Requirement already satisfied: asttokens>=2.1.0 in /usr/local/lib/python3.9/site-packages (from stack-data->ipython==8.18.1->aisetup) (2.4.1)
    Requirement already satisfied: pure-eval in /usr/local/lib/python3.9/site-packages (from stack-data->ipython==8.18.1->aisetup) (0.2.2)
    
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m A new release of pip is available: [0m[31;49m24.0[0m[39;49m -> [0m[32;49m24.2[0m
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m To update, run: [0m[32;49mpip install --upgrade pip[0m


Now the package is installed, you can import helper functions from it using the `import` command. For example, if you want to import `get_llm_response`, you now run this code:


```python
from aisetup import get_llm_response
```


```python
response = get_llm_response("Why is the programming language called Python?")

# Print LLMs response
print(response)
```

    Python is named after the British comedy television show "Monty Python's Flying Circus," which its creator, Guido van Rossum, enjoyed. The name reflects the language's emphasis on fun and ease of use.


## Extra practice

Try the following exercises to test what you have learned. If you get stuck, as the chatbot for help!

### Exercise 1

Modify the following code to answer the following question:
- Who built the new short course mentioned in the letter?


```python
# Modify the prompt
prompt = f"""Who build the new course mentioned in below text:
Text:
{combined_text}
"""
print_llm_response(prompt)
```

    The new course "Multimodal RAG: Chat with Videos" was built by Andrew, as indicated in the text.


### Exercise 2

Use the `celsius_to_fahrenheit` function in the `aisetup` package to calculate the Fahrenheit equivalent of 0 degrees Celsius.

You'll need to complete the import statement and the calculation.


```python

# Complete the import statement
from aisetup import celsius_to_fahrenheit

# Complete the calculation
zero_celsius_in_fahrenheit = celsius_to_fahrenheit(0)
print (zero_celsius_in_fahrenheit)
```

    0°C is equivalent to 32.00°F
    None


### Challenge exercise!

Write code that uses the `bs4` package to create a string that contains the **title element from the Batch newsletter**. This is the text that starts "The World Needs More Intelligence".

**Hint 1:** Titles on webpages are often header elements, with tags like `<h1>` or `<h2>`.
**Hint 2:** Ask the chatbot for help, using the code you have already written as a starting point.


```python
# Your code here

title = soup.find('h1').get_text()

print(title)
```

    The World Needs More Intelligence Human intelligence is expensive, artificial intelligence is cheap. To solve big problems like climate change, it makes sense to double down on AI.

