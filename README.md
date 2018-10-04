# Watson Assistant Sample Application With Dashbot

This tutorial takes you through desining, building and deploying a Watson Assistant chatbot on a nodejs application. This is part of the IBM/Dashbot meetup held at Galvanize.

## Presentation
[Introduction to chatbot using Watson Assistant and Dashbot Analytics PDF](assets/ibm-dashbot-galvanize-100318.pdf)

## Before you begin

* Create an IBM Cloud account
    * [Sign up](https://www.ibm.com/cloud/) or use an existing account. Your account must have available space for at least 1 app and 1 service.
* Make sure that you have the following prerequisites installed:
    * The [Node.js](https://nodejs.org/#download) runtime, including the [npm][npm_link] package manager

## Step 1- Provision IBM Watson Assistant Service
1. Log into IBM Cloud and provision a Watson Assistant Service

![alt text](/pics/step1-1-1.png)

2. You can change the name to something more meaningful. Pick the free tier and click create

![alt text](/pics/step1-2-1.png)

3. Click on Launch Tool to go to Watson assistant home page

![alt text](/pics/step1-3-1.png)

## Step 2 - Designing Your Bot
Building a chatbot with Watson Assistant is so easy, some developers choose to dive right into the tooling. However, with a well-thought out, well-planned chatbot, the interaction with the user can lead to a much better experience that can handle edge cases. In this section, we will design the interaction between a user, Dave, and a chatbot named HungerBot.

A good question to ask yourself is, "Who is my user and what problem do they have?" Expand on the user's profile by determining what the user needs from this chatbot. Does the user have a need to book a reservation at a restaurant? Or an answer to a common question like "Where's the bathroom?" at a conference. Maybe a chatbot that handles tasks like turning on lights or other equipment. It might help to think of the chatbot as an automated version of an existing agent, such as a customer service agent. Look at existing processes that include repeated manual processes, which can sometimes be augmented with chatbots.

Training a chatbot is like training a human agent. You will train the chatbot with the knowledge of certain tasks (intents) and things that these tasks interact with (entities). These components are then combined to create a dialog tree that can take one or more paths to respond to the user's request.

In the following steps, we have provided a sample restaurant chatbot that handles reservations for a restaurant. On the right side, it's your turn to create your own chatbot. Fill in the blanks to design your chatbot.

1. Envision the user that interacts with the bot.

    |Example:|Your turn:|
    |------------------------------------------------|------------------------------------------|
    |A user, named Dave, needs to book a table at the restaurant.|A user, named _______________________, needs to _______________________________________.|

2. Now, let's give the chatbot a name and describe the overall function it can help with.

    |Example:|Your turn:|
    |------------------------------------------------|-----------------------------------------|
    |The chatbot, named HungerBot, can help the user with common tasks at a restaurant.|The chatbot, named _______________________, can help the user with ________________________.|

3. It can be helpful to take a snapshot of an existing dialogue and then break it down into intents and entities. A sample conversation is shown below. Keep the conversation simple…you can always add more complex logic later.

    |Example:|Your turn:|
    |------------------------------------------------|-----------------------------------------|
    |HungerBot: Hi, I'm HungerBot. You can ask to reserve a table and more.|Bot: |
    |Dave: I want to book a table.|User: |
    |HungerBot: What type of cuisine would you like?|Bot: |
    |Dave: I like American food|User: |
    |HungerBot: When do you want to book a table?|Bot: |
    |Dave: Tonight|User: |
    |HungerBot: How many people will be coming?|Bot: |
    |Dave: Five people|User: |
    |HungerBot: Excellent! Here are the details of your booking.|Bot: |

4. Let's start with the action the user wants to do, which is referred to as an intent. Write a human-friendly description of the action the user is wanting to perform. List at least five ways the user might phrase this action. Lastly, add a label, like a variable name in code (alpha-numeric, underscores, etc.), that can be used later as a reference.

    |Example:|Your turn:|
    |------------------------------------------------|-----------------------------------------|
    |Intent: book a reservation|Intent: |
    |Variations||
    |1.  Reserve a table|1.|
    |2.  Book a reservation|2.|
    |3.  Make a reservation|3.|
    |4.  Secure a reservation|4.|
    |5.  Schedule a reservation|5.|
    |Label: #book_reservation|Label: #|

  	If you find that you don't have many variations, invite a friend (or a real user!) to suggest how they would ask "*to book a reservation.*" In the real world, you could use customer interactions as a base of inspiration or use a thesaurus.

5. Another component to training a chatbot is recognizing objects, which is referred to as an entity. This example reservation system can differentiate different types of cuisine. We add a type of cuisine to booking a reservation.

    |Example:|Your turn:|
    |------------------------------------------------|-----------------------------------------|
    |Entity: type of cuisine|Entity:|
    |Variations:||
    |1. Mexican|1.|
    |2. Chinese|2.|
    |3. American|3.|
    |4. Italian|4.|
    |5. Mediterranean|5.|
    |Label: @cuisine|Label: @|

    We could add time and number entities, however, there are some built-in system entities provided by IBM, like numbers, dates, and times, that the HungerBot will use. If you have another entity, define the additional entity in a new table.

In the Dialog editor of Watson Assistant, we can now setup logic to step the user through the conversation. In the next section, we will use this design to train the Watson Assistant service.

[Here is an attempt to design the bot as an exercise in mind mapping](assets/conference-bot-mindmap.pdf)

## Step 3 Build your bot

1.	Once on the home page, click on workspaces tab

![alt text](/pics/step3-1-1.png)

2.	Click on “Create a new workspace” and your chatbot a name. We will use conference-chatbot here. At this point, you are brought to the tooling to build conference-bot.

![alt text](/pics/step3-2-1.png)

3.	Let’s start by creating an empty dialog so we can start trying out the bot. Click on “Dialog” tab.

![alt text](/pics/step3-3-1.png)

4.	Click on “Create”. You are given the default skeleton dialog

![alt text](/pics/step3-4-1.png)

5.	Click on “Try it” to see what the bot looks like. There is not much going on right now, but you should see a welcome message from the bot.

![alt text](/pics/step3-5-1.png)

6.	Let’s start by adding a Greeting intent. Click on “Intents” tab.

![alt text](/pics/step3-6-1.png)

7.	Click on “Add intent” and name is #greeting_hello

![alt text](/pics/step3-7-1.png)

8.	Give the following user examples:

* Hello
* Hi
* Howdy
* What’s going on ?
* How are you doing today ?
* Hey
* Hey !
* What’s up !

![alt text](/pics/step3-8-1.png)

9.	Let’s add a node in our dialog to handle this user intent. Click on the back button next to the intent name to go back to the main page.


10.	Click on Dialog tab. Once there, click on the three dots next to “Welcome” dialog and then pick “Add node below”.

![alt text](/pics/step3-10-1.png)


11.	This should bring up the detail pane for the new node. Give is a name of “user_greeting” and then add “#greeting_hello” to the “If the bot recognizes” field. Finally, add the following responses 

* Hello back ! Hope you are doing well today.
* Hey there ! Greetings !
* What’s going on !
* Hey hey !

![alt text](/pics/step3-11-1.png)


12.	Let’s try it out. Click “clear” button to clear out the “try it out” panel. Then type “hello” as the user. See what comes back.

![alt text](/pics/step3-12-1.png)

You can type it again and the next response should be returned by the bot.

![alt text](/pics/step3-12-2.png)

13.	Now, let’s handle the case where the user wants to register for the event. Click on the “Intents” tab and add the following intent
Intent: #user_registration
Description: The user wants to register for the event.

User Examples:
* Am I too late to join ?
* Can I join the sessions now ?
* Can I register now ?
* I would like to attend !
* Let me in !
* Let’s do this !
* Sign me up !
* Where can I sign up ?

![alt text](/pics/step3-13-1.png)

14.	When the user wants to register, we want to collect the following information
* Full Name
* Email
* Phone
* Location (where the person is coming from)

Lets start adding these entities


15.	We will use system defined entities for full name and location. Go to the home page. You may have to click on the back arrow if you are inside the intent page. Click on “Entites”. Click on “System entities” and then enable @sys-location and @sys-person.

![alt text](/pics/step3-15-1.png)

16.	We will now define the email entity. Click on “My entities” and then “Add entity”. For email, we will use the regular expression pattern “^[^@\s]+@[^@\s]+\.[^@\s]+$”. This means anytime the user enters something that matches this pattern, it will be stored as the email of the user. Finally click on “Add value”. Your screen should look like this at this point:

![alt text](/pics/step3-16-1.png)

17.	Click on the back arrow to go to “Entities” page and then “Add entity”. Let’s add the phone number. We will assume the user can enter a phone numbers as (xxx)xxx-xxxx. Add the phone entity with the pattern “\(\d{3}\)\d{3}-\d{4}” as shown below.

![alt text](/pics/step3-17-1.png)

18. Let’s add a node in our dialog to handle user registration. Go back to the “Dialog” tab and add a node below the “user_greeting” node.

![alt text](/pics/step3-18-1.png)

19.	Before doing anything else, let’s first enable slots. Click on the “Customize” button on the top right and toggle the “slots” button. Slots will let us gather information in this node. Click Apply.

![alt text](/pics/step3-19-1.png)

20.	Once back in the node pane, name it “user_register” and then trigger the node when the intent “#user_registration” is recognized. The bottom of the screen should let you enter slots in the “Then check for:” section. 

21.	In the “check for” column, add the entities we just added/enabled in the entities tab. In the “save it as”, use meaningful variable names to store these values. In the “If not present, ask” column, you can add questions that the user will be asked if the slot is not filled yet. You screen should look like: 

```
@sys-person: $person: What is your full name ?
@email.literal: $email: What is your email ?
@phone.literal: $phone: We need your phone in case of emergency.
@sys-location: $location: Where are you coming from ?
```
Finally, in the “Then respond with”, enter “Thank you !”

![alt text](/pics/step3-21-1.png)

22.	Let’s try it out. Open the “Try it” panel

![alt text](/pics/step3-22-1.png)

23.	Let’s change the output to be more user friendly. Change the “Then respond with” text to “Thank you <?$person?>. You have been registered. Please check <?$email?> for more details on the conference.”. Let’s try it out again

![alt text](/pics/step3-23-1.png)

24.	You can check the variables by clicking on the “Context variables” button next to the try it panel. Close it after you are done to go back to the “try it” panel.

![alt text](/pics/step3-24-1.png)

25.	Finally, let’s add a way for the user to exit. Go to the “Content Catalog” tab and add the “General” topic to the workspace.

![alt text](/pics/step3-25-1.png)

26.	If you go back to the “Intents” tab, you will see more intents added there.

![alt text](/pics/step3-26-1.png)

27.	Go back into dialog and add a node under user_register node.

![alt text](/pics/step3-27-1.png)

28.	In the detail pane, name the node “user_bye”. Use “#General_Ending” intent as the trigger for the node and finally response with “Thank you for talking to me. See you soon.” Your screen should look like

![alt text](/pics/step3-28-1.png)

29.	If you open the “try it” panel and type “bye”, you should see the thank you message.

30.	To make things more interesting, let’s allow the user to say bye in the middle of registering. Open the user_register node in the dialog pane. Click on “Customize” button. Click on “Digressions” tab and enable “away Digressions”. This means the user can go away from this node. Click Apply.

![alt text](/pics/step3-30-1.png)

31.	Click on the “user_bye” node to open the detail pane. Click “Customise” and “Digressions” and ensure the screens looks like the following

![alt text](/pics/step3-31-1.png)

32.	Let’s try it out ! Ask to register and in the middle, say “Bye”. 

![alt text](/pics/step3-32-1.png)

## Step 4 Install Locally

Let’s create a front end of this app. 

1. First 
Git clone this repository: https://github.com/lidderupk/chatbot-dashbot-workshop
2.	`cd` into the directory and execute `npm install`
3.	Make a copy of the .env.local file and call it .env. 
4.	Give your bot a unique id NAME_CONFERENCE_CHAT
5.	Get the DASHBOT_API_KEY from dashbot 
6.	Get the WORKSPACE_ID from the workspace detail

![alt text](/pics/step4-6-1.png)

7.	Get the CONVERSATION_USERNAME and the CONVERSATION_PASSWORD from the IBM Cloud service you created earlier.

![alt text](/pics/step4-7-1.png)

![alt text](/pics/step4-7-2.png)

8. Run `npm start`
9. Point your browser to http://localhost:3000 to try out the app.

## Additional Links
1. [Watson Assistant Service Home](https://www.ibm.com/watson/ai-assistant/)
2. [Tutorials and Documentation](https://console.bluemix.net/docs/services/conversation/getting-started.html#gettingstarted)
3. [API for different SDKs](https://www.ibm.com/watson/developercloud/assistant/api/v1/node.html?node)
4. [API Explorer ](https://watson-api-explorer.ng.bluemix.net/apis/assistant-v1)
5. [IBM Functions](https://www.ibm.com/cloud/functions)
6. [IBM Cloud Dashboard](https://console.bluemix.net/dashboard/apps/)
