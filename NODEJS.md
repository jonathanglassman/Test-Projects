# GOV.UK Notify Node.js client

This documentation is for developers interested in using this Node.js client to integrate their government service with GOV.UK Notify.

## Installation

```shell
npm install --save notifications-node-client
```

## Getting started

```javascript
var NotifyClient = require('notifications-node-client').NotifyClient,
	notifyClient = new NotifyClient(apiKey);
```

Generate an API key by logging in to [GOV.UK Notify](https://www.notifications.service.gov.uk) and going to the _API integration_ page.

### Connect through a proxy (optional)

```
notifyClient.setProxy(proxyUrl);
```

## Send messages

### Text message

#### Method 

<details>
<summary>
Click here to expand for more information.
</summary>

```javascript
notifyClient
	.sendSms(templateId, phoneNumber, personalisation, reference)
	.then(response => console.log(response))
	.catch(err => console.error(err))
;
```

</details>

#### Response

If the request is successful, `response` will be an `object`.
<details>
<summary>
Click here to expand for more information.
</summary>

```javascript
{
    "id": "bfb50d92-100d-4b8b-b559-14fa3b091cda",
    "reference": null,
    "content": {
        "body": "Some words",
        "from_number": "40604"
    },
    "uri": "https://api.notifications.service.gov.uk/v2/notifications/ceb50d92-100d-4b8b-b559-14fa3b091cd",
    "template": {
        "id": "ceb50d92-100d-4b8b-b559-14fa3b091cda",
       "version": 1,
       "uri": "https://api.notifications.service.gov.uk/v2/templates/bfb50d92-100d-4b8b-b559-14fa3b091cda"
    }
}
```

Otherwise the client will return an error `err`:

|`err.error.status_code`|`err.error.errors`|
|:---|:---|
|`429`|`[{`<br>`"error": "RateLimitError",`<br>`"message": "Exceeded rate limit for key type TEAM of 10 requests per 10 seconds"`<br>`}]`|
|`429`|`[{`<br>`"error": "TooManyRequestsError",`<br>`"message": "Exceeded send limits (50) for today"`<br>`}]`|
|`400`|`[{`<br>`"error": "BadRequestError",`<br>`"message": "Can"t send to this recipient using a team-only API key"`<br>`]}`|
|`400`|`[{`<br>`"error": "BadRequestError",`<br>`"message": "Can"t send to this recipient when service is in trial mode - see https://www.notifications.service.gov.uk/trial-mode"`<br>`}]`|

</details>

#### Arguments

<details>
<summary>
Click here to expand for more information.
</summary>

##### `phoneNumber`

The phone number of the recipient, only required for sms notifications.

##### `templateId`

Find by clicking **API info** for the template you want to send.

##### `reference`

An optional identifier you generate. The `reference` can be used as a unique reference for the notification. Because Notify does not require this reference to be unique you could also use this reference to identify a batch or group of notifications.

You can omit this argument if you do not require a reference for the notification.

##### `personalisation`

If a template has placeholders, you need to provide their values, for example:

```javascript
personalisation={
    'first_name': 'Amala',
    'reference_number': '300241',
}
```

Otherwise the parameter can be omitted or `undefined` can be passed in its place.

</details>


### Email

#### Method

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Response

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Arguments

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


### Letter

#### Method

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Response

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Arguments

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


## Get the status of one message

#### Method

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Response

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>

#### Arguments

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>

## Get the status of all messages (with pagination)

#### Method

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Response

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>



#### Arguments

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


## Get the status of all messages (without pagination)

#### Method

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>

#### Response

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Arguments

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


## Get a template by ID

#### Method 

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Response

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Arguments

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


## Get all templates

#### Method

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Response

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Arguments

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


## Generate a preview template

#### Method

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Response

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>


#### Arguments

XYZ
<details>
<summary>
Click here to expand for more information.
</summary>

XYZ
</details>



