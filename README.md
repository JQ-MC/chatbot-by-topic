# chatbot-by-topic
 Proposed chatbot based on topics>scenarios>frames

***

## Intro

The developed chatbot assists on providing a weather forecast, finding a restaurant
as well as finding the next bus and tram.

Each problem is modelled as a topic. For each topic, there are a set of different
scenarios that cover the possible variability of the topic, which at the same time,
require specific information to generate an appropriate response. In the case of the
weather, this would be a topic by itself, which has different scenarios (provide a
weather forecast, know if it will be sunny, snowy or how much will it rain), which at
the same time, require specific information (frames) such as place and time.

***

## Logic explanation and implementation
We have defined a set of classes that deal with our logic explanation of the problem:

* __Frames__ are used to find and store information required to answer questions
about a topic. An example is a weather forecast, where a place and a time is
required to give an answer. Each frame has a priority score used to decide the
order in which frames are asked first, an array of different formulations asking
the user for the relevant information and a parser for finding the relevant
information given a user input.
* __Scenarios__. The components of a scenario are a vocabulary, a predefined
question to ask the user, and an array of necessary information to make an
appropriate response. The vocabulary is a list of keywords which the chatbot
matches with the user input in order to guess the scenario. For a weather bot,
the vocabulary of one scenario might contain hot, cold, temperature while
another scenario’s vocabulary contains sun, sunny.
* __Topics__ contains a list of scenarios, a vocabulary, a database, a filter (required
frame), and a list of frames. Topic is also where the main logic functions are
implemented. The vocabulary is initiated as a list of generic keywords for a
specific topic. Then, the vocabularies of all its scenarios are concatenated to
the list, generating one main vocabulary that is used to guess the topic of the
user input. For our weather bot, the complete vocabulary contains words such
as weather, hot, and sun.
All information must be coded by hand.

***

## Chatbot procedures

Of course, we have also implemented the Chatbot in a class, which leads the
process of the chatbot. The performed steps are:
1. __Guess topic__. When the chatbot receives the initial input it matches it to the
keywords of the topic vocabularies by establishing a score that counts the
number of words in the input sentence that are present in each vocabularies’
topic. The topic that receives the highest score from these matches is
returned.
2. __Guess scenario__. By using the same technique explained above, it guesses
the possible case scenario among the defined scenarios of the guessed topic.
If it cannot guess the scenario, it will ask about possible scenarios from the
topic.
3. __Fill frames__. Then, the chatbot will fill any viable frames with the initial input. If
a frame is empty, the chatbot will ask additional questions until all frames are
filled. The information is parsed using regular expressions that are defined
when initialising the class.
4. __Respond__. Once the topic, scenario and frames are defined, a response is
provided. By looking at the database, we filter it according to the filter value
stored in the topic, and select the columns that provide information about the
needed_information defined in each scenario. This results in a filtered
database that is decoded using the function decoder that returns a natural
sentence.
5. __Loop__. This process is repeated until the user inputs the word STOP.
The chatbot tries to keep the initiative in the conversation by asking relevant
questions to the topic and scenario. It also handles missing knowledge if a topic can
not be guessed by the input of the user or the defined scenarios are not what the
user is looking for.

***

## Assumptions made
For the implementation we made the following assumptions:
* All relevant input data will be correctly spelled (ignoring uppercase).
* Time is only given as weekdays. We will not be considering time stamps or
dates in input nor time relations on the present (e.g. following week).
* The database containing relevant information will be static and fictional.
To evaluate the chatbot we used manually curated databases formatted as
dataframes, each containing a small set of data. Each topic was associated with a
single specific database. The implementation of the chatbot is modular enough that
expanding it with more topics would not change the logic behind the chatbot, and
would only require adding the additional information.

***

## Future work 
Some possible improvements include:
* Implementing functionality for accessing external databases. The ability to
access external databases would greatly increase the knowledge available to
the system, enabling it to give more and better responses. For example, if the
chatbot could access SMHI:s data it would be able to answer weather
questions for all of Sweden without the need of manually maintaining a
weather forecast.
* Enabling topics and scenarios to be created through machine learning. For
example, a model might be trained on transcripts of dialogues for the specific
subject.
* Refine keyword sets for topics and scenarios. A better selection of keywords
will make the chatbot more accurately guess specific topics and scenarios,
and a larger amount of keywords will increase the chatbot’s ability to identify
topics.
* Implementing functionality to understand misspellings and other linguistic
anomalies. At present, any input has to be correctly spelled but allowing minor
misspellings may improve user experience. A possible solution would be
pattern matching with the letters of keywords, but that could result in false
positives.
* Implementing functionality to handle times/dates. At present, only weekdays
are understood to be units of time but expanding this to dates and clock
strokes would greatly enhance functionality.
