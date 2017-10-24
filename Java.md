# GOV.UK Notify Java client

This documentation is for developers interested in using this Java client to integrate their government service with GOV.UK Notify.

## Installation

### Maven

The notifications-java-client is deployed to [Bintray](https://bintray.com/gov-uk-notify/maven/notifications-java-client). Add this snippet to your Maven `settings.xml` file.

<details>
<summary>
Click here to expand for more information.
</summary>

```xml
<?xml version='1.0' encoding='UTF-8'?>
<settings xsi:schemaLocation='http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd' xmlns='http://maven.apache.org/SETTINGS/1.0.0' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
<profiles>
	<profile>
		<repositories>
			<repository>
				<snapshots>
					<enabled>false</enabled>
				</snapshots>
				<id>bintray-gov-uk-notify-maven</id>
				<name>bintray</name>
				<url>http://dl.bintray.com/gov-uk-notify/maven</url>
			</repository>
		</repositories>
		<pluginRepositories>
			<pluginRepository>
				<snapshots>
					<enabled>false</enabled>
				</snapshots>
				<id>bintray-gov-uk-notify-maven</id>
				<name>bintray-plugins</name>
				<url>http://dl.bintray.com/gov-uk-notify/maven</url>
			</pluginRepository>
		</pluginRepositories>
		<id>bintray</id>
	</profile>
</profiles>
<activeProfiles>
	<activeProfile>bintray</activeProfile>
</activeProfiles>
</settings>
```
Then add the Maven dependency to your project.
```xml
    <dependency>
        <groupId>uk.gov.service.notify</groupId>
        <artifactId>notifications-java-client</artifactId>
        <version>3.4.0-RELEASE</version>
    </dependency>

```
</details>

### Gradle

<details>
<summary>
Click here to expand for more information.
</summary>

```
repositories {
    mavenCentral()
    maven {
        url  "http://dl.bintray.com/gov-uk-notify/maven"
    }
}

dependencies {
    compile('uk.gov.service.notify:notifications-java-client:3.4.0-RELEASE')
}
```
</details>

### Artifactory or Nexus

Click 'set me up!' on https://bintray.com/gov-uk-notify/maven/notifications-java-client for instructions.

## Getting started

```java
import uk.gov.service.notify.NotificationClient;
import uk.gov.service.notify.Notification;
import uk.gov.service.notify.NotificationList;
import uk.gov.service.notify.SendEmailResponse;
import uk.gov.service.notify.SendSmsResponse;

NotificationClient client = new NotificationClient(apiKey);
```

Generate an API key by signing in to [GOV.UK Notify](https://www.notifications.service.gov.uk) and going to the **API integration** page.

## Send messages

### Text message

#### Method 

<details>
<summary>
Click here to expand for more information.
</summary>

```java
Map<String, String> personalisation = new HashMap<>();
personalisation.put("name", "Jo");
personalisation.put("reference_number", "13566");
SendSmsResponse response = client.sendSms(templateId, mobileNumber, personalisation, "yourReferenceString");
```

</details>

#### Response

If the request is successful, the SendSmsResponse is returned from the client. Attributes of the SendSmsResponse are listed below. 
<details>
<summary>
Click here to expand for more information.
</summary>

```java
    UUID notificationId;
    Optional<String> reference;
    UUID templateId;
    int templateVersion;
    String templateUri;
    String body;
    Optional<String> fromNumber;

```

Otherwise the client will raise a `NotificationClientException`:

|status code|error message|
|:---|:---|
|`429`|`429 {`<br>`"errors":`<br>`[{`<br>`"error": "RateLimitError",`<br>`"message": "Exceeded rate limit for key type live of 10 requests per 10 seconds"`<br>`}]`<br>`}`|
|`429`|`429 {`<br>`"errors":`<br>`[{`<br>`"error": "TooManyRequestsError",`<br>`"message": "Exceeded send limits (50) for today"`<br>`}]`<br>`}`|
|`400`|`400 {`<br>`"errors":`<br>`[{`<br>`"error": "BadRequestError",`<br>`"message": "Can"t send to this recipient using a team-only API key"`<br>`}]`<br>`}`|
|`400`|`400 {`<br>`"errors":`<br>`[{`<br>`"error": "BadRequestError",`<br>`"message": ""Can"t send to this recipient when service is in trial mode - see https://www.notifications.service.gov.uk/trial-mode""`<br>`}]`<br>`}`|

</details>

#### Arguments

<details>
<summary>
Click here to expand for more information.
</summary>

#### `phoneNumber`
The mobile number the SMS notification is sent to.

#### `emailAddress`
The email address the email notification is sent to.

#### `templateId`

The template id is visible on the template page in the application.

#### `personalisation`

The letter must contain:

- mandatory address fields
- optional address fields if applicable
- fields from template

#### `personalisation` (for letters)

If you are sending a letter, you will need to provide the address fields in the format `"address_line_#"`, numbered from 1 to 6, and also the `"postcode"` field
The fields `"address_line_1"`, `"address_line_2"` and `"postcode"` are required.

#### `reference`
An optional unique identifier for the notification or an identifier for a batch of notifications. `reference` can be an empty string or null.

</details>

### Email

#### Method

<details>
<summary>
Click here to expand for more information.
</summary>

```java
HashMap<String, String> personalisation = new HashMap<>();
personalisation.put("name", "Jo");
personalisation.put("reference_number", "13566");
SendEmailResponse response = client.sendEmail(templateId, mobileNumber, personalisation, "yourReferenceString");
```

</details>

#### Response - SendEmailResponse

If the request is successful, the SendEmailResponse is returned from the client. Attributes of the SendEmailResponse are listed below. 

<details>
<summary>
Click here to expand for more information.
</summary>


```java
	UUID notificationId;
	Optional<String> reference;
	UUID templateId;
	int templateVersion;
	String templateUri;
	String body;
	String subject;
	Optional<String> fromEmail;

```

Otherwise the client will raise a `NotificationClientException`:

|`error.status_code`|`error.message`|
|:---|:---|
|`429`|`[{`<br>`"error": "RateLimitError",`<br>`"message": "Exceeded rate limit for key type TEAM of 10 requests per 10 seconds"`<br>`}]`|
|`429`|`[{`<br>`"error": "TooManyRequestsError",`<br>`"message": "Exceeded send limits (50) for today"`<br>`}]`|
|`400`|`[{`<br>`"error": "BadRequestError",`<br>`"message": "Can"t send to this recipient using a team-only API key"`<br>`]}`|
|`400`|`[{`<br>`"error": "BadRequestError",`<br>`"message": "Can"t send to this recipient when service is in trial mode - see https://www.notifications.service.gov.uk/trial-mode"`<br>`}]`|

</details>

#### Arguments

<details>
<summary>Click here for more information</summary>

#### `phoneNumber`
The mobile number the SMS notification is sent to.

#### `emailAddress`
The email address the email notification is sent to.

#### `templateId`

The template id is visible on the template page in the application.

#### `personalisation`

The letter must contain:

- mandatory address fields
- optional address fields if applicable
- fields from template

#### `personalisation` (for letters)

If you are sending a letter, you will need to provide the address fields in the format `"address_line_#"`, numbered from 1 to 6, and also the `"postcode"` field
The fields `"address_line_1"`, `"address_line_2"` and `"postcode"` are required.

#### `reference`
An optional unique identifier for the notification or an identifier for a batch of notifications. `reference` can be an empty string or null.

</details>

### Letter

#### Method

The letter must contain:

- mandatory address fields
- optional address fields if applicable
- fields from template

<details>
<summary>
Click here to expand for more information.
</summary>

```java
HashMap<String, String> personalisation = new HashMap<>();
personalisation.put("address_line_1", "The Occupier"); // mandatory address field
personalisation.put("address_line_2", "Flat 2"); // mandatory address field
personalisation.put("address_line_3", "123 High Street"); // optional address field
personalisation.put("address_line_4", "Richmond upon Thames"); // optional address field
personalisation.put("address_line_5", "London"); // optional address field
personalisation.put("address_line_6", "Middlesex"); // optional address field
personalisation.put("postcode", "SW14 6BH"); // mandatory address field
personalisation.put("application_id", "1234"); // field from template
personalisation.put("application_date", "2017-01-01"); // field from template

SendLetterResponse response = client.sendLetter(templateId, personalisation, "yourReferenceString");
```
</details>

#### Response

If the request is successful, the SendLetterResponse is returned from the client. Attributes of the SendLetterResponse are listed below.
<details>
<summary>
Click here to expand for more information.
</summary>

```java
	UUID notificationId;
	Optional<String> reference;
	UUID templateId;
	int templateVersion;
	String templateUri;
	String body;
	String subject;
```

Otherwise the client will raise a `NotificationClientException`:

|`error.status_code`|`error.message`|
|:---|:---|
|`429`|`[{`<br>`"error": "RateLimitError",`<br>`"message": "Exceeded rate limit for key type live of 10 requests per 20 seconds"`<br>`}]`|
|`429`|`[{`<br>`"error": "TooManyRequestsError",`<br>`"message": "Exceeded send limits (50) for today"`<br>`}]`|
|`400`|`[{`<br>`"error": "BadRequestError",`<br>`"message": "Cannot send letters with a team api key"`<br>`]}`|
|`400`|`[{`<br>`"error": "BadRequestError",`<br>`"message": "Cannot send letters when service is in trial mode - see https://www.notifications.service.gov.uk/trial-mode"`<br>`}]`|
|`400`|`[{`<br>`"error": "ValidationError",`<br>`"message": "personalisation address_line_1 is a required property"`<br>`}]`|

</details>

#### Arguments

<details>
<summary>Click here to expand for more information.</summary>

#### `phoneNumber`
The mobile number the SMS notification is sent to.

#### `emailAddress`
The email address the email notification is sent to.

#### `templateId`

The template id is visible on the template page in the application.

#### `personalisation`

The letter must contain:

- mandatory address fields
- optional address fields if applicable
- fields from template

#### `personalisation` (for letters)

If you are sending a letter, you will need to provide the address fields in the format `"address_line_#"`, numbered from 1 to 6, and also the `"postcode"` field
The fields `"address_line_1"`, `"address_line_2"` and `"postcode"` are required.

#### `reference`
An optional unique identifier for the notification or an identifier for a batch of notifications. `reference` can be an empty string or null.

</details>


## Get the status of one message

#### Method

<details>
<summary>
Click here to expand for more information.
</summary>

```python
response = notifications_client.get_notification_by_id(notification_id)
```
</details>

#### Response

If the request is successful, `response` will be a `dict`. 
<details>
<summary>
Click here to expand for more information.
</summary>


```python
{
  "id": "notify_id", # required
  "reference": "client reference", # optional
  "email_address": "email address",  # required for emails
  "phone_number": "phone number",  # required for sms
  "line_1": "full name of a person or company", # required for letter
  "line_2": "123 The Street", # required for letter
  "line_3": "Some Area", # optional
  "line_4": "Some Town", # optional
  "line_5": "Some county", # optional
  "line_6": "Something else", # optional
  "postcode": "postcode", # required for letter
  "type": "sms|letter|email", # required
  "status": "current status", # required
  "template": {
    "version": 1 # template version num # required
    "id": 1 # template id # required
    "uri": "/v2/template/{id}/{version}", # required
  },
  "body": "Body of the notification",
  "subject": "Subject of an email notification or None if an sms message"
	"created_at": "created at", # required
	"sent_at": " sent to provider at", # optional
	"completed_at:" "date the notification is delivered or failed" # optional
}
```

Otherwise the client will raise a `HTTPError`:

|`error.status_code`|`error.message`|
|:---|:---|
|`404`|`[{`<br>`"error": "NoResultFound",`<br>`"message": "No result found"`<br>`}]`|
|`400`|`[{`<br>`"error": "ValidationError",`<br>`"message": "id is not a valid UUID"`<br>`}]`|

</details>

## Get the status of all messages (with pagination)

#### Method

This will return one page of notifications (250) per call. Use the `get_all_notifications_iterator` to retrieve all notifications unpaginated. 
<details>

<summary>
Click here to expand for more information.
</summary>


```python
response = notifications_client.get_all_notifications(template_type, status, reference, older_than)
```

</details>

#### Response

If the request is successful, `response` will be a `dict`. 
<details>
<summary>
Click here to expand for more information.
</summary>



```python
{"notifications":
  [
    {
      "id": "notify_id", # required
      "reference": "client reference", # optional
      "email_address": "email address",  # required for emails
      "phone_number": "phone number",  # required for sms
      "line_1": "full name of a person or company", # required for letter
      "line_2": "123 The Street", # required for letter
      "line_3": "Some Area", # optional
      "line_4": "Some Town", # optional
      "line_5": "Some county", # optional
      "line_6": "Something else", # optional
      "postcode": "postcode", # required for letter
      "type": "sms | letter | email", # required
      "status": sending | delivered | permanent-failure | temporary-failure | technical-failure # required
      "template": {
        "version": 1 # template version num # required
        "id": 1 # template id # required
        "uri": "/v2/template/{id}/{version}", # required
      },
      "body": "Body of the notification",
      "subject": "Subject of an email notification or None if an sms message"
      "created_at": "created at", # required
      "sent_at": " sent to provider at", # optional
      "completed_at:" "date the notification is delivered or failed" # optional
    },
    …
  ],
  "links": {
    "current": "/notifications?template_type=sms&status=delivered",
    "next": "/notifications?other_than=last_id_in_list&template_type=sms&status=delivered"
  }
}
```

Otherwise the client will raise a `HTTPError`:

|`error.status_code`|`error.message`|
|:---|:---|
|`400`|`[{`<br>`"error": "ValidationError",`<br>`"message": "bad status is not one of [created, sending, delivered, pending, failed, technical-failure, temporary-failure, permanent-failure]"`<br>`}]`|
|`400`|`[{`<br>`"error": "ValidationError",`<br>`"message": "Apple is not one of [sms, email, letter]"`<br>`}]`|


</details>


#### Arguments


<details>
<summary>Click here to expand for more information.</summary>

##### `template_type`

You can filter by:

* `email`
* `sms`
* `letter`

You can omit this argument to ignore this filter.

##### `status`

__email__

You can filter by:

* `sending` - the message is queued to be sent by the provider.
* `delivered` - the message was successfully delivered.
* `failed` - this will return all failure statuses `permanent-failure`, `temporary-failure` and `technical-failure`.
* `permanent-failure` - the provider was unable to deliver message, email does not exist; remove this recipient from your list.
* `temporary-failure` - the provider was unable to deliver message, email box was full; you can try to send the message again.
* `technical-failure` - Notify had a technical failure; you can try to send the message again.

You can omit this argument to ignore this filter.

__text message__

You can filter by:

* `sending` - the message is queued to be sent by the provider.
* `delivered` - the message was successfully delivered.
* `failed` - this will return all failure statuses `permanent-failure`, `temporary-failure` and `technical-failure`.
* `permanent-failure` - the provider was unable to deliver message, phone number does not exist; remove this recipient from your list.
* `temporary-failure` - the provider was unable to deliver message, the phone was turned off; you can try to send the message again.
* `technical-failure` - Notify had a technical failure; you can try to send the message again.

You can omit this argument to ignore this filter.

__letter__

You can filter by:

* `accepted` - the letter has been generated.
* `technical-failure` - Notify had an unexpected error while sending to our printing provider

You can omit this argument to ignore this filter.

##### `reference`

This is the `reference` you gave at the time of sending the notification. The `reference` can be a unique identifier for the notification or an identifier for a batch of notifications.

You can omit this argument to ignore the filter.

##### `olderThanId`

You can get the notifications older than a given Notification.notificationId.

You can omit this argument to ignore the filter.

</details>

## Get the status of all messages (without pagination)

#### Method

<details>
<summary>
Click here to expand for more information.
</summary>

```python
response = get_all_notifications_iterator(status="sending")
```
</details>

#### Response

If the request is successful, `response` will be a `<generator object>` that will yield all messages. 
<details>
<summary>
Click here to expand for more information.
</summary>

```python
<generator object NotificationsAPIClient.get_all_notifications_iterator at 0x1026c7410>
```

Otherwise the client will raise a `HTTPError`:

|`error.status_code`|`error.message`|
|:---|:---|
|`400`|`[{`<br>`"error": "ValidationError",`<br>`"message": "bad status is not one of [created, sending, delivered, pending, failed, technical-failure, temporary-failure, permanent-failure]"`<br>`}]`|
|`400`|`[{`<br>`"error": "ValidationError",`<br>`"message": "Apple is not one of [sms, email, letter]"`<br>`}]`|

</details>

#### Arguments

<details>
<summary>Click here to expand for more information.</summary>

##### `template_type`

You can filter by:

* `email`
* `sms`
* `letter`

You can omit this argument to ignore this filter.

##### `status`

__email__

You can filter by:

* `sending` - the message is queued to be sent by the provider.
* `delivered` - the message was successfully delivered.
* `failed` - this will return all failure statuses `permanent-failure`, `temporary-failure` and `technical-failure`.
* `permanent-failure` - the provider was unable to deliver message, email does not exist; remove this recipient from your list.
* `temporary-failure` - the provider was unable to deliver message, email box was full; you can try to send the message again.
* `technical-failure` - Notify had a technical failure; you can try to send the message again.

You can omit this argument to ignore this filter.

__text message__

You can filter by:

* `sending` - the message is queued to be sent by the provider.
* `delivered` - the message was successfully delivered.
* `failed` - this will return all failure statuses `permanent-failure`, `temporary-failure` and `technical-failure`.
* `permanent-failure` - the provider was unable to deliver message, phone number does not exist; remove this recipient from your list.
* `temporary-failure` - the provider was unable to deliver message, the phone was turned off; you can try to send the message again.
* `technical-failure` - Notify had a technical failure; you can try to send the message again.

You can omit this argument to ignore this filter.

__letter__

You can filter by:

* `accepted` - the letter has been generated.
* `technical-failure` - Notify had an unexpected error while sending to our printing provider

You can omit this argument to ignore this filter.

##### `reference`

This is the `reference` you gave at the time of sending the notification. The `reference` can be a unique identifier for the notification or an identifier for a batch of notifications.

</details>

## Get a template by ID

#### Method 
This will return the latest version of the template. Use [get_template_version](#get-a-template-by-id-and-version) to retrieve a specific template version. 
<details>
<summary>
Click here to expand for more information.
</summary>


```python
response = notifications_client.get_template(
    'template_id'
)
```
</details>

#### Response

If the request is successful, `response` will be a `dict`. 
<details>
<summary>
Click here to expand for more information.
</summary>


```python
{
    "id": "template_id", # required
    "type": "sms" | "email" | "letter", # required
    "created_at": "created at", # required
    "updated_at": "updated at", # required
    "version": "version", # integer required
    "created_by": "someone@example.com", # email required
    "body": "Body of the notification", # required
    "subject": "Subject of an email or letter notification or None if an sms message"
}
```

Otherwise the client will raise a `HTTPError`:

|`error.status_code`|`error.message`|
|:---|:---|
|`404`|`[{`<br>`"error": "NoResultFound",`<br>`"message": "No Result Found"`<br>`}]`|

</details>

#### Arguments

<details>
<summary>Click here to expand for more information.</summary>

##### `template_id`

Find by clicking **API info** for the template you want to send.

</details>


## Get a template by ID and version

#### Method

<details>
<summary>
Click here to expand for more information.
</summary>

```python
response = notifications_client.get_template_version(
    'template_id',
    1   # integer required for version number
)
```
</details>

#### Response

If the request is successful, `response` will be a `dict`. 
<details>
<summary>
Click here to expand for more information.
</summary>

```python
{
    "id": "template_id", # required
    "type": "sms" | "email" | "letter", # required
    "created_at": "created at", # required
    "updated_at": "updated at", # required
    "version": "version", # integer required
    "created_by": "someone@example.com", # email required
    "body": "Body of the notification", # required
    "subject": "Subject of an email or letter notification, or None if an sms message"
}
```

Otherwise the client will raise a `HTTPError`:

|`error.status_code`|`error.message`|
|:---|:---|
|`404`|`[{`<br>`"error": "NoResultFound",`<br>`"message": "No Result Found"`<br>`}]`|

</details>

#### Arguments

<details>
<summary>Click here to expand for more information.</summary>

##### `template_id`

Find by clicking **API info** for the template you want to send.

##### `version`

The version number of the template.

</details>

## Get all templates

#### Method

This will return the latest version for each template. 
<details>
<summary>
Click here to expand for more information.
</summary>


```python
response = notifications_client.get_all_templates(
    template_type=None # optional
)
```

[See available template types](#template_type)

</details>

#### Response

If the request is successful, `response` will be a `dict`. 
<details>
<summary>
Click here to expand for more information.
</summary>

```python
{
    "templates" : [
        {
            "id": "template_id", # required
            "type": "sms" | "email" | "letter", # required
            "created_at": "created at", # required
            "updated_at": "updated at", # required
            "version": "version", # integer required
            "created_by": "someone@example.com", # email required
            "body": "Body of the notification", # required
            "subject": "Subject of an email or letter notification, or None if an sms message"
        },
        {
            ... another template
        }
    ]
}
```

If no templates exist for a template type or there no templates for a service, the `response` will be a `dict` with an empty `templates` list element:

```python
{
    "templates" : []
}
```

</details>

#### Arguments

<details>
<summary>Click here to expand for more information.</summary>

##### `template_type`

If omitted all messages are returned. Otherwise you can filter by:

- `email`
- `sms`
- `letter`

</details>

## Generate a preview template

#### Method

<details>
<summary>
Click here to expand for more information.
</summary>


```
response = notifications_client.post_template_preview(
    'template_id',
    personalisation={'name': 'chris'}
)
```

</details>

#### Response

If the request is successful, `response` will be a `dict`. 
<details>
<summary>
Click here to expand for more information.
</summary>


```python
{
    "id": "notify_id", # required
    "type": "sms" | "email" | "letter", # required
    "version": "version", # integer required
    "body": "Body of the notification", # required
    "subject": "Subject of an email or letter notification, or None if an sms message"
}
```

Otherwise the client will raise a `HTTPError`:

|`error.status_code`|`error.message`|
|:---|:---|
|`400`|`[{`<br>`"error": "BadRequestError",`<br>`"message": "Missing personalisation: [name]"`<br>`}]`|
|`400`|`[{`<br>`"error": "NoResultFound",`<br>`"message": "No result found"`<br>`}]`|

</details>

#### Arguments

<details>
<summary>Click here to expand for more information.</summary>

##### `template_id`

Find by clicking **API info** for the template you want to send.

##### `personalisation`

If a template has placeholders, you need to provide their values, for example:

```python
personalisation={
    'first_name': 'Amala',
    'reference_number': '300241',
}
```

</details>
