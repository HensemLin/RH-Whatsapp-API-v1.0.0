
# Table of Contents
- [[#GETTING STARTED]]
	- [[#Introduction]]
	- [[#Authentication]]
	- [[#Making Requests]]
- [[#ENDPOINTS]]
	- [[#Send WhatsApp Message]]
		- [[#Text Message]]
		- [[#Media Message]]
	- [[#Send WhatsApp Template]]
		- [[#Template Message]]
	- [[#Get Media ID]]
		- [[#Upload Media]]
- [[#SUPPORTED MEDIA TYPES]]

# GETTING STARTED
## Introduction

Welcome to Reply Hero API documentation. You can interact with the API through HTTP requests from any language. 

<hr>

## Authentication

The ReplyHero API uses API keys for authentication. Visit our API Keys endpoint to create the API key you’ll use in your requests.

Remember that your API key is a secret! Do not share it with others or expose it in any client-side code (browsers, apps). Production requests must be routed through your own backend server where your API key can be securely loaded from an environment variable or key management service.

```
X-API-KEY: REPLYHERO_API_KEY
```

Example curl command:

```
curl --location ‘https://whatsapp.replyherotech.com/whatsapp/messages/send' \
	-H 'X-API-KEY: eUG3uKs0r6YoHmkRZVXe0nCEmJY19r'
```

<hr>

## Making Requests

You can paste the command below into your terminal to run your first API request. Make sure to replace $REPLYHERO_API_KEY with your secret API key.

```
curl --location 'https://whatsapp.replyherotech.com/whatsapp/messages/send' \
--header 'x-api-key: eUG3uKs0r6YoHmkRZVXe0nCEmJY19r' \
--header 'Content-Type: application/json' \
--data '{
    "message": {
        "type": "text",
        "text": {
            "body": "testing"
        }
    },
    "sessionId": 'YOUR_SESSEION_ID',
    "customerId": 'YOUR_CUSTOMER_ID',
    "userId": 'YOUR_USER_ID',
    "orgId": 'YOUR_ORG_ID'
}
```

Customer IDs is the receiver IDs  \****(needs to request from RH)***
User IDs is the admin of the organization \****(needs to request from RH)***
Organization IDs \****(needs to request from RH)***

This request queries the `send WhatsApp message` endpoint to send a message with a text of *"testing"*. You should get a response back the resembles the following:

```
{
    "customerId": "CUSTOMER_ID",
    "id": "MESSAGE_ID",
    "messageType": "human",
    "orgId": "ORG_ID",
    "response": {
        "contacts": [
            {
                "input": "601******67",
                "wa_id": "601******67"
            }
        ],
        "messages": [
            {
                "id": "wamid.HBgLNjAxODM4OTE2NjcVAgARGBI3QzVBREE1Qzg5NTRCREY1RjIA"
            }
        ],
        "messaging_product": "whatsapp"
    },
    "sessionId": "testing",
    "timestamp": "Sun, 17 Mar 2024 00:45:25 +0800",
    "userId": "UKIR5Iumv6a6fs1ULT6BKBmyMjG3"
}
```

\****note*** that the client will only receive the message if they initiated the conversation, and the conversation window remain for ***24 hours*** only

Now that you've send your first message in WhatsApp, let's break down the **response object**. We can see that `messageType` is `human` which means the WhatsApp message is sent through human. There are few `messageType` such as `human`, `user`, `assistant`, and `template`. 

`human`, `template`, and `assistant` here are all classified as **sender**; while `user` here is the receiver.

Below show case some use cases:
1. **Customer Service A *(human)*** reply WhatsAPP message to **Customer B**
	- `human`: **Customer Service A** (the sender)
	- `user`: **Customer B** (the receiver)
2. **Customer Service A *(bot)*** reply WhatsAPP message to **Customer B**
	- `assitant`:  **Customer Service A** (the sender)
	- `user`: **Customer B** (the receiver)
3. **Customer Service A *(template)*** send WhatsAPP template message to **Customer B**
	- `template`: **Customer Service A** (the sender)
	-  `user`: **Customer B** (the receiver)

<hr>

# ENDPOINTS
## Send WhatsApp Message

```
POST https://whatsapp.replyherotech.com/whatsapp/messages/send
```

Sends a message to a specified recipient via WhatsApp.

<hr>

### Text Message

Programmatically send a text messages via Whatsapp.

#### Request Body
<hr>

**message** <span style="font-size: 10px">object </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
<span style="font-size: 14px">An object of messages comprising the type and body of the message.</span>
>[!example]- <span style="font-size: 14px"> Text Message </span>
><span style="font-size: 14px">**Text message**</span> <span style="font-size: 12px">object </span>
><span style="font-size: 12px">properties</span>
><hr>
>
><span style="font-size: 14px">**type**</span> <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The type of the message, in this case `text`</span>
>
><hr>
>
><span style="font-size: 14px">**text**</span> <span style="font-size: 10px">object </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The type of the message, in this case `text`</span>
>- <span style="font-size: 14px"> **body**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The body message to send</span>
>
><br>
>

**sessionId** <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
<span style="font-size: 14px">The session IDs for this communication session</span>

<hr>

**customerId** <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
<span style="font-size: 14px">The customer IDs for the receiver </span>

<span style="color: red; font-weight: 300; font-size: 12px">*Required from RH</span> 

<hr>

**userId** <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
<span style="font-size: 14px">The admin of the organization</span>

<span style="color: red; font-weight: 300; font-size: 12px">*Required from RH</span> 

<hr>

**orgId** <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
<span style="font-size: 14px">The organization IDs for the sender</span>

<span style="color: red; font-weight: 300; font-size: 12px">*Required from RH</span>

#### Example Requests
<hr>

Example curl commnd:

```
curl --location 'https://whatsapp.replyherotech.com/whatsapp/messages/send' \
--header 'x-api-key: REPLYHERO_API_KEY' \
--header 'Content-Type: application/json' \
--data '{
  "message": {
     "type": "text",
      "text": {
           "body": "testing msg"
      }
   },
   "sessionId": "SESSION_ID",
   "customerId": "CUSTOMER_ID",
   "userId": "USER_ID",
   "orgId": "ORG_ID"
}'
```

Example Python - request command

```python
import requests
import json

url = "https://whatsapp.replyherotech.com/whatsapp/messages/send"

payload = json.dumps({
 "message": {
   "type": "text",
   "text": {
     "body": "testing msg"
   }
 },
 "sessionId": "SESSION_ID",
 "customerId": "CUSTOMER_ID",
 "userId": "USER_ID",
 "orgId": "ORG_ID"
})
headers = {
 'x-api-key': 'REPLYHERO_API_KEY',
 'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```

#### Response Example
<hr>

Example success response
```
{
   "customerId": "CUSTOMER_ID",
   "id": "MESSAGE_ID",
   "messageType": "human",
   "orgId": "ORG_ID",
   "response": {
       "contacts": [
           {
               "input": "601******67",
               "wa_id": "601******67"
           }
       ],
       "messages": [
           {
               "id": "wamid.HBgL******REY1RjIA"
           }
       ],
       "messaging_product": "whatsapp"
   },
   "sessionId": "SESSION_ID",
   "timestamp": "Sun, 17 Mar 2024 00:45:25 +0800",
   "userId": "USER_ID"
}
```

<hr>

### Media Message

Programmatically send a media messages via WhatsApp.

#### Request Body
<hr>

**message** <span style="font-size: 10px">object </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
<span style="font-size: 14px">An object of messages comprising the type and body of the message.</span>

> [!example]- <span style="font-size: 14px"> Media Message </span>
><span style="font-size: 14px">**Media message**</span> <span style="font-size: 12px">object </span>
><span style="font-size: 12px">properties</span>
><hr>
>
><span style="font-size: 14px">**type**</span> <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The type of the message, in this case `image`.</span>
></span>
><span style="color: green; font-size: 12px; font-weight: 300">Supported media type include:</span>
><span style="font-size: 12px; font-weight: 300">`image`, `document`, and  `video`.</span>
>
><hr>
>
><span style="font-size: 14px">**Media**</span> <span style="font-size: 10px">object </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The type of the message, in this case `image`</span>
></span>
><span style="color: green; font-size: 12px; font-weight: 300">Supported media type include:</span>
><span style="font-size: 12px; font-weight: 300">`image`, `document`, and  `video`.</span>
>
><br>


>[!example]- <span style="font-size: 14px"> Media Object </span>
><span style="font-size: 14px">**Media**</span> <span style="font-size: 12px">object </span>
><span style="font-size: 12px">properties</span>
>
><hr>
>
><span style="font-size: 14px"> **file**</span> <span style="font-size: 10px">object </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">The image file to be sent.</span>
></span>
><span style="color: red; font-size: 12px; font-weight: 300">Required when `type` is `document`, `image`, or `video` and you are not using `link`.</span>
>
><hr>
>
><span style="font-size: 14px"> **link**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">The protocol and URL of the media to be sent. Use **only** with HTTP/HTTPS URLs.</span>
></span>
><span style="color: red; font-size: 12px; font-weight: 300">Required when `type` is `document`, `image`, or `video` and you are not using `file`.</span>
>
><hr>
>
><span style="font-size: 14px"> **caption**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">The media asset caption</span>
>
><hr>
>
><span style="font-size: 14px"> **filename**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">Describes the filename for the specific document. Use **only** with `document` media.</span>
>
><hr>
>
><span style="font-size: 14px"> **provider**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">This path is optionally used with a `link` when the HTTP/HTTPS link is not directly accessible and requires additional configurations like a bearer token.</span>
>
><br>
>
><br>


>[!example]- <span style="font-size: 14px"> Media File </span>
><span style="font-size: 14px">**File**</span> <span style="font-size: 12px">object </span>
><span style="font-size: 12px">properties</span>
>
><hr>
>
> <span style="font-size: 14px"> **name**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The name of the image file to be sent</span>
>
><hr>
>
> <span style="font-size: 14px"> **type**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">Type of media file being uploaded. See [[#SUPPORTED MEDIA TYPES]] for more information.</span>
>
><hr>
>
><span style="font-size: 14px"> **displayType**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The type of the media to be displayed</span>
></span>
><span style="color: green; font-size: 12px; font-weight: 300">Supported `displayType` include:</span>
><span style="font-size: 12px; font-weight: 300">`IMAGE`, `DOCUMENT`, and  `VIDEO`.</span>
>
><hr>
>
 ><span style="font-size: 14px"> **extension**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The extension of the image file to be sent</span>
>
><hr>
>
><span style="font-size: 14px"> **data**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The **bytes** of the image file to be sent in string</span>
>
>

<hr>

**sessionId** <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
<span style="font-size: 14px">The session IDs for this communication session</span>

<hr>

**customerId** <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
<span style="font-size: 14px">The customer IDs for the receiver </span>

<span style="color: red; font-weight: 300; font-size: 12px">*Required from RH</span> 

<hr>

**userId** <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
<span style="font-size: 14px">The admin of the organization</span>

<span style="color: red; font-weight: 300; font-size: 12px">*Required from RH</span> 

<hr>

**orgId** <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
<span style="font-size: 14px">The organization IDs for the sender</span>

<span style="color: red; font-weight: 300; font-size: 12px">*Required from RH</span> 

#### Example Requests
<hr>

Example curl command:

```
curl --location 'https://whatsapp.replyherotech.com/whatsapp/messages/send' \
--header 'x-api-key: REPLYHERO_API_KEY' \
--header 'Content-Type: application/json' \
--data '{
   "message": {
       "type": "image",
       "image": {
           "file": {
               "name": "bot.png",
               "type": "image/png",
               "displayType": "IMAGE",
               "extension": "png",
               "data": "BYTES_DATA_IN_STRING"
           },
           "caption": "testing"
       }
   },
   "sessionId": "SESSION_ID",
   "customerId": "CUSTOMER_ID",
   "userId": "USER_ID",
   "orgId": "ORG_ID"
}
'
```

Example Python - request command: 

```python
import requests
import json

url = "https://whatsapp.replyherotech.com/whatsapp/messages/send"

payload = json.dumps({
 "message": {
   "type": "image",
   "image": {
     "file": {
       "name": "bot.png",
       "type": "image/png",
       "displayType": "IMAGE",
       "extension": "png",
       "data": "BYTES_DATA_IN_STRING"
     },
     "caption": "testing"
   }
 },
 "sessionId": "SESSION_ID",
 "customerId": "CUSTOMER_ID",
 "userId": "USER_ID",
 "orgId": "ORG_ID"
})
headers = {
 'x-api-key': 'REPLYHERO_API_KEY',
 'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```

#### Response Example
<hr>

Example success response
```
{
   "customerId": "CUSTOMER_ID",
   "id": "MESSAGE_ID",
   "messageType": "human",
   "orgId": "ORG_ID",
   "response": {
       "contacts": [
           {
               "input": "601******67",
               "wa_id": "601******67"
           }
       ],
       "messages": [
           {
               "id": "wamid.HBgL******REY1RjIA"
           }
       ],
       "messaging_product": "whatsapp"
   },
   "sessionId": "SESSION_ID",
   "timestamp": "Sun, 17 Mar 2024 00:45:25 +0800",
   "userId": "USER_ID"
}
```

<hr>

## Send WhatsApp Template 

```
POST https://whatsapp.replyherotech.com/whatsapp/template/send
```

Sends a template message to a all the recipient that are grouped under the same organization via WhatsApp.

<hr>

### Template Message

Programmatically send a template message via WhatsApp. 

#### Request Body
<hr>

**templateName** <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span>  
An object of messages comprising the type and body of the message.

---

**sessionId** <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
The session IDs for this communication session

---

**userId** <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
The admin of the organization  
  
*<span style="color: red; font-weight: 300; font-size: 12px">*Required from RH</span> 

---

**orgId** <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
The organization IDs for the sender  
  
<span style="color: red; font-weight: 300; font-size: 12px">*Required from RH</span> 

---

**components** <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
A list of components objects comprising the parameters of the message.

>[!example]- <span style="font-size: 14px"> Components Object </span>
><span style="font-size: 14px">**Components Object**</span> <span style="font-size: 12px">object </span>
><span style="font-size: 12px">properties</span>
><hr>
>
><span style="font-size: 14px">**type**</span> <span style="font-size: 10px"> string  </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The type of the component.</span>
></span>
><span style="color: green; font-size: 12px; font-weight: 300">Supported component type include:</span>
><span style="font-size: 12px; font-weight: 300">`header`, and `body`.</span>
>
><hr>
>
><span style="font-size: 14px">**Parameters**</span> <span style="font-size: 10px">array </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">A list of parameters objects with the content of the message</span>
>
><br>

>[!example]- <span style="font-size: 14px"> Parameters Object </span>
><span style="font-size: 14px">**Parameters**</span> <span style="font-size: 12px">object </span>
><span style="font-size: 12px">properties</span>
>
><hr>
>
><span style="font-size: 14px"> **type**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
><span style="font-size: 14px">The type of the parameters.</span>
></span>
><span style="color: green; font-size: 12px; font-weight: 300">Supported  `type` is `text`, `document`, `image`, and `video`.</span>
></span>
><span style="color: red; font-size: 12px; font-weight: 300">*Note: only component `type` `header` support `document`, `image`, and `video`.</span>
>
><hr>
>
><span style="font-size: 14px"> **text**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span>
><span style="font-size: 14px">The message’s text. Character limit varies based on the following included component type.</span>
>
><span style="font-size: 14px">For the `header` component `type`:</span>
>
><ul><li><span style="font-size: 14px">60 characters</span></li></ul>
>
><span style="font-size: 14px">For the `body` component `type`:</span>
>
><ul><li><span style="font-size: 14px">1024 characters if other component types are included</span></li><li><span style="font-size: 14px">36768 characters if `body` is the only component type included</span></li></ul>
>
><span style="color: red; font-size: 12px; font-weight: 300">Required when `type` is `text`.</span>
>
><hr>
>
><span style="font-size: 14px"> **Media Object**</span> <span style="font-size: 10px">object </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">The type of the media, in this case `image`</span>
></span>
><span style="color: green; font-size: 12px; font-weight: 300">Supported media type include:</span>
><span style="font-size: 12px; font-weight: 300">`image`, `document`, and  `video`.</span>
></span>
><span style="color: red; font-size: 12px; font-weight: 300">Required when `type` is `image`, `document`, or `video`.</span>

>[!example]- <span style="font-size: 14px"> Media Object </span>
><span style="font-size: 14px">**Media**</span> <span style="font-size: 12px">object </span>
><span style="font-size: 12px">properties</span>
>
><hr>
>
><span style="font-size: 14px"> **id**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">The media object ID. Do not use this field when parameter `type` is set to `text`.</span>
></span>
><span style="color: red; font-size: 12px; font-weight: 300">Required when parameter `type` is `document`, `image`, or `video` and you are not using `link`.</span>
>
><hr>
>
><span style="font-size: 14px"> **link**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">The protocol and URL of the media to be sent. Use **only** with HTTP/HTTPS URLs.</span>
></span>
><span style="color: red; font-size: 12px; font-weight: 300">Required when parameter `type` is `document`, `image`, or `video` and you are not `file`.</span>
>
><hr>
>
><span style="font-size: 14px"> **caption**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">The media asset caption</span>
>
><hr>
>
><span style="font-size: 14px"> **filename**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">Describes the filename for the specific document. Use **only** with `document` media.</span>
>
><hr>
>
><span style="font-size: 14px"> **provider**</span> <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Optional</span> 
><span style="font-size: 14px">This path is optionally used with a `link` when the HTTP/HTTPS link is not directly accessible and requires additional configurations like a bearer token.</span>
>

<br>
#### Example Requests
<hr>

Example curl command:
```
curl --location 'https://whatsapp.replyherotech.com/whatsapp/template/send' \
--header 'x-api-key: REPLYHERO_API_KEY' \
--header 'Content-Type: application/json' \
--data '{
   "templateName": "TEMPLATE_NAME",
   "sessionId": "SESSION_ID",
   "orgId": "ORG_ID",
   "userId": "USER_ID",
   "components": [
       {
           "type": "header",
           "parameters": [
               {
                   "type": "image",
                   "image": {"id": "MEDIA_ID"}
               }
           ]
       },
       {
           "type": "body",
           "parameters": [
               {
                   "type": "text",
                   "text": "https://replyhero.io/"
               },
               {
                   "type": "text",
                   "text": "https://replyhero.io/"
               }
           ]
       }
   ]
}'
```

Example Python - request command: 

```python
import json

url = "https://whatsapp.replyherotech.com/whatsapp/template/send"

payload = json.dumps({
 "templateName": "TEMPLATE_NAME",
 "sessionId": "SESSION_ID",
 "orgId": "ORG_ID",
 "userId": "USER_ID",
 "components": [
   {
     "type": "header",
     "parameters": [
       {
         "type": "image",
         "image": {
           "id": "MEDIA_ID"
         }
       }
     ]
   },
   {
     "type": "body",
     "parameters": [
       {
         "type": "text",
         "text": "https://replyhero.io/"
       },
       {
         "type": "text",
         "text": "https://replyhero.io/"
       }
     ]
   }
 ]
})
headers = {
 'x-api-key': 'REPLYHERO_API_KEY',
 'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```

#### Response Example
<hr>

Example success response

```
[
   {
       "customerId": "CUSTOMER_ID",
       "file": null,
       "id": "MESSAGE_ID",
       "message": "TEMPLATE_NAME",
       "messageType": "template",
       "orgId": "ORG_ID",
       "response": {
           "contacts": [
               {
                   "input": "601******13",
                   "wa_id": "601******13"
               }
           ],
           "messages": [
               {
                   "id": "wamid.HBgL******REY1RjIA"
               }
           ],
           "messaging_product": "whatsapp"
       },
       "sessionId": "SESSION_ID",
       "timestamp": "Sun, 17 Mar 2024 13:37:13 +0800",
      "userId": "USER_ID"
   },
   {
       "customerId": "CUSTOMER_ID",
       "file": null,
       "id": "MEESSAGE_ID",
       "message": "TEMPLATE_NAME",
       "messageType": "template",
       "orgId": "ORG_ID",
       "response": {
           "contacts": [
               {
                   "input": "601******67",
                   "wa_id": "601******67"
               }
           ],
           "messages": [
               {
                   "id": "wamid.HBgL******REY1RjIA"
              }
           ],
           "messaging_product": "whatsapp"
       },
       "sessionId": "SESSION_ID",
       "timestamp": "Sun, 17 Mar 2024 13:37:13 +0800",
       "userId": "USER_ID"
   },
   {
       "customerId": "CUSTOMER_ID",
       "file": null,
       "id": "MESSAGE_ID",
       "message": "TEMPALTE_NAME",
       "messageType": "template",
       "orgId": "ORG_ID",
       "response": {
           "contacts": [
               {
                   "input": "601******28",
                   "wa_id": "601******28"
               }
           ],
           "messages": [
               {
                   "id": "wamid.HBgL******REY1RjIA"
               }
           ],
           "messaging_product": "whatsapp"
       },
       "sessionId": "SESSION_ID",
       "timestamp": "Sun, 17 Mar 2024 13:37:14 +0800",
       "userId": "USER_ID"
   },
   {
       "customerId": "CUSTOMER_ID",
       "file": null,
       "id": "USER_ID",
       "message": "TEMPLATE_NAME",
       "messageType": "template",
       "orgId": "ORG_ID",
       "response": {
           "contacts": [
               {
                   "input": "601******78",
                   "wa_id": "601******78"
               }
           ],
           "messages": [
               {
                   "id": "wamid.HBgL******REY1RjIA"
               }
           ],
           "messaging_product": "whatsapp"
       },
       "sessionId": "SESSION_ID",
       "timestamp": "Sun, 17 Mar 2024 13:37:15 +0800",
       "userId": "USER_ID"
   }
]
```

<hr>

## Get Media ID

```
POST https://whatsapp.replyherotech.com/whatsapp/uploads/media_upload
```

To complete some of the API calls such as [[#Send WhatsApp Template]], you need to have a media ID. You can retrieve the media ID by uploading the media. 

<hr>

### Upload Media
Once you have successfully uploaded media files to the API, the media ID is included in the response to your call.

#### Request Body \*(form-data)
---

**orgId** <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
The organization IDs for the sender  
  
<span style="color: red; font-weight: 300; font-size: 12px">*Required from RH</span> 

---

**type** <span style="font-size: 10px">string </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
Type of media file being uploaded. See [[#SUPPORTED MEDIA TYPES]] for more information.

---

**file** <span style="font-size: 10px">file </span><span style="color: red; font-weight: 300; font-size: 14px">Required</span> 
The media file

#### Example Requests
<hr>

Example curl command:

```
curl --location 'https://whatsapp.replyherotech.com/whatsapp/uploads/media_upload' \
--header 'X-API-KEY: REPLYHERO_API_KEY' \
--form 'orgId="ORG_ID"' \
--form 'type="image/jpg"' \
--form 'file=@"/path/to/image/example.jpeg"'
```

Example Python - request command:

```python
import requests

url = "https://whatsapp.replyherotech.com/whatsapp/uploads/media_upload"

payload = {'orgId': 'ORG_ID','type': 'image/jpg'}
files=[
 ('file',('example.jpeg',open('/path/to/image/example.jpeg','rb'),'image/jpeg'))
]
headers = {
	 'X-API-KEY': 'REPLYHERO_API_KEY'
}

response = requests.request("POST", url, headers=headers, data=payload, files=files)

print(response.text)

```

#### Response Example
<hr>

Example success response

```
{
   "id": "1063108041439098"
}
```

<hr>

# SUPPORTED MEDIA TYPES

Supported options for images are:

- `image/jpeg`
- `image/png`

Supported options for documents are:

- `text/plain`
- `application/pdf`
- `application/vnd.ms-powerpoint`
- `application/msword`
- `application/vnd.ms-excel`
- `application/vnd.openxmlformats-officedocument.wordprocessingml.document`
- `application/vnd.openxmlformats-officedocument.presentationml.presentation`
- `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

Supported options for video are:

- `video/mp4`
- `video/3gp`

