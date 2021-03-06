# .FLOW
Part of the SEED token project. This is a sneak preview - there is more to come.
See [the Wiki](https://github.com/SeedVault/flow/wiki) for more information.

## About the SEED Token Project
SEED democratizes AI by offering an open and independent alternative to the monopolies of a few large corporations that currently control conversational user interfaces (CUIs) and AI technologies. SEED's licensed, monetized open-source platform for bots on blockchain supports collaboration and creative compensation that will exceed the proprietary deployments from industry giants. We are also giving users back control of their personal data. Find out more about the SEED Token project at [seedtoken.io](https://seedtoken.io). See the Connect section at the end for contact info.

## Sneak Preview
From day one, Botanic (through Seed Vault Ltd.) will ‘seed’ the SEED Network with free to use IP and work to compensate existing open source CUI communities to do the same in order to grow the platform’s usefulness.
This repository is just the beginning of a process, in which we will open source more parts of the otherwise proprietary bot framework/CUI toolkit and many other bot and AI components. 

## What is .Flow?
- The .FLOW project is designed to set a unifying standard for bot volleys that might be written in several different languages

## Why .Flow?
- to design and build how a conversation to be between user and bot, is built using node and path as building blocks, where a node represent either a bot message or user input, path represent a possible way from current node to a future node. 


## Why should one care about it?
- any one who wishes to come up with a bot, which has some task in it, knows how to execute it in a conversation model, that person can utilize this in representing the conversation in a flow.


## Who builds it?
- any one who is interested in building a bot with some purpose in it, such person can be called a bot developer


## Does a bot developer need to be technically qualified?
- not really, it completely depends on the kind of bot the bot developer wanted to create


## How to build one?
- can be built using author tool, a web client which has beautiful UX required for building a bot
- since it is in json, can write by hand directly in json without any other tool, after writing just one need to validate if it is a valid json or not


## How is it represented?
- is represented in the form of json


## Who consumes it?
- a flow engine, which usually can be built using any language of choice, but as of now Botanic has chosen Python and PHP in writing it.


## What does it give?
- a response that need to be shown to the end user, which is set by the bot developer while creating the flow


## What does it expect to continue further?
- user input, once it is received, it will processes it, after understanding the user input, it decides what need to be done next


# Documentation

## .Flow v2.0 - Introduction

This document is a draft specification for a new .Flow specification standard version 2.0
.Flow is a domain-specific language to build conversational bots executed by BBot bot engine. 
This document explains in detail both the syntax and semantics of .Flow and has a full reference for all operations and functions to be supported by BBot.

##### Changes from previous version
.Flow 2.0 is a full rewrite of its previous version. This new version is extensible, powerful and highly flexible. 
It shifted from concrete definitions to an abstract syntax tree (AST) where humans are not expected to develop a bot directly in .Flow but let a compiler generate the proper .Flow code from an human friendly concrete syntax implementation.

These are some of the improvements:
- Discarded concrete syntax structure in favor of an AST extensible conditions and responses definition. On this new version the conditions to follow a path can be built with multiple nested functions and operators to execute all kind of instructions including several ways to detect user intents from simple string manipulation and regular expressions to  pattern rule based or machine learning based back-ends like ChatScript, Microsoft's Luis, AIML, Google's DialogFlow, etc.
- Added an isolated high-level python-like condition criteria expression provided by the template engine Jinja2 to ease high-level language implementation compilers complexity.
- Added hybrid AST/concrete high-level language condition criteria expression to ease coding GUI tools complexity.  
- Added entities detection from machine learning functions and data capturing from pattern based functions.
- Added triggered paths feature for non-verbal signals like sensor data from the user's device, the internal scheduler or external services like weather report.
- Discarded node types in favor of generic nodes which can do anything. Each node can run several functions as response.
- New functions to import and call code from other bots. This new set of functions allows bot developers to share features from their bots to others. Also, the platform running the bot engine could make available its flow code to the bot developers for different services and useful reusable flow code. Provides a way to add *quibbles* to the bot.
- New API functions to get external information like weather report.
- Added a scheduler to run flows every certain length of time and with the possibility to run in background mode.
- Added exceptions feature to handle the flow when there is an issue like ambiguous location provided by the user.
- Added template engine based on Jinja2 for output formatting and interpolation on any string. All the .Flow functions have access to the template engine.
- Added functions for stemming, lemmatization and canonicalization in order to provide a proper output to the user.
- More awareness for the bot, handling sensors data from cellphone and IoT devices linked to the user through the channel.
- Added authorization feature for flows with restricted access.
- Switched structure of conditions and responses order. Now paths start with conditions then response is executed. This is easier to match with other bot languages.
- Added recursion support in order to allow AIML srai tag feature. 
- FormId is deprecated in favor of building the feature programming it with .Flow functions nodeInput and nodeOutput.

##### To-Dos:
- Add return and exception descriptions of each function.
- Add more intent detection functions from libraries and cloud based services.
- Add more functions from AIMLv2 and ChatScript.
- Add more features from WordNet and other libs?
- Add more API web services
 


## Root object

This is the root object of the bot flowchart. It contains data about the bot flow.
```
{
   "id": "73324811db2671478394690a75ab",
   "name": "Main bot flow",
   "nodes": [],
   "entities": [],
   "utterances": []
}
```
- id: Unique flow id
- name: Friendly descriptive flow name
- nodes: List of volleys nodes which defines the bot flowchart
- entities: Defines entities. See entities section.
- utterances: Defines utterances. See utterances section.


## Node object

A node object contains a group of paths that the conversation bot can follow based on the conditionals defined on each path. This sets the conversation context.
```
{
    "id": "ec465bade6ba2959f0fee3bae6a",
    "name": "Welcome message",
    "global": true,
    "paths": []
}
```
(@TODO remove global attr for a conventional id = 'globalNode'?)
- id: Unique node id
- name: Friendly descriptive name
- paths: List of paths to be tested first on this node. It can be an user intent test if intent match functions are used in the conditions attribute, or any criteria test. If no match is found here, the bot engine will test the criteria conditions on volley with attribute "global" set true. That volley will contain all paths which the bot will always be ready to follow if none of the current volley paths match.
-  (@TODO When the user is in a volley context and matches a global criteria the engine will stack conversations, follow matched path and then try to go back to previous conversation)

## Path object
```
This object defines the volley conditional/response.

{
	"id": "76c929b4c25096ba9ec29c3ce2c4c38496",
    "name": "Conditions for welcome message",
    "forceRun": true,
	"permissions": []
    "conditions": {},
    "responses": [],
    "exceptions": []
}
```
- id: (string) Unique path id
- name: (string) Friendly descriptive name
- forceRun: (boolean. optional. Default FALSE) If it is set on TRUE and if condition matches, this volley response will run even if there are other higher priority path being matched. (@TODO when more than one intent matches and at least one of them has forceRun TRUE and both have context volleys… engine should stack conversation topics… start with one and when finished, start with the other)
- permissions: (object. optional) Defines which role have access to this node and subsequent child nodes
- conditions: (object or string. optional. default is TRUE) Condition criteria object or string to be evaluated in order to execute the responses
- responses: (array of objects) List of responses to be executed when conditions evaluates as true
- exceptions: (object. optional) List of expected exceptions might happen and how to handle them


# Conditions criteria object

Conditions is a list of nested objects defining a criteria using .Flow functions and operators. If a condition evaluates to TRUE, the path will be matched and bot engine will stop looking for more path criteria (except if there are other nodes with forceRun equals to true) and run the responses on the node.

The result should be TRUE or FALSE. In the case of having functions returning other value type than boolean, the bot developer needs to add an operator to resolve it to true or false (testing if value  is higher than/equal to x). 
Proper way to handle intent scoring functions is to set an argument forcing them to return boolean based on a specified threshold (can be an additional argument, or use default value). In this case the engine will be in charge of group all matching paths which includes intent scoring responses and take the first highest score as match if it is above the defined bot threshold.

Arguments can be nested objects with more functions inside:
```
{
    "$function1": [
        {
            "$function2": [
                value1,
                "value2 {{interpolated | filter}}"
            ]
        },
        {
            "$function3": [
                value3,
                value4
            ]
        }
    ]
}
```
 

Example: 
`{"$eq": ["abc", "abc"]}` 
It returns TRUE

`{"$eq": ["abc", "def"]}` 
It returns FALSE

`{"$and": [false, true]}`
It returns FALSE

`{"$and": [true, true]}`
It returns TRUE


When functions returns an object and the developer only needs one attribute of the object, the function $attribute should be used. Ex:
```
{'$eq': 
    {'$lower': {
        '$attribute': [
                {'$weather': {'city': 'California', 'date': 'today'}},
                'text'
            ]
        }
    },
    'rainy'
}
```
This condition will return TRUE only on rainy days. Note that we need to lower case the return of $weather text as it is capitalized
Note: if we want to show weather in the response this can be done using the function defined in the template engine: {{ weather(‘California’, ‘today’).text }}

Easier using case insensitive feat. of regex pattern:
```
{'$regex': {
        '$attribute': [
            {'$weather': ['California', 'today']},
            'text'
        ]
    },
    's/rainy/i'
}
```


## Available functions and operators

Most functions in this list are also available in the output template engine filters and functions (see in responses > text)

### Boolean operators:
This operators are fundamental to build a condition object with multiple criteria.

- {"$and": [*arg1, arg2, ...*]}: Boolean AND operator.
- {"$or": [*arg1, arg2, ...*]}: Boolean OR operator.
- {"$not": [*arg1, arg2*]}: Boolean NOT operator.
- {"$xor": [*arg1, arg2, ...*]}: Boolean XOR operator.

### User input value functions:
These functions return values from user input in its different forms.

- {"$userInput": [index]}:  Returns user input string.
	- index: (number. optional. defaut is 0) If defined, function will return previous user input based on the index position. Ex: `{"$userInput": [-1]}` will return the previous user input.
- {"$button": [*"buttonId"*]}: Returns true if channel informs the user clicked buttonId button.         
	- buttonId: (string or array of strings) Button id or list of button ids. 

### Channel provided inputs:
These functions returns values provided by channel taken from user linked devices like cellphone, IoT, smartwatch, etc.

- {"accelerometer": []}: Returns axis-based motion sensing data from the user's device.
- {"lightLevel": []}: Returns light level data from user's device surroundings.
- {"magnetometer": []}: Returns compass data from the user's device position.
- {"audioLevel": []} Returns dB level from user's device microphone.
- {"geolocation": []} Returns latitude/longitude position of the user's device.
- {"barometer": []} Returns altitude from the user's device.
- {"proximity": []} Returns true if device is next to the user.
- {"batteryLevel": []} Returns battery level of the user's device.
- {"heartRate": []} Returns heart rate sensed by user's smartwatch.

@TODO add more sensors from devices.

### Comparisson operators:

- {"$eq": [*arg1, arg2*]}: Returns true if arg1 is equal to arg2.
	- arg1/arg2: (string or number). 
- {"$ne": [*arg1, arg2*]}: Returns true if arg1 is not equal to arg2.
	- arg1/arg2: (string or number). 
- {"$gt": [*arg1, arg2*]}: Returns true if arg1 is greater than arg2.  
	- arg1/arg2: (number). 
- {"$gtr": [*arg1, arg2*]}: Returns true if arg1 is equal or greater than arg2.
	- arg1/arg2: (number). 
- {"$lt": [*arg1, arg2*]}: Returns true if arg1 is less than arg2.
	- arg1/arg2: (number). 
- {"$lte": [*arg1, arg2*]}: Returns true if arg1 is equal or less than arg2.
	- arg1/arg2: (number). 

### Intent matching functions:

These are the main functions related to user intent which will return matching score or true/false response in the case of pattern rule based functions.

.Flow provides several ways to test user intents, from different cloud--based services and libraries and more will be added in the future. **Take into account that some of them are paid services.**

If text to be tested is an empty string/null value, intent match functions will return false. This means wildcard patterns are not going to work when the user does not enter any text. This can happen when the channel updates other input values like, for instance, geolocation. If run method from BBot class is called, the flow will be executed by bot engine and will respond if any criteria matches with the new entered values.

##### Pattern rule based match functions: 
Bot engine will stop at the first rule based match function which returns true if the condition criteria returns true too.

- {"$regexMatch": [*pattern, text*]: Test regular expression pattern on specified text. It will store variables from it if using named capturing group. 
	- pattern: (string or array of strings) Regex pattern. See https://docs.python.org/3/library/re.html
	- text: (string) Text to be tested by the regex pattern.

- {"$aimlMatch": [*pattern, text*]}: Test AIMLv2 pattern on specified text. @TODO add more information when support is ready
	- pattern: (string or array of strings):  AIMLv2 pattern. See AIMLv2 draft.
	- text: (string) Text to be tested by the AIMLv2 pattern.

- {"$chatscriptMatch": [*pattern, text, variables*]): Test ChatScript pattern on specified text. It will store values from wildcards to variables defined in the 3rd argument. 
	- pattern: (string or array of strings) ChatScript pattern. See https://github.com/bwilcox-1234/ChatScript/blob/master/WIKI/ChatScript-Basic-User-Manual.md#simple-patterns
	- text: (string) Text to be tested by the Chatscript pattern.
	- variables: (array. optional) Defines variable name of each returning wildcard
	
##### Machine learning based match functions:
Machine learning functions returns a 'confidence' score for each intent with a trained the model with utterances/phrases defined in the utterances attribute in the root object. 

The bot engine will wait until all intents in the node object returned its score and will take the higher score as match if it is above the threshold. If when looking for more matches any pattern based function returns true (like $regex or $chatscript), bot engine will stop and take it as a match discarding the rest. 

Entities detected will be automatically stored in variables. See utterances and entities section. (@TODO add $entities functions to retrieve all entities detected by the last match function. Bot developer will be free to use $store with it?)

- {"$msCSLuisIntentMatch": [*text, utterancesId*]}: Returns confidence score based on Microsoft Cognitive Service Luis.	
	- text: (string) Text to be used to score intents.
	- utterancesId: (string) Id of the object containing utteraces/phrases to train the model.
	 
- {"$dialogFlowIntentMatch": [*text, utterancesId*]}: Returns confidence score based on Google's DialogFlow NLU service.	
	- text: (string) Text to be used to score intents.
	- utterancesId: (string) Id of the object containing utterances/phrases to train the model.
     
@TODO add IBM's Watson, Facebook's fastText, Rasa NLU, etc

##### Other functions:
- {"$question": "*text*"}: Returns true if text is an inquiry. 
	- text: (string) Text to be processed.

### Algebraic operators:

- {"$abs": [*arg1*]}: Return the number absolute value.
	- arg1: (number)
- {"$random": [*arg1*]}: Returns a random number.
	- arg1: (number) Sets the maximum number.  
- {$floor": [*arg1*]}: Returns round fractions down.
	- arg1: (number) Number to round.
- {"$add": [*arg1, arg2*]}: Return sum result of arguments.
	- arg1/arg2: (number)
- {"$sub": [*arg1, arg2*]}: Return substraction result of arguments.
	- arg1/arg2: (number)
- {"$div": [*arg1, arg2*]}: Return division result of arguments.
	- arg1/arg2: (number)
- {"$mul": [*arg1, arg2*]}: Return multiplication result of argument.
	- arg1/arg2: (number)

### Date/event functions:

- {"$cron": [*periodicityOrDate, eventId*]: Scheduler. Sets events with periodicity or date defined in unix-like cron format or epoch. When called without arguments it will return the eventId  previously defined. Also, these functions could be used without eventId defined to return true if periodicity or date is matched.
	- periodicityOrDate: (string or array of strings. optional). Unix-line cron schedule configuration or specific date unix epoch based. See $canonicalDate.
	In order to delete an event, set this arg to empty string.
	- eventId: (string. optional) Id to be retuned. 


### Notifications function helpers:

This functions helps to avoid spamming the user with duplicated notifications

- {"$notifyNTime": [*arg1, arg2*]}: Will return arg1 text only each arg2 times.
	- arg1/arg2: (string, number or object)
- ['$notifyChange": [*arg1, arg2*]}: Will return arg1 text only when it changes.


### String functions:

- {"$string": [*string*]: This is the function executed when no function is defined but a string. The string can be any text and can execute the template engine syntax with interpolation. ex. `"ID{{bookId}}"` will be taken as "ID001" if bookId value is "001"). Filters and functions provided by template engine for output text response are available too.
- {"$upper": [*string*]}: Upper case the whole string.
- {"$lower": [*string*]}: Lowercase the whole string. 
- {"$capitalize": [*string*]}: Capitalize the string.
- {"$title": [*string*]}: Capitalize all words in the string.
- {"$length": [*string*]}: Return string length.
- {"$trim": [*string*]}: Trim spaces left and right from the string.
- {"$wordCount": [*string*]}: Return word count of the string.
- {"$contains": [*string1, string2*]}: Returns true if string string1 is contained in string2. In order to get not contains use \$not function. 
Ex: `{'$not': {'$contains': {'x', 'abcde'}}}` will return true.


### Grammatical filters/functions:

- {"$grammaticalNumber": [*arg1, arg2*]}: Returns word in its plural or singular form
	- arg1: (string) Word to be processed.
	- arg2: (number) If number is 1 will return singular form, if higher than 1, will return plugarl form. Zero, null, and false will result in plural form.
- {"$grammaticalGender": [*arg1, arg2*]}: Returns word in a specified gender
	- arg1: (string) Word to be processed.
	- arg2: (string) "M" for Masculine, "F" for Feminine, "N" for Neutral. (@TODO improve gender inclusive support)


### Object/Array functions:

- {"$in": [*array, attributeName*]}: Returns true if *attributeName* is in *array*. In order to get not included values, use $not function.
- {"$attribute": [*arg1, arg2*]}: Returns attr value arg1 from object arg2. You can use dot notation for arg1 to access also a specified element from an array.
- {"$count": [*arg1*]}: Returns array elementcount


### Flow reflection:

Flow reflection is useful to get data inside the node objects. You can place data anywhere and the bot will be able to load it.

- {"$conditionalObject": [*conditionalId*]}: Returns condition object. 	
	- conditionalId: (string. optional. default is current conditional) Conditional object id.
- {"$pathsObject": [*pathId*]}: Returns path object
	- pathId: (string. optional. default is current path) Path object id.
- {"$nodeObject": [*nodeId*]}: Returns node object.
	- nodeId: (string. optional. default is current node object): Node object id.
- {"$flowObject": []}: Returns flow object.


### Sentiment analysis:

Sentiment analysis is the ability to determine if an utterance is positive, negative, or neutral.

- {"$msCSSentimentAnalysis": [*text*]}  Microsoft's Cognitive sentiment analysis service. Returns a float number between 0 and 1. 0 means negative, 1 positive and 0.5 neutral. When it can't recognize the sentiment, it returns neutral 0.5.

@TODO add Watson sentiment analysis function

### Canonicalization, stemming and lemmatization:
Due to grammatical reasons, documents are going to use different forms of a word, such as  _organize_,  _organizes_, and  _organizing_. Additionally, there are families of derivational related words with similar meanings, such as  _democracy_,  _democratic_, and  _democratization_. In many situations, it seems as if it would be useful for a search for one of these words to return documents that contain another word in the set.
The goal of both stemming and lemmatization is to reduce inflectional forms and sometimes derivationally related forms of a word to a common base form.

- {"$canonicalWord": [*word*]): Return the canonical word of argument.
	- word: (string) word to get the canonical form. 
- {"$canonicalLocation": [*location*]:  Returns canonical location. 
	Ex: `'Buenos Aires'` returns `{'city':'Buenos Aires', 'country': 'Argentina'}`
	When there is an ambiguity or a not recognized location, it will throw an exception 'INVALID_LOCATION' with variable $exception filled with a list of possible correct values if available. 
	- location: (string or object) Location in natural language or object.
	
- {"$canonicalDate": [*date*]}	Returns canonical date in different formats. @TODO add canonical types
	- date: (string) Date in natural language entered by the user.
			
@TODO add more features from WordNet and CoreNLP? research state-of-art libs

### External API functions: @TODO define arguments and returned object

These functions can be used as a condition and also can be used as response function.

- {"$weather": [*location, date*]} Returns an object with weather report of the specified date. @TODO describe object response
	- location: (string or location object): String with location in natural language form or an object with latitude/longitude attributes. If location is not recognized or if there is any ambiguity, it wil thrown an exception in the same way than function $canonicalLocation does. 
	- date: (string): String with date in natural language form or unix-like epoch date. If date is not recognized or there is an ambiguity it will throw an exception in the same way function $canonicalDate does.

- {"$httpRequest": [*url*]}: It does a custom http request. It returns an object from the web service if it returns json and returns string if is anything else.
	- url: (string) Url to be called on the request. You can interpolate variables and call functions with the template engine feature to build the url. 
Ex:
```
{
  "$eq": [
    {
      "$attribute": [
        {
          "$apiCall": "https://{user}:{password}@www.domain/path?var1={variable1}&var2={variable2}"
        },
        "response.0.wantedAttribute"
      ]
    },
    "foobar"
  ]
}
```
@TODO limit its usage


### Language/location: 
These functions return information about the user set by the channel. If the channel does not provide some information, the bot engine will try to detect the value automatically. If it is not possible, then the function will throw an exception.

- {"$userLocale": []}: Returns user locale.
- {'$userLanguage": []}:  Returns user language.
- {"$userGeolocation": []}: Returns user geolocation (latitude/longitude).
- {"$userCountry": []}: Returns user country.
- {"$userCity": []}: Returns user city.
- {"$userCurrency: []}:  Returns user currency.


### Authentication/Authorization

Authentication is handled by OAuth. When the user wants to reach a secured flow, they will be asked to follow a link which leads to the user authentication based on OAuth.

- {"$login": [*noAuth*]:	Provides a link to the user to do authentification with OAuth. Returns true if authorized.
	- noAuth: (boolean. optional. default is false) True will log the user in with no authentification needed. User will not be checked against OAuth service. This provides a way for the channel to handle authentication by itself. 
- {"$isLogged: []}: Returns true if user is already logged.

### User session variables

- {"$variable": [*variableName, scope*]: Returns variable's value. Each user has its own user session with global variables across the flow. Variables can be also interpolated on any string with the template engine like this `"variable value: {{ variableName }}"`. See $string.
	- variableName: (string): Name of the variable. Can be also dotted notation for objects and arrays.
	- scope: (string) Defines if variable is set on the bot domain or organization domain.
	
- {"$store": [*variableName, value, ttl*]}:  Stores value into a variable. This functions returns always true.
	- variableName: (string) Variable name to store the value. You can use dot notation to set values inside objects.
	- value: (object, string, number, boolean) Value to be set.
	- ttl: (number. optional. default is 0) Time to live for the set value in seconds. 0 means undefined time.

Ex: on this example a flag is set to let the bot know that the user was welcomed and it is not necessary to do it again.
Response object:
```
{
    '$store': ['welcomeStatus', 'done']
}
```
Conditional object:
```
{
    '$neq': [
        {'$variable': 'welcomeStatus'},
        'done'
    ]
}
```


### Import/Call other bot's code

- {"$callBotNode": [*botId, nodeId, parameters*]}: It will run the specified node from the specified bot. This way you can "import" all paths from a bot node. It will return flow control to your node when it reaches an end of a flow with no more paths or reaches an explicit $return function from a response object. Or if any other global path matches.
	- botId: (string) Bot id.
	- nodeId: (string) Node id.
	- parameters: (boolean or object) This object sets variables to be used from the called code. This is a way to isolate user data from the caller bot. You can overwrite internal values like $userInput with `{"bot.userInput": "new input"}` This is used to emulate AIML srai tag.
If instad of providing an object you set boolean true, the call will send all your global variables as parameters, letting the called code to access all of it. 
*Security note: take into account that sending all user data lets called code to do anything, including sending it to 3rd parties through $apiCall (@TODO limit use of $apiCall?)*

- {"$callBotPath": [*botId, pathId, parameters*]}: Executes the specified path from the specified bot and will return its returning value, ignoring the response from the intent, it will not be executed. This is used to reuse conditions criteria from other path.
	- botId: (string) Bot Id.
	- pathId: (string) Node Id.
	- parameters: See function $callBotNode.

### Misc conditional functions:

- {"$that": []}: Returns previous bot text output. (provided for future AIML support).


##            
### Highlevel language criteria definition

Instead of defining the conditions criteria with nested objects, you can define it with Python syntax using all functions available on .Flow.
Ex:
```
"{regex(\"/test/i\", userInput()) and lowercase(weather(\"california, us\").text) == \"sunny\" and userAge > 18}"
```

### Hibrid definition

You can do a mix of highlevel language criteria strings with nested object. Can be useful to have a structure with defined purposes.
Ex:
```
{
  "$and": [
    "{regex(\"/test/i\", userInput())}",
    "{lowercase(weather(\"california, us\").text) == \"sunny\"}",
    "{userAge > 18}"
  ]
}
```


## RESPONSE OBJECT

Response attribute have an array of response objects that will be executed in order from top to bottom.
Same as in conditionals, they are $functions which can be chained in nested objects.

Response functions returns data which the bot engine adds to the output defined by its type. For instance, $text function returns a string to the text output, $button function returns an object with button definition to the button type output. The bot has output types for all kind of data types to be sent to the channel.

Most of functions already documented in conditions object can be used in response object.
 
#### List of response functions:

- {$output": [*type, data*]: Feeds output defined by its datatye in order to provide the bot to send output in different ways. This function is internally used by $text, $button function and others. 
	- type: (string) Output data type. Can be any identification to label the provided output data.
	- data: (object, string, number or boolean) Can be any data to be sent to the output.
	
- {"$text": [*text*, type*]}:  Send text to the text output for channel to send to the user. 
	- text: (string or array of strings) Text to be sent to the text output. You have access to a powerful template engine compatible with django templates with lots of filters and functions.
	- type: (string) If you need random outputs or multiple controlled output you can define *text* argument as an array of strings and *type* define the way output is sent: R for random, C for continual order form the first to the last one. RC is a continual order but first randomized to get a more natural output. 

Ex: This shows the use of variable interpolation, functions and filters
```
"Hello {{username}}! Weather report for today is {{weather('today', location) | lower}}"
```  
Ex2: This is an example of using a custom function to get recipes from an API defined in the platform. dinnerName will be a variable where is stored the user input when asked for a dinner to respond with its recipe.
```html
<p>"This is the recipe list for {{dinnerName}}:</p>
<ul>
    {% for element in recipe(dinnerName}.elements %}
    <li>{{element | capitalize}}</li>
    {% endfor %}
</ul>"
```
- {"$sendEmail": [*recipientEmail, subject, body*]} Sends an email.
	- recipientEmail: (string) The email addres of the recipient.
	- subject: (string) The email subject. This, as any string, can use the template engine.
	- body:  (string) The email body text.
Ex:
```
{
    '$sendEmail': [
        ['asd@asd.com','dfg@fdg.com'],
        'the subject {{alsoWithInterpolation | filters}} - {{andFunctions(someVar)}}',
        'this is the body. It also can have all features from template engine and supports html'
        ]
},
{
    '$text': ['email sent']
}
```        
            
- {"$sendButtons": [*label, buttonId, data*]}: Returns buttons the channel will show to the end-user. When user clicks on a button, the channel has to send buttonId as a alternative input source 'button' instead of 'text'. Ex: `{"input":{"button": "buttonId"}}`. See $buttons function on conditional object.
	- label: (string): Button label
	- buttonId: (string) Unique button Id
	- data: (object. optional) Button metadata related to button styles    
            
Ex:
```
{
  "$sendButtons": [
    {
      "text": "Button one",
      "buttonId": "button1",
      "data": {
        "webChannel": {
          "backgroun-color": "black"
        },
        "otherChannel": {}
      }
    },
    {
      "text": "Button two",
      "buttonId": "button2"
    }
  ]
}
```
##### Flow control:
- {"$goto": [*nodeId*]}: Controls the flow leading the bot to the specified node id.
        If the node id has functions which needs user input, the bot engine will send all the output it has and will wait for a new user input. If the node doesn't have any, then it will go and run the conditionals and will run the matching conditional's response.
        Note: if your next node doesn't have any function which needs the user input but you still want it to wait for new user input you can use $gotoAndWait.
        
- {"$gotoAndWait": [*nodeId*]}:  Sets the next context node id, sends all output it has and will stop the bot engine, waiting for new user input.

- {"$callNode": [*nodeId*]: Calls node with nodeId.
	- nodeId: (string) Node Id to be called. 

- {'$return": []: When a node is called with $callBotNode  or $callNode this function will output all queued content and stop the execution flow and return the flow control to the caller. If there were no previous call, the function will do nothing. 
	Note: every time a response has no $goto function to continue with the flow, the bot engine will execute an implicit $return to return the flow control to the caller if any.

## Misc functions

- {"$nodeInput": "*nodeId*"}: Returns last user input entered when nodeId was the context node. 
	- nodeId: (string) Node id
- {"$nodeOutput": "*nodeId*"}: Returns resulting bot output from when nodeId was the context node.
	- nodeId: (string) Node Id.

## EXCEPTIONS

Exceptions are thrown by functions when there is a fatal error and the bot can't continue with the flow. Having exceptions lets the bot to handle situations when there are invalid data provided from the user or any unexpected issue from api calls.
Exceptions are thrown only from functions executed in responses. If there is an issue running a function from the condition criteria object, it will return false. Take this in mind when testing data on your code.

Ex:
Having a path to return the weather of a specified date and location, the user enters a non recognizable location. $weather functions throws exception "BAD_LOCATION". This is how you can handle that situation:
```
{
  "exceptions": {
    "BAD_LOCATION": [
      {
        "$text": [
          "Location not specified or is not recognized. Please enter a valid location"
        ],
        {
	      "$goto": "nodeId1"
	    }
      }
    ],
    "BAD_DATE": [
      {
        "$text": [
          "No date specified or invalid date. Please enter a valid date"
        ]
      },
      {
	      "$goto": "nodeId2"
	  }
    ]
  }
}
```
	

## PUSH NOTIFICATIONS (how to) 

Channels have to do push notification request calling the bot engine every certain time. 
If there is an user input and it matches a condition path, the bot responses and a cron notification will be executed.

On this example, a notifications for weather is pushed. The ot will notify when weather forecast announces rain for next day each 1hr. Also, it will notify when current weather changes.,
```
{
  "id": "botService",
  "name": "Botanic Bot Services",
  "entities": [],
  "nodes": [
    {
      "id": "rainAlert",
      "name": "Rain Alert Function",
      "paths": [
        {
          "id": "rainAlertNotification",
          "name": "Rain Alert Notification",
          "multipleIntent": true,
          "conditions": {
              "$regex": [
                "s/rain/i",
                {
                  "$weather": [
                    "location",
                    "tomorrow"
                  ]
                }
              ]
          },
          "responses": [
            {
              "$text": [
                {
                  "$notifyNTime": [
                    {
                      "$weather": [
                        "{location}",
                        "tomorrow"
                      ]
                    },
                    86400
                  ]
                }
              ]
            }
          ],
          "exceptions": []
        },
        {
          "id": "currentweatherChangeNotification",
          "name": "Current Weather Change Notification",
          "conditions": [
            {
              "$cron": [
                "* * * * *"
              ]
            }
          ],
          "responses": [
            {
              "$notifyChange": [
                {
                  "$weather": [
                    "{location}",
                    "current"
                  ]
                }
              ]
            }
          ],
          "exceptions": []
        }
      ]
    }
  ]
}
```




## Utterance (also called training phrases objects)

Utterances are inputs from the user that your bot needs to understand. In order to train the bot to extract intents and entities from them, it's important to capture a variety of different inputs for each intent. Active learning, or the process of continuing to train on new utterances, is essential to machine-learned intelligence.

Utterances can be used also as UTT's for pattern rule based bots to do regression tests.

This is an example of an utterance element from the utterance attribute.
It has also defined the entity to be extracted from it and the variable name to be stored with the value, which in this case, it's an attribute of location object (see entities section):

```
[
  {
    "id": "6d840f1fba71eaa1ec",
    "list": [
      {
        "text": "go to Seattle",
        "entityLabels": [
          {
            "entityName": "location.destination",
            "startCharIndex": 6,
            "endCharIndex": 12
          }
        ]
      }
    ]
  }
]
```


##  Entities object

Entity represents a word or phrase inside the user input that you want extracted. An utterance can include many entities or none at all. An entity represents a class including a collection of similar objects (places, things, people, events or concepts). Entities describe information relevant to the intent, and sometimes they are essential for your app to perform its task.

```
[
  {
    "drink": {
      "type": "list",
      "values": [
        {
          "normValue": "water",
          "synonyms": [
            "h20",
            "perrier"
          ]
        },
        {
          "normValue": "soda pop",
          "synonyms": [
            "coca-cola",
            "coke"
          ]
        }
      ]
    },
    "location": {
      "type": "hierarchical",
      "childsName": [
        "origin",
        "destination"
      ]
    },
    "KBArticle": {
      "type": "regex",
      "regex": "kb[0-9]"
    }
  }
]
```
