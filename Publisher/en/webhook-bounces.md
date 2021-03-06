# Webhooks: bounces

The Marketing Suite normally modifies the envelope address of all mails that flow
through it to track bounces and other messages that are sent back. All
bounce messages are therefore sent back to the Marketing Suite. You can set 
up a "bounce" Webhook to be notified about these bounces too with an 
HTTP(S) POST call.

If you are only interested in failed deliveries then you can also use 
[Webhooks for failures](webhook-failures).

## Type of messages

The bounce Webhook is used for literally **all** messages that are 
sent back to the envelope address. This includes the regular
delivery status notifications, but also out-of-office replies, vacation
mails, or error messages from servers that do not respect the official
format for bounce messages. All these type of messages are sent back to
Copernica and with a Webhook you can also receive these messages in real-time.

## Variables

With each POST call the variables in the table below are sent cover. The 
POST data is sent with the application/x-www-form-urlencoded content type.

Associative arrays such as "parameters" and "fields" are sent per key-value pair,
e.g. *parameters[key]=value*.
Arrays such as "interests" are sent per item, e.g. *interests[]=xyz*.

| Variable  | Description                                                              |  
|-----------|--------------------------------------------------------------------------|
| id        | Unique message ID with which the bounce is associated                    |
| type      | Type of action that triggered the Webhook ('bounce')                     |
| timestamp | Timestamp for the bounce (in YYYY-MM-DD HH:MM:SS format)                 |
| time      | Unix time for the bounce                                                 |
| recipient | Email address to which the original mail was sent                        |
| envelope  | Envelope address to send the bounce to                                   |
| mime      | The MIME data that was sent (the message itself)                         |
| tags      | The tags that you associated with the mail                               |

The 'id", 'recipient' and 'tags' variables allow you to link the click to the '
originally sent email message.

## More information
 
* [Webhooks](./webhooks)
* [Types of bounces](./bounces)
