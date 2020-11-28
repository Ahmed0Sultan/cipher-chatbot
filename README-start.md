# Build a Message Encryptor/Decryptor Chatbot on Messenger using Wit.ai, Messenger, FastAPI

## Overview
In this tutorial, we are going to learn about wit and how we can use it to build a smart chatbot that can understand us and help us encrypt and decrypt messages using a secret key to help transfer encrypted messages with your friends.

## Prerequisites
*   A [Wit.ai](https://wit.ai/) account
*   Familiar with Python Programming
*   Familiar with Backend Development


## Table of Content
So these are the key points that we will cover:
*   [Introduction about the tools used](#Introduction-about-the-tools-used)
*   [Design the user interaction and cover the basic scenarios](#Design-the-user-interaction-and-cover-the-basic-scenarios)
*   [Create and train a Wit app to do natural language processing (NLP)](#Create-and-train-a-Wit-app-to-do-natural-language-processing)
*   [Create a Facebook Page and Facebook App to host the chatbot](#Create-a-Facebook-Page-and-Facebook-App-to-host-the-chatbot)
*   [Create Web Application using FastAPI and Integrate with Messenger and Wit](#Create-Web-Application-using-FastAPI-and-Integrate-with-Messenger-and-Wit)
*   [Deploy the Web Application using ngrok](#Deploy-the-Web-Application-using-ngrok)
*   [Next Steps](#Next-Steps)
*   [Resources](#Resources)

## Concepts
To truly understand the next sections, there are some concepts that we must go through and elaborate first.

### NLP

### Webhook

### Encryption and Decryption

## Introduction about the tools used
In this tutorial, We will create a chatbot on Messenger that will help users encrypt messages using a pre-generated key so they can share the encrypted message with their friends who will decrypt this message using the same pre-generated key. So we will start by giving a simple introduction to the frameworks and APIs we are going to use.

### Wit.ai

Wit is a natural language processing engine, It helps understand text and extract entities. And it makes the process of creating bots or apps that talk to people easier. In the scope of this project, we will use wit to understand the messages that will be sent by our users.

### Messenger

### Bot Society

### FastAPI
FastAPI is a modern, fast (high-performance), a web framework for building APIs. It’s very fast, intuitive, and easy to use. And it provides automatic swagger docs (Yaaaaay). 

## The Practical Part

## Design the user interaction and cover the basic scenarios

This is usually the most important step in building a chatbot because it has a big impact on the user experience with the chatbot. So this step needs to be revisited every now and then to ensure a good conversation flow and experience. In this tutorial, we will not focus on this part and we will use a basic conversation flow like below. You can view an interactive prototype built using Bot Society [here](https://app.botsociety.io/2.0/designs/5fbec9d4a159b908db2f63c1?m=interactive).

<details>
<summary>First Time Encrypting Scenario</summary>


```
Bot:  "Hi John, I'm Lockey. I can encrypt and decrypt messages for you to keep your secrets. What can I do for you?"

User: "I want to encrypt a message"

Bot:  "I have created a new key. Keep it Safe!!"

Bot:  "tkDJnol2UKThgJ_R8wVKVAl_iGwOoywvo45gXWD4v6c="

Bot:  "Now enter the message that you want to keep safe."

User: "I love you Lockey"

Bot:  "This is the encrypted message. It'll only be decrypted using the key that you used"

Bot: "gAAAAABfcNeVsjnOLuz0FuWCPZSNwuun2Emb4kmekRWDE9_Wh4mgqxPioS5zd86KdsI-ubTHD0wS1SdKJjRPhy04kmgbJbQizg=="

Bot:  "Do you want to encrypt anything else?"
```

</details>


<details>
<summary>Next Time Encrypting Scenario</summary>

```
Bot:  "Hi John, I'm Lockey. I can encrypt and decrypt messages for you to keep your secrets. What can I do for you?"

User: "I want to encrypt a message"

Bot:  "It seems that you have an already generated key, Do you want to keep using it?"

User:  "Yes"

Bot:  "Great. Now enter the message that you want to keep safe."

User: "I love you Lockey"

Bot:  "This is the encrypted message. It'll only be decrypted using the key that you used"

Bot: "gAAAAABfcNeVsjnOLuz0FuWCPZSNwuun2Emb4kmekRWDE9_Wh4mgqxPioS5zd86KdsI-ubTHD0wS1SdKJjRPhy04kmgbJbQizg=="

Bot:  "Do you want to encrypt anything else?"
```

</details>

<details>
<summary>Decrypting Scenario</summary>


```
Bot:  "Hi John, I'm Lockey. I can encrypt and decrypt messages for you to keep your secrets. What can I do for you?"

User: "I want to decrypt a message"

Bot:  "Please enter the key that has been shared with you to decrypt the message"

User:  "tkDJnol2UKThgJ_R8wVKVAl_iGwOoywvo45gXWD4v6c="

Bot:  "Now enter the message that you want to decrypt."

User: "gAAAAABfcNeVsjnOLuz0FuWCPZSNwuun2Emb4kmekRWDE9_Wh4mgqxPioS5zd86KdsI-ubTHD0wS1SdKJjRPhy04kmgbJbQizg=="

Bot:  "This is the decrypted message. Keep it safe and delete after you read it."

Bot:  "I love you Lockey"

Bot: "Do you want to decrypt anything else?"
```

</details>

## Create and train a Wit app to do natural language processing

* Create a new Wit App
    * Choose the name of your app
    * Choose **English** as the language of the app
    * Choose whether you want the app **Open** or **Private**
    * Click **Import** if you have a previous wit app or if you want to export the wit app used in this tutorial press [here](https://storage.googleapis.com/iam-sultan/cipher-bot-2020-10-03-07-00-12.zip)
    * Click **Create**


<p align="center">
<img align="center"  src="assets/create-app.png">
</p>

* We will be redirected to the understanding page where we will enter utterances or training data. And you'll notice that for every utterance we enter, we will need to specify the intent of that utterance (Or create the intent in case if it doesn't exist) And after every utterance click on **Train and Validate**.


<p align="center">
<img align="center" src="assets/create-intent.png">
</p>

* You'll notice that over time Wit will automatically match the utterances that you enter with the correct intent and that indicates that we are on the right path.

<p align="center">
<img align="center" src="assets/auto-detect-intent.png">
</p>

* We will keep entering data and creating intents. We will mainly use 6 intents:
    * `greeting`
    * `cipher`
    * `decipher`
    * `new_key`
    * `yes`
    * `no`

* We can find all the utterances in the utterances page.

* We can also test the Wit App using a curl request that is generated automatically by going to the **Settings** page

![Test API](assets/test-api.png)
<p align="center">
<img align="center" src="assets/test-api.png">
</p>

* And we can that the response will contain the predicted intent and the confidence of the prediction

```json
{
    "text": "decipher the message",
    "intents": [{
        "id": "935018250326137",
        "name": "decipher",
        "confidence": 0.9883
    }],
    "entities": {

    },
    "traits": {

    }
}
```

* In the same page we can get the **Server Access Token** as we will use it later when we build the app

<p align="center">
<img align="center" src="assets/app-settings_censored.jpg">
</p>

## Create a Facebook Page and Facebook App to host the chatbot

For this part, we are going to create two things:

### 1. Facebook Page

* Go to this [page](https://www.facebook.com/pages/creation/?ref_type=comet_home) to start the process of creating the new page
* Then choose your page name and category and click on the create button (And yes, that's it. you now have a page)

<p align="center">
<img align="center" src="assets/create-page-1.png">
</p>

### 2. Facebook App

* Go to [Facebook for Developers](https://developers.facebook.com/) and create an account if you don't have one and click on the **Create App** Button
* Choose **Manage Business Integrations**

<p align="center">
<img align="center" src="assets/create-developer-app.png">
</p>

* Fill the required information and click on the **Create App ID**

<p align="center">
<img align="center" src="assets/create-fb-app-3.png">
</p>

* Then from the list of products shown, Click on **Set Up** on the Messenger card

<p align="center">
<img align="center" src="assets/create-fb-app-4.png">
</p>

* In the **Access Tokens** Section, Click on the **Add or Remove Pages** Button and choose the page that we created earlier and agree on the permissions needed.

<p align="center">
<img align="center" src="assets/create-fb-app-5.png">
</p>

* Then click on the **Generate Token** Button and keep the generated token safe as we will use it later.

<p align="center">
<img align="center" src="assets/create-fb-app-6.png">
</p>

## Create Web Application using FastAPI and Integrate with Messenger and Wit 

This section illustrates how you can use this repo to build the chatbot on your own page. You can find the source code in this [repo](https://github.com/Ahmed0Sultan/cipher-chatbot) so download it and follow these steps.

### Clone Project Repository

```
$ git clone https://github.com/Ahmed0Sultan/cipher-chatbot.git
$ cd cipher-chatbot
```

### This is a tree that demonstrates the files and directories in the project.

```
└── cipher-chatbot
    ├── api
    │   ├── api.py
    │   └── endpoints
    │       └── facebook.py
    ├── connector
    │   └── facebook
    │       ├── bot.py
    │       └── utils.py
    ├── core
    │   ├── db
    │   │   ├── crud.py
    │   │   ├── database.py
    │   │   └── models.py
    │   ├── dialog
    │   │   ├── actions.py
    │   │   ├── manager.py
    │   │   └── responses.py
    │   └── nlp
    │       └── engine.py
    ├── LICENSE
    ├── main.py
    ├── README.md
    ├── requirements.txt
    └── variables.py
```

### Create and Activate a Virtual Environment

```
$ python -m venv venv
$ source ./venv/bin/activate
```

### Install Requirements

```
$ pip install -r requirements.txt
```

### Set Environment Variables
Create a new file named `.env` which will contain the secret tokens that we will use to integrate with Wit and Messenger (That we have already got in earlier steps)

```env
FB_PAGE_ACCESS_TOKEN="Very_Secret_Token"
FB_VERIFY_TOKEN="Very_Secret_Token"
WIT_SERVER_TOKEN="Very_Secret_Token"
```

### Introduction to Fernet
So Fernet is a system for symmetric encryption/decryption. It also authenticates the message, which means that the recipient can tell if the message has been altered in any way from what was originally sent.

And we can find examples to use it below

#### Create a key
```python
from cryptography.fernet import Fernet

key = Fernet.generate_key()
```
#### Encrypting a message
```python
cipher = Fernet(key)

message = "Very Secret Message".encode('utf-8')
encrypted_message = cipher.encrypt(message)
```
#### Decrypting a message
```python
cipher = Fernet(key)

decrypted_message = cipher.decrypt(encrypted_message)
```

### Explore the DB Models in Our App

In [`core/db/database.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/core/db/database.py), We can find that we created an instance of the DB and then create a session that will be used to do all the CRUD operations needed.

```python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "sqlite:///./cipher_bot.db"

engine = create_engine(
    SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False}
)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()
```

Then in [`core/db/models.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/core/db/models.py), We will find the models that will be used through the app which will be translated into database tables.

```python
from sqlalchemy import Boolean, Column, ForeignKey, Integer, String
from sqlalchemy.orm import relationship
from cryptography.fernet import Fernet

from core.db.database import Base


class User(Base):
    __tablename__ = "users"

    fb_id = Column(String, primary_key=True, unique=True, index=True)
    last_intent = Column(String)
    state = Column(String)
    last_used_key = Column(String)

    keys = relationship("Key", back_populates="owner")


class Key(Base):
    __tablename__ = "keys"

    id = Column(Integer, primary_key=True, index=True)
    key = Column(String, default=Fernet.generate_key().decode())
    owner_id = Column(Integer, ForeignKey("users.fb_id"))

    owner = relationship("User", back_populates="keys")
```
So here we have two models ([`User`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/f10bf2d047ba1463b767547d5894815b0f55a261/core/db/models.py#L8), [`Key`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/f10bf2d047ba1463b767547d5894815b0f55a261/core/db/models.py#L19)). 
* [`User`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/f10bf2d047ba1463b767547d5894815b0f55a261/core/db/models.py#L8) Model
  * We can see that we use the `fb_id` as a primary key where we will save the ID of the user that we get from the Messenger API
  * `last_intent` will contain the last intent identified by Wit so that we can handle the context of the conversation
  * `state` will contain a predefined state that we will later map to a specific action
  * `last_used_key` will contain the last key that was used by the user to make it easy for the user to reuse a key that was generated before
* [`Key`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/f10bf2d047ba1463b767547d5894815b0f55a261/core/db/models.py#L19) Model
  * `id` will contain an auto-generated primary key 
  * `key` will contain the key that was generated to encrypt and decrypt and its default value will be a key generated as illustrated above
  *  `owner_id` a foreign key that will link the key generated to the user by using the `fb_id`

And in [`core/db/crud.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/core/db/crud.py) we will find all the CRUD (Create, Read, Update, Delete) operations that we are going to use like (creating a new user, checking if the user exists, creating a new key, or updating a user state)

### Using Wit.ai in Our App
In [`core/nlp`](https://github.com/Ahmed0Sultan/cipher-chatbot/tree/master/core/nlp) directory, We can find the [`engine.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/core/nlp/engine.py) file as below
```python
from wit import Wit

from variables import WIT_SERVER_TOKEN

class NLPEngine:
    def __init__(self):
        self.engine = Wit(WIT_SERVER_TOKEN)

    def predict(self, message):
        response = self.engine.message(message)
        try:
            intent = response["intents"][0]["name"]
        except:
            intent = "fallback"

        return intent
```
In this file, we create a class where we initialize an instance of the `Wit` class with a server token that we get from the environment variables. Then we create a prediction function which takes a text message as an input and we try to extract the recognized intent from the response or return the fallback intent in the of nothing found

### Using Messenger APIs in Our App
To handle sending messages and other Messenger API calls, we use the code in [Pymessenger](https://github.com/davidchua/pymessenger) library and modify it to have additional functionalities like sending messages with quick replies. You can find the modified code in [`connector/facebook/bot.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/connector/facebook/bot.py) and the [Messenger API Docs](https://developers.facebook.com/docs/messenger-platform).

In this part we will see how we can send different types of messages using Messenger API. For this, we have the [Send API](https://developers.facebook.com/docs/messenger-platform/reference/send-api/) which we will use to send different types of messages to our users.

The Base URL that we use for this is `https://graph.facebook.com/v9.0/me/messages?access_token=<PAGE_ACCESS_TOKEN>`, where we will replace the `<PAGE_ACCESS_TOKEN>` with the token that we generated from our page.

Now we will see the different types of messages and how we used them in the code. We will list the popular ones that we are using in this tutorial:

* Actions
    * Mark Seen
    * Typing On
    * Typing Off
* Messages
    * Text
    * Quick Replies
    * Template
        * Generic Template
        * Button Template

Because all the different types use the same API, we will create two functions that are ready to send any type of message.

* [Send Raw](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/afc0419dcbfa20e2d6b1181bf24a987e9377dc78/connector/facebook/bot.py#L258)

    This function takes a payload as an input and sends a request with this payload to the **Send API**
    ```python
    def send_raw(self, payload):
        request_endpoint = 'https://graph.facebook.com/v9.0/me/messages'
        response = requests.post(
            request_endpoint,
            params=self.auth_args,
            json=payload
        )
        result = response.json()
        return result
    ```
* [Send Recipient](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/afc0419dcbfa20e2d6b1181bf24a987e9377dc78/connector/facebook/bot.py#L41)

    This function takes a payload and the recipient user id as inputs, then adds the recipient user id in the payload, and calls the [`send_raw`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/afc0419dcbfa20e2d6b1181bf24a987e9377dc78/connector/facebook/bot.py#L258) function to send the payload.
    ```python
    def send_recipient(self, recipient_id, payload):
        payload['recipient'] = {
            'id': recipient_id
        }
        return self.send_raw(payload)
    ```

<details>
<summary>Send Actions</summary>

Great!! Now we need to know the payload of the action that we want to send. We can find it in the [Send API](https://developers.facebook.com/docs/messenger-platform/reference/send-api/#properties) As well, where the `<PSID>` is the recipient user id, and `<MESSAGING_TYPE>` is the type of the message that we are sending. You can read more about the different types of messages [here](https://developers.facebook.com/docs/messenger-platform/send-messages/#messaging_types).
```json
{
  "messaging_type": "<MESSAGING_TYPE>",
  "recipient": {
    "id": "<PSID>"
  },
  "sender_action": "mark_seen"
}
```
Now it would be easy to use the base functions we created to send an action.

```python
def send_action(self, recipient_id, action):
    return self.send_recipient(recipient_id, {
        'sender_action': action
    })
```

</details>

<details>
<summary>Send Messages</summary>

In the case of sending messages, we will add another [helper function](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/afc0419dcbfa20e2d6b1181bf24a987e9377dc78/connector/facebook/bot.py#L49)
Which basically calls the `Mark Seen` and `Typing On` actions before sending a message, and a `Typing Off` action after sending the message to give the user a sense of reality while talking to the chatbot.

```python
def send_message(self, recipient_id, message):
    self.send_action(
        recipient_id=recipient_id,
        action='mark_seen'
    )

    self.send_action(
        recipient_id=recipient_id,
        action='typing_on'
    )

    message_req = self.send_recipient(recipient_id, {
        'message': message
    })

    self.send_action(
        recipient_id=recipient_id,
        action='typing_off'
    )
    return message_req
```

Now we will see how we can implement the different types of messages.

* Text Message
    Here's an example of a payload that sends a simple "Let's win this" message to the user
    ```json
    {
        "messaging_type": "<MESSAGING_TYPE>",
        "recipient": {
            "id": "<PSID>"
        },
        "message": {
            "text": "Let's win this"
        }
    }
    ```
    So we can create a [function](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/afc0419dcbfa20e2d6b1181bf24a987e9377dc78/connector/facebook/bot.py#L91) that sends a text message like that:
    ```python
    def send_text_message(self, recipient_id, message):
        return self.send_message(recipient_id, {
            'text': message
        })
    ```

* Quick Replies Message
    Here's an example of a payload that sends quick replies to the user
    ```json
    {
        "recipient":{
            "id":"<PSID>"
        },
        "messaging_type": "RESPONSE",
        "message":{
            "text": "Pick a color:",
            "quick_replies":[
                {
                    "content_type":"text",
                    "title":"Red",
                    "payload":"<POSTBACK_PAYLOAD>",
                    "image_url":"http://example.com/img/red.png"
                },{
                    "content_type":"text",
                    "title":"Green",
                    "payload":"<POSTBACK_PAYLOAD>",
                    "image_url":"http://example.com/img/green.png"
                }
            ]
        }
    }
    ```

    And we can create a [function](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/afc0419dcbfa20e2d6b1181bf24a987e9377dc78/connector/facebook/bot.py#L347) that sends a message like that:

    ```python
    def send_quick_replies(self, recipient_id, message, quick_replies):
        return self.send_message(recipient_id, {
            "text": message,
            "quick_replies": quick_replies
        })
    ```

* Generic Template Message
    Here's an example of a payload that sends a card or a carousel of cards to the user
    ```json
    {
        "recipient":{
            "id":"<PSID>"
        },
        "message":{
            "attachment":{
                "type":"template",
                "payload":{
                    "template_type":"generic",
                    "elements":[
                        {
                            "title":"Welcome!",
                            "image_url":"https://petersfancybrownhats.com/company_image.png",
                            "subtitle":"We have the right hat for everyone.",
                            "buttons":[
                                {
                                    "type":"web_url",
                                    "url":"https://petersfancybrownhats.com",
                                    "title":"View Website"
                                },{
                                    "type":"postback",
                                    "title":"Start Chatting",
                                    "payload":"DEVELOPER_DEFINED_PAYLOAD"
                                }              
                            ]      
                        }
                    ]
                }
            }
        }
    }
    ```
    So we can create a [function](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/afc0419dcbfa20e2d6b1181bf24a987e9377dc78/connector/facebook/bot.py#L106) that sends a message like that:
    ```python
    def send_generic_message(self, recipient_id, elements):
        return self.send_message(recipient_id, {
            "attachment": {
                "type": "template",
                "payload": {
                    "template_type": "generic",
                    "elements": elements
                }
            }
        })
    ```

</details>

And that's how we use the Send API.


### Dialog Management

In [`core/dialog`](https://github.com/Ahmed0Sultan/cipher-chatbot/tree/master/core/dialog) directory we will find two files:

* [`actions.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/core/dialog/actions.py)

    In this file, We will create the functions that will trigger the CRUD operations function in [`core/db/crud.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/core/db/crud.py) file, and will also trigger the Messenger API functions in [`connector/facebook/bot.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/connector/facebook/bot.py).

* [`manager.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/core/dialog/manager.py)

    In this file, We will trigger the actions that we created based on the user current state, last intent, and the current intent identified by Wit.


### Our API Routes
So for our application to send and recieve requests it has to have routes. We can find the routes of the app in the [`api`](https://github.com/Ahmed0Sultan/cipher-chatbot/tree/master/api) directory. Let's start with the [`api/endpoints/facebook.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/api/endpoints/facebook.py) file, which is the only route we have.
```python
@router.get('/facebook-webhook')
async def verify_token(token: str = Query(None, alias="hub.verify_token"),
 challenge: int = Query(None, alias="hub.challenge")):
    if token == FB_VERIFY_TOKEN:
        return challenge
    else:
        raise HTTPException(status_code=403, detail="Token invalid.")

@router.post('/facebook-webhook')
async def process_fb_requests(request: Request, db: Session = Depends(get_db)):
    output = await request.json()
    for event in output['entry']:
        messaging = event['messaging']
        for x in messaging:
            if x.get('message'):
                recipient_id = str(x['sender']['id'])
                if x['message'].get('text'):
                    message = x['message']['text']
                    dialog_manager.process_message(
                    message,
                    recipient_id,
                    db)

            elif x.get('postback'):
                recipient_id = str(x['sender']['id'])
                if x['postback'].get('title'):
                    message = x['postback']['title']
                    dialog_manager.process_message(message, 
                    recipient_id,
                    db)
    return "Success"
```
So the route name is `/facebook-webhook` and it has two methods:
* [`GET`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/f10bf2d047ba1463b767547d5894815b0f55a261/api/endpoints/facebook.py#L24)

    So the `GET` method is used to verify the bot’s token and thus connect the app with Facebook messenger.

* [`POST`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/f10bf2d047ba1463b767547d5894815b0f55a261/api/endpoints/facebook.py#L32)

    The `POST` method is used by Facebook to send the messages that are sent by users to our web application. And we can find here that we handle two types of messages normal text `message` and a `postback` message.

Then we import all the routes (Which in our case is only one) into another file [`api/api.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/api/api.py) which in turn gathers all the available routes under one instance of the [`APIRouter`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/f10bf2d047ba1463b767547d5894815b0f55a261/api/api.py#L5) class.
```python
from fastapi import APIRouter

from api.endpoints import facebook

api_router = APIRouter()
api_router.include_router(facebook.router, tags=["verify_token"])
```

### Our Application Entry Point
So After explaining the logic that resides in the application, let's explain how the app works.

We can find in the [`main.py`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/main.py) file that we initialize an instance of the [`FastAPI`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/f10bf2d047ba1463b767547d5894815b0f55a261/main.py#L5) class which we will use later to run the app. Then we will import the instance of [`APIRouter`](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/f10bf2d047ba1463b767547d5894815b0f55a261/api/api.py#L5)  that we created before and added to our app.
```python
from fastapi import FastAPI

from api.api import api_router

app = FastAPI()

app.include_router(api_router)
```

### Run Our App

To run the app just enter this command.
```sh
$ uvicorn main:app
```

## Deploy the Web Application using ngrok
So we can find that our app works on `localhost:8000` and it only works for requests created on the same computer as the running server. So we need to let Facebook know how to reach the FastAPI server. This can be done using ngrok.

Let's go to the [ngrok download page](https://ngrok.com/download) and install the version suitable for our operating system

And then we run this command to expose the local port serving the web application.
```sh
$ ./ngrok http 8000
```

> Make sure that the FastAPI is up and running

We will copy the generated link (the one with https as Facebook requires the webhook to be secure). And note that this link expires after eight hours and changes every time you run the command.

<p align="center">
<img align="center" src="assets/ngrok.png">
</p>

### Setup the webhook in the Facebook App

Back to [Facebook Developers](https://developers.facebook.com/), we can see the `Webhooks` section. Click on the `Add Callback URL`

<p align="center">
<img align="center" src="assets/add-callback.png">
</p>

And then enter the URL generated by ngrok and the `Verify Token` which we created in the `.env` file so Facebook can verify our webhook.

<p align="center">
<img align="center" src="assets/edit-callback.png">
</p>

In the same `Webhooks` section, click the **Add Subscriptions** button and check the (`messages`, `messaging_postbacks`, `message_deliveries`) boxes and click **Save**

<p align="center">
<img align="center" src="assets/subs.png">
</p>

**Congratulations**, That's it. You can now go to the page you created and start sharing encrypted messages with your friends.


## Next Steps
Next, We will make the app voice-enabled using [Wit.ai](https://wit.ai), and add the option to share the keys and messages between users.

## Resources

* [Wit Documentation](https://wit.ai/docs)
* [Messenger API Docs](https://developers.facebook.com/docs/messenger-platform)
* [FastAPI](https://fastapi.tiangolo.com/)


## License
The tutorial and the source code are licensed, as found in the [LICENSE](https://github.com/Ahmed0Sultan/cipher-chatbot/blob/master/LICENSE) file.