What if you want to send one message to many receivers?

## SNS
Pub / Sub - SNS Topic

Subscribers can be:
- SQS
-  HTTP / HTTPS (with delivery retries – how many times)
- Lambda
- Emails
- SMS messages
- Mobile Notifications

Publisher can be:
- CloudWatch (for alarms)
- Auto Scaling Groups notifications
- Amazon S3 (on bucket events)
- CloudFormation (upon state changes => failed to build, etc)

## SNS + SQS: Fan Out

![Alt text](../images/sns+sqs.png?raw=true)

It is a classic model

- Push once in SNS, receive in many SQS 
- Fully decoupled
- No data loss
- Ability to add receivers of data later
- SQS allows for delayed processing
- SQS allows for retries of work
- May have many workers on one queue and one worker on the other queue