This project has been created for Microsoft Bot Framework v4. This is not for Bot Framework v3.

This is a step by step process to help newbies into the Microsoft Bot framework V4.

Prerequisites - Basic knowledge of .Net and C#

Here's the link to the bot framework documentation. My notes were created on reading through the documentation.
Start with the concepts. They are very important. They might not make much sense first but once you dive into the code later, it will all fall in place and the dots will connect.
https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0&tabs=csharp

After you have gone through the concepts, you can dive into the questions and answers below.

After reading through the questions and answers below, you need to dive into some code. Always remember, this is a shorter version of the Concepts, design and develop modules of the bot framework documentation. Detailed explaination is always found at the Microsoft Bot framework documentation.

Here is the URL to the samples projects provided by Microsoft. They are pretty good for understanding how to code using the bot framework.
https://github.com/microsoft/BotBuilder-Samples/tree/master/samples/csharp_dotnetcore 

-



Q: Which card to use to render a video?

	Video Card	

Q: What are activities?

	Activities are events taking place to communicate between the channel and the bot framework.
	Activity objects are used to communicate between channel and bot framework service.

Q: How do you send out greetings to a user as a first level interaction?

	This needs to be processed in the OnMemberAddedAsync method.
	Check if the member id already exists.

Q: Which card is used to render a GIF or short video?

	Animation Card

Q: Which card to use for Showing a bill?

	Receipt Card

Q: What process can you use for sign in to an account using a bot? 

	Use a Sign In Card

Q: What type of objects are used to communicate between bot framework service and channels?

	Activity objects

Q: What is turn context?

	A turn is the processing associated with the arrival of a given activity. Bot and user takes turns to speak.
	
	The turn context object provides information about the activity such as the sender and receiver, the channel, and other data needed to process the activity. It also allows for the addition of information during the turn across various layers of the bot.
	
Q: How does adapters work and elaborate about the process from user to bot and back to user.

Q: What is an adapter and how is it related to the turn context?

	Adapters provide an abstraction for your bot to work with a variety of environments. The adapter, an integrated component of the SDK, is the core of the SDK runtime. The activity is carried as JSON in the HTTP POST body. This JSON is deserialized to create the Activity object that is then handed to the adapter with a call to process activity method. On receiving the activity, the adapter creates a turn context and calls the middleware.
	
	A bot is directed by it's adapter, which can be thought of as the conductor for your bot. The adapter is responsible for directing incoming and outgoing communication, authentication, and so on. The adapter differs based on it's environment (the adapter internally works differently locally versus on Azure) but in each instance it achieves the same goal. Adapters provide an abstraction for your bot to work with a variety of environments.

Q: What is the turn Handler?

	Turn Handler is the default (event) handler. All activities get routed through the turn handler. The turn handler then calls the individual handler for whatever type of activity it has received.

Q: What are Hero Cards used for?

	A hero card is one that contains a single large image, one or more buttons, and text. Typically used to visually highlight a potential user selection.

Q: What does OnConversationUpdateActivityAsync do? 

	On a ConversationUpdateactivity, calls a handler if members other than the bot joined or left the conversation.

Q: What is OnMembersAddedAsync used for?	

	Override this to handle members joining a conversation. So if you want some task performed when a member joins the conversation, this method needs to be overridden. 

Q: What is the concept of middleware in the bot framework?

	Middleware is much like any other messaging middleware, comprising a linear set of components that are each executed in order, giving each a chance to operate on the activity. The final stage of the middleware pipeline is a callback to the turn handler on the bot class the application has registered with the adapter's process activity method. The turn handler is generally OnTurnAsync in C#.

Q: Where do you store your app id, password, URL and keys for your bot?

	appsettings.json file
	
Q: How does your bot start? what file invokes it?

	The main method is executed first as always which configures the services and kicks off the conversation. 

Q: How can you store data for your user?

	Memory Storage - local in-memory storage. Temporary.
	Azure Blob Storage - connects to Azure's blob storage object database.
	Azure Cosmos DB Storage - Azure's NoSQL offering. 
	
Q: How are states maintained in the bot framework?

	 State is stored in state properties. State properties are lumped into three buckets -
		User State
		Conversation state
		Private Conversation State
	• User state is available in any turn that the bot is conversing with that user on that channel, regardless of the conversation
	• Conversation state is available in any turn in a specific conversation, regardless of user (i.e. group conversations)
	• Private conversation state is scoped to both the specific conversation and to that specific user

Q: How are keys formed in the three states?

	• The user state creates a key using the channel ID and from ID. For example, {Activity.ChannelId}/users/{Activity.From.Id}#YourPropertyName
	• The conversation state creates a key using the channel ID and the conversation ID. For example, {Activity.ChannelId}/conversations/{Activity.Conversation.Id}#YourPropertyName
	• The private conversation state creates a key using the channel ID, from ID and the conversation ID. For example, {Activity.ChannelId}/conversations/{Activity.Conversation.Id}/users/{Activity.From.Id}#YourPropertyName

Q: Describe how the state property accessor methods work.

	The accessor methods are the primary way for your bot to interact with state. How each work, and how the underlying layers interact, are as follows:
	• The accessor's get method:
	• Accessor requests property from the state cache.
	• If the property is in the cache, return it. Otherwise, get it from the state management object. (If it is not yet in state, use the factory method provided in the accessors get call.)
	• The accessor's set method:
	• Update the state cache with the new property value.
	• The state management object's save changes method:
	• Check the changes to the property in the state cache.
	• Write that property to storage.

	Calling the save changes method for a state management object (i.e. the buckets mentioned above) saves all properties in the state cache that you have set up to that point for that bucket, but not for any of the other buckets you may have in your bot's state.

Q: What are dialogs?

	Dialogs can be thought of as a programmatic stack, which we call the dialog stack, with the turn handler as the one directing it and serving as the fallback if the stack is empty. The topmost item on that stack is considered the active dialog, and the dialog context directs all input to the active dialog.

Q: What are the different methods used in dialogs?

	To create your dialog context, call the create context method of your dialog set.
	To start a dialog, pass the dialog ID you want to start into the dialog context's begin dialog, prompt, or replace dialog method.

	To continue a dialog, call the continue dialog method.

	The end dialog method ends a dialog by popping it off the stack and returns an optional result to the parent context (such as the dialog that called it, or the bot's turn handler). This is most often called from within the dialog to end the current instance of itself.

	If you want to pop all dialogs off the stack, you can clear the dialog stack by calling the dialog context's cancel all dialogs method.

	You can replace a dialog with itself, creating a loop, by using the replace dialog method. This is a great way to handle complex interactions and a good technique to manage menus.


Q: What are Intents, Utterances and Entities?

	Intent - A bot needs to detect what a user wants to do, which is their intent. 

	Utterances -This intent is determined from spoken or textual input, or utterances.

	Entities - A bot may also need to extract entities, which are important words in utterances. Sometimes entities are required to fulfill an intent. 

Q: How do you ensure that a user doesn't get lost in a conversation with a bot?
Q: Can a user navigate "back" in a conversation with a bot?
Q: How does a user navigate to the "main menu" during a conversation with a bot?
Q: How does a user "cancel" an operation during a conversation with a bot?

	Put a maximum number of retry attempts for each question
	check for keywords like stop cancel start over etc in middleware
	keep the conversation relevant to the current flow of conversation and do not keep referring to the old conversations. 
	Design your bot to immediately acknowledge user input, even in cases where the bot may take some time to compile its response. consider sending a "typing" message to indicate your bot is working and then followup with a proactive response.

Q: When do you need to handover the session to a human?

	On the following scenarios - 
	Triage - Collect first hand information from user before handing over to human. Name, Problem desc etc.
	Escalation - Handle a few basic tasks, if anything other than that, hand over to human.
	User-driven menu - User gets a prompt with the tasks that the bot can do and an option to speak to the agent for anything else
	Scenario-driven - The bot may decide whether or not to transfer control based upon whether or not it determines that it is capable of handling the scenario at hand.

Q: Does the bot do anything when the user and an agent are talking?

	Even after an agent is engaged, the bot remains the behind-the-scenes facilitator of the conversation. The user and agent never communicate directly with each other; they just route messages through the bot.

Q: Write the code to send a message to a user

	await turnContext.SendActivityAsync($"Welcome!");

Q: How would you read the text message that was sent by the user?

	Read it from the Text property of the Activity as shown in the following line of code. 
	var responseMessage = turnContext.Activity.Text;

Q: How will you add attachments to a bot's response?

	The Attachments property of the Activity object contains an array of Attachment objects that represent the media attachments and rich cards attached to the message. To add a media attachment to a message, create an Attachment object for the reply activity (that was created off the activity with CreateReply()) and set the ContentType, ContentUrl, and Name properties.

Q: What are the interfaces that are used for the bot framework?

	using Microsoft.Bot;

Q: Where do you add the welcome message?

	In the method OnMembersAddedAsync

Q: What is the default type of handler from which all other activities are routed through?

	The turn handler which is mentioned as the OnTurnAsync method in the ActivityHandler Class.

Q: What method do you use to send back the user a reply in the form of text?

	await turnContext.SendActivityAsync(MessageFactory.Text($"This is a new message from bot."), cancellationToken);

Q: Which class is used to maintain the state of an user in the bot service?

	UserState class which is derived from BotState

Q: Name a few properties of the HeroCard

	Title
	Text
	Images(List<CardImage>)
	Buttons(List<CardAction>)

Q: What are dialogs and how are they used?

	Dialogs are a central concept in the SDK, and provide a useful way to manage a conversation with the user. Dialogs are structures in your bot that act like functions in your bot's program; each dialog is designed to perform a specific task, in a specific order. You can specify the order of individual dialogs to guide the conversation, and invoke them in different ways - sometimes in response to a user, sometimes in response to some outside stimuli, or from other dialogs.
	
Q: What class is used to set the sequence of the flow of conversation with an user?

	The WaterfallStep delegate of the Dialog class is used to set the sequence of the flow of the conversation.

Q; How can you take some input from the user?

	Using various Prompts which are part of the Dialogs class.

Q: Name a few types of prompts

	TextPrompt, NumberPrompt, ChoicePrompt, ConfirmPrompt
	
Q: How can you create a carousel of prompts?

	By creating a prompt with an IList of Choices. Choices are a part of the Dialogs class.

Q: How do you add a card as a reply to an user?

	We add it as an attachment to the Activity.
	
Q: How can you create a carousel of cards?

	By setting the AttachmentLayout property of the reply activity as AttachmentLayoutTypes.Carousel

Q: What is the difference between AdaptiveCards and Rich Cards?

	Adaptive cards are adaptive to channels.

Q: How can you add suggested actions for the user?

	Add the SuggestedActions property to the reply Activity and add a list of card actions to the SuggestedAction property with value and text set to them.

Q: Which class is used for managing state?

	BotState class is the base class to manage state. Derived classes, like ConversationState and UserState, provide a StorageKeyFactory which is used to determine the key used to persist a given storage object.

Q: How is an activity received by a bot? How is it processed?

	An activity comes in as a JSON HTTP POST task. It is desearialised into an activity object. The adapter then takes over and calls the respective activity method and the middleware. Finally we use the turnContext to send back an activity.
	



