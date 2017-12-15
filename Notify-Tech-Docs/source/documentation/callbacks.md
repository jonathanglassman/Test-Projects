# Callbacks

Received text messages can be forwarded to a URL that you specify, using the callback feature. Messages are forwarded as they are received.

To use this functionality, you must:

- provide a bearer token
- enable "receive text messages" for your service

You need to provide a bearer token to allow the API notifications client access to your service. This token goes in the authorisation header of the callback requests.

Contact the GOV.UK Notify team on the [support page](https://www.notifications.service.gov.uk/support) or through the [slack channel](https://govuk.slack.com/messages/C0AC2LX7E) to enable "receive text messages" for your service. Once it has been set up:

1. Log into your GOV.UK Notify account.
1. Go to Settings.
1. Check that you are in the correct service; if you are not, click Switch service in the top right corner of the screen and select the correct one.
1. _QP: What next?_

Arguments?

_QP: Need example here to link to table below_

|Key|Description|Format|
|:---|:---|:---|
|id|Notify’s id for the received message|UUID|
|source_number|The phone number the message was sent from|447700912345|
|destination_number|The number the message was sent to (your number)|07700987654|
|message|The received message|Hello Notify!|
|date_received|The UTC datetime that the message was received by Notify|2017-05-14T12:15:30.000000Z|

The callback message is formatted in JSON.
