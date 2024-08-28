# HR-Recruiter-Bot


## Table of Contents

- [Build AI WhatsApp Bots with Python](#build-ai-whatsapp-bots-with-python)
  - [Prerequisites](#prerequisites)
  - [Get Started](#get-started)
  - [Step 1: Select Phone Numbers](#Step-1-select-phone-numbers)
  - [Step 2: Send Messages with the API](#Step-2-send-messages-with-the-api)
  - [Step 3: Configure Webhooks to Receive Messages](#Step-3-configure-webhooks-to-receive-messages)
      - [Start your app](#start-your-app)
      - [Launch ngrok](#launch-ngrok)
      - [Integrate WhatsApp](#integrate-whatsapp)
      - [Testing the Integration](#testing-the-integration)
  - [Step 4: Understanding Webhook Security](#step-4-understanding-webhook-security)
  - [Step 5: Learn about the API and Build Your App](#Step-5-learn-about-the-api-and-build-your-app)
  - [Step 6: Integrate AI into the Application](#Step-6-integrate-ai-into-the-application)
  - [Step 7: Add a Phone Number](#Step-7-add-a-phone-number)
- [Setup for BlandAI Call Integration](#Setup-for-BlandAI-Call-Integration)
  - [Setting Up WhatsApp API](#Setting-Up-WhatsApp-API)
  - [Setting Up BlandAI for Calls](#Setting-Up-BlandAI-for-Calls)
  - [Integration with MongoDB](#Integration-with-MongoDB)
  - [Scheduling-Interviews-and-Managing-Candidate-Status](#Scheduling Interviews and Managing Candidate Status)
  - [Troubleshooting and Error Handling](#Troubleshooting-and-Error-Handling)
  - [Best Practices](#Best-Practices)





# build-ai-whatsapp-bots-with-python

In this guide, you'll learn how to create a WhatsApp bot using the Meta (formerly Facebook) Cloud API, leveraging Python and Flask. We’ll also cover how to set up webhooks to receive real-time messages and integrate OpenAI for AI-generated responses. For detailed information on setting up the Flask application, refer to the provided documentation.
<br>

# prerequisites

Before you start, ensure you have the following:

1. **Meta Developer Account**: If you don’t already have a Meta developer account, you can create one [here](https://developers.facebook.com/).

2. **Business Application**: If you don't have one, you can [learn to create a business app here](https://developers.facebook.com/docs/development/create-an-app/). If you don't see an option to create a business app, select **Other** > **Next** > **Business**.

3. **Python Knowledge**: Basic understanding of Python is necessary to follow along with this tutorial.
<br>

# get-started

1. **Getting Started**: Kick off your setup [here](https://developers.facebook.com/docs/whatsapp/cloud-api/get-started).

2. **Find Your Bots**: Access your bots [here](https://developers.facebook.com/apps/).

3. **WhatsApp API Reference**: Get acquainted with the [official API documentation](https://developers.facebook.com/docs/whatsapp).

4. **Python Messaging Guide**: Follow this [step-by-step Python guide](https://developers.facebook.com/blog/post/2022/10/24/sending-messages-with-whatsapp-in-your-python-applications/) for sending messages.

5. **Message Sending API Documentation**: Explore the [API documentation](https://developers.facebook.com/docs/whatsapp/cloud-api/guides/send-messages) for sending messages.
<br>

## Step-1-select-phone-numbers

- Ensure that WhatsApp has been integrated into your app.
- Start with a test number that allows you to send messages to up to 5 different numbers.
- Navigate to the API Setup section and find the test number you'll use to send messages.
- You can also add additional numbers to which you'll send messages. Input your **personal WhatsApp number** here.
- A verification code will be sent to your phone via WhatsApp to confirm your number.
<br>


## Step-2-send-messages-with-the-api

1. Obtain a temporary 24-hour access token from the API access section.
2. You'll be provided with an example `curl` command for sending messages, which you can execute from the terminal or using a tool like Postman.
3. Convert this example into a [Python function using the requests library](https://github.com/Gowthami0901/HR-Recruiter-Bot/blob/main/bot.py).
4. Set up a `.env` file based on requirements and update it with the necessary variables.[EXAMPLE](https://github.com/Gowthami0901/HR-Recruiter-Bot/commit/cb64d41570ab368fcfcd540fded2c7c464ebbcb2)
5. You should receive a "Hello World" message (note that there may be a delay of 60-120 seconds).


To create an access token that lasts longer than 24 hours:

1. Create a [system user at the Meta Business account level](https://business.facebook.com/settings/system-users).
2. On the System Users page, assign full control of your WhatsApp app to the system user and click Save Changes.
   - [Refer to step 1 here](https://github.com/daveebbelaar/python-whatsapp-bot/blob/main/img/meta-business-system-user-token.png)
   - [Refer to step 2 here](https://github.com/daveebbelaar/python-whatsapp-bot/blob/main/img/adding-assets-to-system-user.png)
3. Click `Generate new token`, select your app, and choose the duration for the access token—either 60 days or a never-expiring token.
4. Grant all permissions, as selecting only WhatsApp permissions may lead to errors.
5. Confirm and copy the newly generated access token.


Next, locate the following details on the **App Dashboard**:

- **APP_ID**: "<YOUR-WHATSAPP-BUSINESS-APP_ID>" (Available on the App Dashboard)
- **APP_SECRET**: "<YOUR-WHATSAPP-BUSINESS-APP_SECRET>" (Available on the App Dashboard)
- **RECIPIENT_WAID**: "<YOUR-RECIPIENT-TEST-PHONE-NUMBER>" (This is your WhatsApp ID or phone number. Ensure it is registered in your account as shown in the test message example.)
- **VERSION**: "v18.0" (The most recent version of the Meta Graph API)
- **ACCESS_TOKEN**: "<YOUR-SYSTEM-USER-ACCESS-TOKEN>" (Obtained in the previous step)

> Please note that you must send a template message as your initial communication with a user. Hence, you need to send a reply before proceeding. This took me a while to figure out.
<br>


## Step-3-configure-webhooks-to-receive-messages

## start-your-app
- Ensure you have Python installed and set up, then install the required dependencies: `pip install -r requirements.txt`.
- Launch your Flask application locally by running [main.py](https://github.com/Gowthami0901/HR-Recruiter-Bot/tree/main).


## Launch-ngrok

The following steps are based on the [ngrok documentation](https://ngrok.com/docs/integrations/whatsapp/webhooks/).


Once your app is running locally, you'll need to expose it to the internet securely using ngrok:

1. If you haven't used ngrok before, sign up for a free account.
2. Download the ngrok client.
3. Go to the ngrok dashboard, click on Your [Authtoken](https://dashboard.ngrok.com/get-started/your-authtoken), and copy your Authtoken.
4. Follow the instructions to authenticate your ngrok client. This is a one-time setup.
5. In the left menu, expand Cloud Edge and select Domains.
6. On the Domains page, click + Create Domain or + New Domain. (You can start with [one free domain](https://ngrok.com/blog-post/free-static-domains-ngrok-users))
7. Start ngrok by running the following command in your terminal:
   ```
   ngrok http 8000 --domain your-domain.ngrok-free.app
   ```
8. ngrok will provide a URL that exposes your localhost application to the internet (copy this URL for use with Meta).


## integrate-whatsapp


In the Meta App Dashboard, navigate to WhatsApp > Configuration and click Edit.

1. In the Edit webhook's callback URL dialog, enter the ngrok URL with `/webhook` appended (e.g., https://myexample.ngrok-free.app/webhook).

2. Enter a verification token. This token is a string of your choice that you set when creating your webhook endpoint. Make sure to update this token in your `VERIFY_TOKEN` environment variable.

3. After adding the webhook to WhatsApp, Meta will send a validation POST request to your application through ngrok. Ensure that your localhost app logs `WEBHOOK_VERIFIED` in the terminal upon receiving the validation GET request.

4. Return to the Configuration page and click Manage.

5. In the Webhook fields dialog, select the **messages** field to subscribe. Note: You can subscribe to multiple fields.

6. With your Flask app and ngrok running, you can click "Test" next to messages to verify the subscription. You should receive a test message in uppercase. If so, your webhook setup is correct.


## testing-the-integration

Use the phone number associated with your WhatsApp product or the test number you used earlier.

1. Add this number to your WhatsApp contacts and send a message to it.
2. Confirm that your localhost app receives the message and logs both headers and body in the terminal.
3. Check if the bot replies in uppercase.
4. You've successfully integrated the bot.
5. Now, you can start developing more advanced features.
<br>


## step-4-understanding-webhook-security

**Verification Requests**

[Source](https://developers.facebook.com/docs/graph-api/webhooks/getting-started#:~:text=process%20these%20requests.-,Verification%20Requests,-Anytime%20you%20configure)

When configuring Webhooks in your App Dashboard, Meta will send a GET request to your endpoint URL. These verification requests will include query string parameters appended to your URL, such as:

```
GET https://www.your-clever-domain-name.com/webhook?
  hub.mode=subscribe&
  hub.challenge=1158201444&
  hub.verify_token=12345 or hello
```

The `verify_token` (e.g., `12345 or hello`) is a string you choose. It should be stored in the `VERIFY_TOKEN` environment variable.


**Validating Verification Requests**

[Source](https://developers.facebook.com/docs/graph-api/webhooks/getting-started#:~:text=Validating%20Verification%20Requests)

When your endpoint receives a verification request, it must:
- Check that the `hub.verify_token` matches the string you configured in the Verify Token field of the Webhooks setup.
- Respond with the `hub.challenge` value.
<br>

## Step-5-learn-about-the-api-and-build-your-app

Review the developer documentation to learn more about building your app and sending messages. [Access the documentation](https://developers.facebook.com/docs/whatsapp/cloud-api).
<br>


## Step-6-integrate-ai-into-the-application

With the connection established, you can enhance your bot's capabilities beyond just uppercase responses. 

Here’s how you can set up and integrate AI into your WhatsApp bot using the whatsapp_client.py and whatsapp_bot.py


- [whatsapp_client](https://github.com/Gowthami0901/HR-Recruiter-Bot/blob/main/whatsapp_client.py): This script processes messages and integrates AI capabilities using LangChain. It interacts with MongoDB for context and uses language models for generating responses.

    - Loads context from MongoDB.
    - Sets up LangChain for text splitting, embedding, and response generation.
    - Defines a prompt template for generating HR-related responses based on context and user input.

- [whatsapp_bot](https://github.com/Gowthami0901/HR-Recruiter-Bot/blob/main/whatsapp_bot.py): Import the 'generate_response' function into 'whatsapp_bot.py'. Update 'process_whatsapp_message()' to use the 'generate_response()' function, enabling your bot to generate dynamic responses based on user input and context.
<br>


## Step-7-add-a-phone-number

For production use, you need to add your own phone number to send messages to your users.

To start messaging any WhatsApp number, follow these steps:
1. Manage your account and phone number via the [Overview page](https://business.facebook.com/wa/manage/home/) and the [WhatsApp docs](https://developers.facebook.com/docs/whatsapp/phone-numbers/).

If you wish to use a number already associated with a WhatsApp customer or business app, you'll need to migrate it to the business platform. After migration, you'll lose access to the original WhatsApp app. [Learn about migrating an existing WhatsApp number](https://developers.facebook.com/docs/whatsapp/cloud-api/get-started/migrate-existing-whatsapp-number-to-a-business-account).

Once you've selected your phone number, add it to your WhatsApp Business Account. [Learn how to add a phone number](https://developers.facebook.com/docs/whatsapp/cloud-api/get-started/add-a-phone-number).

For experimenting without affecting your personal number, consider these options:

1. Buy a New SIM Card
2. Use Virtual Phone Numbers
3. Use Dual SIM Phones
4. Utilize a Different Device
5. Temporary Number Services
6. Dedicated Devices for Development

**Recommendation**: For long-term or professional use, consider using a virtual phone number service or purchasing a new SIM card for a dedicated device. For quick testing, a temporary number might be sufficient, but be mindful of security and privacy. Remember, once a number is associated with WhatsApp Business API, it cannot be used with regular WhatsApp on a device unless you deactivate it from the Business API and reverify it on the device.
<br>


# **Setup-for-BlandAI-Call-Integration**


## **1.Setting-Up-WhatsApp-API**
 
**Step 1: Create a WhatsApp Business Account**
- **Visit**: [WhatsApp Business](https://www.whatsapp.com/business/)
- **Follow**: The instructions to create a business account.
- **Verify**: Your phone number and business details.


**Step 2: Obtain API Access from Meta**

- **Sign Up:** Go to Meta for Developers and sign in with your Facebook account.
- **Create App:** Create a new app in the Meta Developer Dashboard. You’ll need to select the appropriate product (WhatsApp) and set up the app.
- **Request API Access:** Follow the instructions to apply for API access. You might need to provide details about your use case and business.
- **Receive Credentials:** After approval, you'll receive API credentials including api_token and cloud_number_id.


**Step 3: Configure WhatsApp API**

- **API Token:** You will receive an api_token from Meta. Store this token securely.
- **Cloud Number ID:** Obtain your cloud_number_id from the Meta Developer Dashboard or via API setup.


**Step 4: Set Up the API for Sending Messages**
- **API Endpoint**: `http://localhost:5000/send_message`
- **Request Method**: POST
- **Request Payload**:
  ```json
  {
    "template_name": "hr_recruter_ai",
    "language_code": "en_US",
    "phone_number": "<Candidate Phone Number>",
    "api_token": "<Your API Token>",
    "cloud_number_id": "<Your Cloud Number ID>",
    "selected_job_id": "<Selected Job ID>"
  }
  
  response = requests.post("http://localhost:5000/send_message", json=message_payload)
  ```


## **2.Setting-Up-BlandAI-for-Calls**

**Step 1: Create a BlandAI Account**
- **Visit**: [BlandAI](https://bland.ai/)
- **Sign Up**: Create an account using your email address.
- **Verify**: Your email address.


**Step 2: Obtain API Key**
- **Navigate**: To the API settings or dashboard in your BlandAI account.
- **Generate**: An API key for accessing the call services.


**Step 3: Set Up API for Call Scheduling**
- **API Endpoint**: `https://api.bland.ai/v1/calls`
- **Request Method**: POST
- **Request Payload**:
  ```json
  {
    "phone_number": "<Candidate Phone Number>",
    "task": "Hello, I am from Recruiter AI"
  }
  ```
- **Headers**:
  ```json
  {
    "authorization": "sk-<Your API Key>",
    "Content-Type": "application/json"
  }
  ```


**Step 4: Fetch Call Details**
- **API Endpoint**: `https://api.bland.ai/v1/calls/<call_id>`
- **Request Method**: GET
- **Headers**:
  ```json
  {
    "Authorization": "sk-<Your API Key>",
    "Content-Type": "application/json"
  }
  ```


**Step 5: Handle API Responses**
- **Check**: If the call has been completed.
- **Extract**: The `concatenated_transcript` and `interested` status.
- **Store**: The call details and update the candidate's status in MongoDB.


## **3.Integration-with-MongoDB**
 
**Step 1: Connect to MongoDB**
- **Connection String**: `mongodb://localhost:27017/`
- **Python Code Example**:
  ```python
  import pymongo
  client = pymongo.MongoClient("mongodb://localhost:27017/")
  db = client['recruiter_ai']
  ```


## **4.Scheduling-Interviews-and-Managing-Candidate-Status**
  
This below steps outlines the process for scheduling interviews, updating candidate statuses, and handling call details using a Streamlit application integrated with MongoDB and various APIs.

**Step 1: Determine Interest from Call Transcript**

The `determine_interest` function assesses the interest level of a candidate based on the transcript of a call.

**Function: `determine_interest`**

```python
def determine_interest(transcript):
    transcript_lower = transcript.lower()
    positive_keywords = ["interested", "yes", "looking for", "want", "job"]
    negative_keywords = ["not interested", "no interest", "decline", "disinterested", "no"]

    if any(keyword in transcript_lower for keyword in negative_keywords):
        return False

    if any(keyword in transcript_lower for keyword in positive_keywords):
        return True

    return None
```
<br>

**Step 2: Update Candidate Status**

The `update_candidate_status` function updates the status of a candidate in the MongoDB collection based on various criteria.

**Function: `update_candidate_status`**

```python
def update_candidate_status(candidate_id, new_status, mongo_client):
    db = mongo_client['recruiter_ai']
    candidates_collection = db['candidates']

    candidate = candidates_collection.find_one({"ID": candidate_id})
    if candidate:
        # Determine the final status for each type
        final_initiated_status = determine_initiated_status(candidate, new_status)
        final_scheduled_status = determine_scheduled_status(candidate, new_status)
        final_completed_status = determine_completed_status(candidate, new_status)
        final_offer_status = determine_offer_status(candidate, new_status)
        final_interested_status = determine_interested_status(candidate, new_status)

        # Update the candidate's status
        candidates_collection.update_one(
            {"ID": candidate_id},
            {
                "$set": {
                    "initiated_status": final_initiated_status,
                    "scheduled_status": final_scheduled_status,
                    "completed_status": final_completed_status,
                    "offer_status": final_offer_status,
                    "interested_status": final_interested_status
                },
                "$unset": {
                    "Status": ""  # Remove the Status field
                }
            }
        )
```

**Step 3: Update Candidate Interest**

The `update_candidate_interest` function updates the `interested_status` of a candidate based on the latest call details from MongoDB.

**Function: `update_candidate_interest`**

```python
def update_candidate_interest(candidate_id, mongo_client):
    db = mongo_client['recruiter_ai']
    candidates_collection = db['candidates']
    call_conversations_collection = db['call_conversations']

    candidate = candidates_collection.find_one({"ID": candidate_id})
    if not candidate:
        print(f"Candidate with ID {candidate_id} not found.")
        return

    selected_job_id = candidate.get('selected_job_id')
    if not selected_job_id:
        print(f"Selected Job ID for candidate ID {candidate_id} not found.")
        return

    latest_conversation = call_conversations_collection.find_one(
        {"selected_job_id": selected_job_id},
        sort=[("timestamp", -1)]
    )

    if latest_conversation:
        interested = latest_conversation.get("interested", False)
        candidates_collection.update_one(
            {"ID": candidate_id},
            {"$set": {"interested_status": "Interested" if interested else "Not interested"}}
        )
        print(f"Updated candidate ID {candidate_id} status to {'Interested' if interested else 'Not interested'}.")
    else:
        print(f"No conversations found for candidate ID {candidate_id} and job ID {selected_job_id}.")
```

**Step 4: Fetch and Store Call Details**

The `fetch_and_store_call_details` function retrieves call details from an API and stores them in MongoDB.

**Function: `fetch_and_store_call_details`**

```python
def fetch_and_store_call_details(call_id, mongo_client):
    url = f"https://api.bland.ai/v1/calls/{call_id}"
    headers = {
        "Authorization": "sk-hnboicwdoudlcgbajpjguispjspoxlgugaq9i224ljnawcr0lgjuzkyi94j06x1c69",
        "Content-Type": "application/json"
    }

    while True:
        try:
            response = requests.get(url, headers=headers)
            response.raise_for_status()
            call_data = response.json()
            print(f"API Response: {call_data}")

            if call_data.get("completed", False):
                concatenated_transcript = call_data.get("concatenated_transcript", "No transcript available")
                interested = determine_interest(concatenated_transcript)

                call_conversation = {
                    "call_id": call_data.get("call_id"),
                    "timestamp": datetime.now(),
                    "caller": call_data.get("from", "Unknown"),
                    "receiver": call_data.get("to", "Unknown"),
                    "message": call_data.get("summary", "No summary available"),
                    "concatenated_transcript": concatenated_transcript,
                    "call_length": call_data.get("call_length"),
                    "completed": call_data.get("completed"),
                    "started_at": call_data.get("started_at"),
                    "end_at": call_data.get("end_at"),
                    "Disposition Tag": call_data.get("Disposition Tag"),
                    "transcripts": call_data.get("transcripts"),
                    "interested": interested,
                    "selected_job_id": st.session_state.get('selected_job_id')
                }

                db = mongo_client['recruiter_ai']
                collection = db.call_conversations

                try:
                    insert_result = collection.insert_one(call_conversation)
                    print(f"Call details stored successfully with _id: {insert_result.inserted_id}")

                    # Update candidate interest based on the latest call details
                    candidate_id = st.session_state.get('selected_candidate_id')
                    if candidate_id:
                        update_candidate_interest(candidate_id, mongo_client)

                except Exception as e:
                    print(f"Error inserting data into MongoDB: {str(e)}")
                break
            else:
                print(f"Call {call_id} not completed yet. Retrying in 30 seconds...")
                time.sleep(30)
        except requests.RequestException as e:
            print(f"Request failed: {str(e)}")
            break
```

**Step 5: Schedule Interviews**

The `schedule_interviews` function schedules interviews and handles the associated communication and data updates.

**Function: `schedule_interviews`**

```python
def schedule_interviews():
    with st.spinner('Scheduling Interview for the selected Candidate...'):
        df_candidates = st.session_state.get('df_candidates')
        successful_updates = []
        error_messages = []

        user_id = st.session_state.get('user_id')
        user = fetch_user_data_by_id(user_id)
        api_token = user.get('whatsapp_api_token')
        cloud_number_id = user.get('whatsapp_cloud_number_id')

        if not api_token or not cloud_number_id:
            st.error("Missing API token or Cloud number ID.")
            st.session_state['setting_flag'] = True
            return

        mongo_client = pymongo.MongoClient("mongodb://localhost:27017/")

        for idx in st.session_state.get('checked_candidates', []):
            candidate = df_candidates.iloc[idx]
            phone_number = candidate['Phone']

            message_sent = False
            call_queued = False

            message_payload = {
                "template_name": "hr_recruter_ai",
                "language_code": "en_US",
                "phone_number": phone_number,
                "api_token": api_token,
                "cloud_number_id": cloud_number_id,
                "selected_job_id": st.session_state.get('selected_job_id')
            }

            try:
                message_response = requests.post("http://localhost:5000/send_message", json=message_payload)
                message_response.raise_for_status()
                if message_response.status_code == 200:
                    message_sent = True
                    print(f"WhatsApp message sent successfully for candidate ID: {candidate['ID']}")
                else:
                    error_messages.append(f"Failed to send WhatsApp message for candidate ID: {candidate['ID']} - Status code: {message_response.status_code}")
            except requests.exceptions.HTTPError as http_err:
                error_messages.append(f"HTTP error occurred while sending WhatsApp message for candidate ID: {candidate['ID']} - {http_err}")

            if not phone_number.startswith("+91"):
                phone_number = "+91" + phone_number

            call_payload = {
                "phone_number": phone_number,
                "task": "Hello, I am from Recruiter AI"
            }
            call_headers = {
                "authorization": "sk-hnboicwdoudlcgbajpjguispjspoxlgugaq9i224ljnawcr0lgjuzkyi94j06x1c69",
                "Content-Type": "application/json"
            }

            retry_attempts = 3
            for attempt in range(retry_attempts):
                try:
                    call_response = requests.post("https://api.bland.ai/v1/calls", json=call_payload, headers=call_headers)
                    call_response.raise_for_status()
                    if call_response.status_code == 200:
                        call_response_json = call_response.json()
                        if call_response_json.get("status") == "success":
                            call_id = call_response_json.get("call_id")
                            if call_id:
                                fetch_and_store_call_details(call_id, mongo_client)
                                call_queued = True
                                print(f"Call API Response Status Code: 200")
                                break
                            else:
                                error_messages.append(f"

Call ID not found in response for candidate ID: {candidate['ID']}")
                        else:
                            error_messages.append(f"Failed to queue call for candidate ID: {candidate['ID']}")
                    else:
                        error_messages.append(f"Call API response status code: {call_response.status_code}")
                except requests.exceptions.RequestException as request_err:
                    error_messages.append(f"Request error occurred while scheduling call for candidate ID: {candidate['ID']} - {request_err}")

            if message_sent:
                successful_updates.append(candidate['ID'])

        if successful_updates:
            st.success(f"Interviews scheduled successfully for candidates with IDs: {', '.join(map(str, successful_updates))}")

        if error_messages:
            for message in error_messages:
                st.error(message)

        st.session_state['setting_flag'] = True
```



The process involves determining candidate interest based on call transcripts, updating candidate statuses in MongoDB, and scheduling interviews with integrated messaging and calling functionalities. Each step ensures that candidates are properly managed and communicated with, leveraging various APIs and MongoDB for data handling.
<br>


# **5.Troubleshooting-and-Error-Handling**
  
**WhatsApp API Errors**
- **HTTP Errors**: Check status codes and messages.
- **Retries**: Implement retry logic for network issues.

**BlandAI Call Errors**
- **Status Checks**: Ensure the call is completed before fetching details.
- **Error Logs**: Capture and log errors for debugging.
<br>


# **6.Best-Practices**

- **Secure API Keys**: Do not hardcode keys; use environment variables or secure vaults.
- **Monitor Usage**: Keep track of API usage to avoid exceeding limits.
- **Regular Updates**: Stay updated with API changes and updates.

