# Slack ChatBot Tech-Support - OpenAI Solutions Architect

This repository demonstrates a powerful application for OpenAI's GPT-3 model, tailored to meet the needs of solutions architects. The application uses a chatbot designed to provide tech support within an organisation. 

Based on a context defined by the architect, for example, the chatbot is capable of diagnosing and troubleshooting a wide range of software and hardware issues, interacting with specific systems or applications, and providing detailed instructions to users. The responses are specifically tailored to the organisation's IT use case. The application is built with FastAPI, a modern, fast (high-performance), web framework for building APIs with Python 3.6+ based on standard Python type hints.

## Prerequisites

Ensure that you have the following installed on your local machine:

- Python 3.6+
- pip (Python package manager)
- ngrok (for local testing)

You will also need the following API keys and secrets, which should be stored securely in a .env file:

- OpenAI API key
- Slack Bot token
- Slack Bot user ID

## Setup

1. **Clone the repository**

   ```
   git clone https://github.com/Olney1/Slack-ChatBot-Tech-Support---OpenAI---Solutions-Architect.git
   ```

2. **Navigate into the directory**

   ```
   cd Slack-ChatBot-Tech-Support---OpenAI---Solutions-Architect
   ```

3. **Create a virtual environment (optional)**

   ```
   python3 -m venv venv
   source venv/bin/activate  # On Windows use `.\venv\Scripts\activate`
   ```

4. **Install the dependencies**

   ```
   pip install -r requirements.txt
   ```

## Context

This is where the bot can be customised for any company use case.

**Add your context in main.py**

In this application instance, the context sets the scope and behaviour of the chatbot by providing essential information about the specific IT set-up of the company. It acts as the initial introduction for the AI to understand the environment in which it operates. The AI uses this context to shape its responses to user queries.

**Key information examples for your own context:**

- What devices the organisation primarily uses (e.g., specific models of laptops or phones)
- The applications regularly used by the organisation. This can include both software installed on the devices (e.g., Microsoft Office suite, Adobe Creative Cloud) and web-based applications (e.g., Google Workspace, Slack)
- The sign-in methods and security protocols used by the organisation
- The procedure for common IT tasks within the organisation (e.g., how to reset a password, the procedure for installing updates)
- Specific language, abbreviations, or terminology used within the organisation

The context doesn't have to be limited to just these points. Depending on the specific needs of the organisation, it can include any relevant information that would assist the AI in providing accurate and helpful responses.

A detailed example context has been provided in the `main.py` file to give you an idea of how you can tailor the context to suit the specific needs of your organisation.

Please note, it should be self-explanatory that you'll need to replace '***' in the sample context I have provided with the relevant information from the organisation you are designing this solution for.
 
```
   context = """
   Your context goes here.
   """
```

## Running the Application Locally

1. **Start FastAPI server**

   ```
   uvicorn main:app --reload
   ```


2. **Expose your local server to the internet with ngrok**

   In a new terminal window, navigate to the directory where ngrok is installed and start a tunnel on the same port as the FastAPI application (default is 8000).

   ```
   ./ngrok http 8000
   ```

   Note the https URL ngrok provides. This is the public URL you can use to interact with your local FastAPI application.


3. **Add the ngrok URL as a Slack Event Subscription**

   Go to your app settings page on the Slack API site and navigate to 'Event Subscriptions'. Enable events, then paste the ngrok URL in the 'Request URL' box. Append `/slack/events` to the end of this URL.


## Testing

In order to test the application, you need to mimic a real user interaction. This is done by sending a POST request to the `/slack/events` endpoint with a JSON body representing a Slack event. The application should then respond by posting a message in the designated Slack channel with the tech support response.

**Add the Bot to a Private Channel**

First, you need to invite the bot to a private channel in your Slack workspace. You can create a new private channel for testing purposes. To invite the bot to the channel:

1. Go to the channel where you want to add the bot.
2. Click on the 'Details' icon at the top right.
3. Click 'More' and then 'Add apps'.
4. Search for your bot's name and click 'Add'.
5. The bot should now be a member of the channel.

**Assign Necessary Permissions in the Slack Dashboard**

In order for the bot to function properly, you need to assign necessary permissions via the Slack dashboard. These permissions include the ability to post messages, view channel details, and receive event notifications.

1. Go to your app settings page on the Slack API site.
2. Navigate to 'OAuth & Permissions'.
3. Scroll down to the 'Scopes' section. Here, you will see two categories of scopes: 'Bot Token Scopes' and 'User Token Scopes'.
4. Add necessary scopes to the 'Bot Token Scopes' category. For this application, the necessary scopes include chat:write and groups:history. Please note that these scopes are necessary for the bot to function properly. The bot must be able to read channel and message history and post messages.
5. Go to 'Events Subscriptions' and then navigate to 'Subscribe to bot events'. Add the message.groups bot user event.

**Send a POST request**

With the bot set up in your private channel and permissions granted, you can now test the application by sending a POST request to the /slack/events endpoint. You can use tools like Postman to easily send POST requests.

The body of the request should contain a JSON object representing a Slack event. This should mimic the format of the event objects that Slack sends to your bot. You can refer to the Slack API documentation for examples of these event objects.

**For example, a message event might look like this:**

```
{
    "token": "YOUR_SLACK_VERIFICATION_TOKEN",
    "team_id": "T061EG9RZ",
    "api_app_id": "A0FFV41KK",
    "event": {
        "type": "message",
        "channel": "YOUR_CHANNEL_ID",
        "user": "YOUR_USER_ID",
        "text": "Hello, bot!",
        "ts": "1558977266.000200"
    },
    "type": "event_callback",
    "event_id": "Ev0FFV41KK",
    "event_time": 1558977266,
    "authed_users": ["U0FFV41K"]
}
```

Remember to replace the placeholders (YOUR_SLACK_VERIFICATION_TOKEN, YOUR_CHANNEL_ID, YOUR_USER_ID) with actual values.

After sending the request, check the private channel in Slack. The bot should respond with a tech support message.

## Customising the Chatbot

The real power of this application lies in the ability for solutions architects to customise the chatbot based on the specific needs of an organisation or a user group. It does not have to be designed for solving company specific IT issues or replacing an IT support team. The context that the chatbot uses to generate responses can be defined, which allows tailoring the chatbot's responses to address specific problems, handle particular systems or applications, and align with the knowledge base of the target users. This makes it a versatile and powerful tool for automated, context-aware chatbot support in a variety of use cases where a company uses the Slack messaging platform (in this instance).

**For example:**

1. **Healthcare:** In a healthcare setting, the chatbot could be provided with a context that includes information about medical procedures, medications, and symptoms. It can be used to handle routine inquiries from patients, book appointments, and give advice on common health issues.

2. **Education:** In an educational environment, a chatbot could be given a context that includes curriculum details, school policies, and class schedules. It could assist students with their homework, provide study resources, and answer questions about assignments and exams.

3. **Retail:** In a retail context, the chatbot could be given details about products, sales, and store policies. It can help customers find products, answer questions about returns and exchanges, and provide recommendations based on the customer's preferences.

4. **Hospitality:** In a hotel or restaurant, a chatbot could be given a context that includes information about menus, room availability, prices, and amenities. It could handle room or table reservations, answer questions about the menu, and provide information about local attractions.

5. **Human Resources:** In an HR setting, a chatbot could be given a context that includes details about company policies, benefits, and job descriptions. It could answer employee questions about benefits, assist with the hiring process, and provide guidance on company policies.

6. **Financial Services:** A chatbot could assist customers with banking or investment queries, provide information on financial products, and help with transactions or account issues.

By changing the context, the same chatbot architecture can be applied to virtually any sector, making it a highly flexible tool for providing automated, context-aware support.

## License

[MIT](https://choosealicense.com/licenses/mit/)

## IMPORTANT INFO AND PRIVACY NOTES

Please note that this is not production ready code. It is for testing solutions via localhost, development and demonstration purposes only.

There are a few potential privacy issues that can be identified in the provided code:

**Logging of Slack events:** The code includes logging statements that log the received Slack events, including the event data. Depending on the logging configuration, this could potentially expose sensitive information contained in the events, such as user messages or other personally identifiable information (PII). It is important to ensure that the logging is properly configured to prevent the exposure of sensitive data in production environments.

**Storage of conversation histories:** The code uses a dictionary named conversation_histories to store conversation histories. If these histories are stored persistently or if the application is running for an extended period, it might accumulate a significant amount of user conversations. Storing user conversations raises privacy concerns, especially if they contain sensitive information. It is important to assess whether storing these conversations is necessary and, if so, implement appropriate measures to protect the privacy and security of the stored data.

**Usage of OpenAI API:** The code utilizes the OpenAI API to generate responses for user inputs. The prompt used for the API call may contain sensitive information, as it includes a context describing the company's tech support operations. Care should be taken to review the context and ensure that it does not inadvertently expose confidential information.

**Error handling and Slack message posting:** When an exception occurs during the OpenAI API call or during message posting to Slack, the error message is sent as a response to the Slack channel. Depending on the nature of the error, it could potentially expose sensitive information or internal system details. It is important to handle errors gracefully and avoid revealing sensitive information to users.

To address these potential privacy issues, it is recommended to:

Review the logging configuration to ensure that sensitive information is not logged.
Implement appropriate data retention and storage practices for conversation histories, including considering encryption and access controls if necessary.
Carefully review and sanitize the context used for the OpenAI API prompt to avoid exposing sensitive information.
Implement robust error handling and ensure that error messages do not reveal sensitive details to users.