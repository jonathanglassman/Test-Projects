# API architecture

Sending a text message

send_sms_notification: Service makes an API Post of send_sms_notification to Notify. State: ???

SMS delivery: Notify accept the message and send it to delivery providers for delivery to recipient. State: Sending

SMS delivery receipt: Notify has successfully delivered a message to user’s email inbox or phone. Notify won’t tell you if a user has opened or read a message. _QP: Contradicts diagram where it says delivery receipt?_ State: Delivered

_QP: Where does Sent Internationally fit_

API Response: _QP: Need more information. Is the state Delivered?_ State: Delivered

_QP: need help on this!_
