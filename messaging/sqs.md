## Why do we need messaging?

Sometimes we have application which one service talks to another service.

Normally it is sync communication, but sometimes we don't need to the downstream service to handle immediately.

The downstream service can handle it in a async/events way. For exmple, the buying service and shipping service.
Shipping service can have a queue on what orders to ship.

So in that sense, it is actually to decouple the app.

what are the models in aws?
- using SQS: queue model
- using SNS: pub/sub model
- using Kinesis: real-time streaming model

Why decoupling is better? because those services can scale independently.

## SQS (it scales automatically)
- Standard Queue
   - Oldest offering (over 10 years old)
   - Fully managed
   - Scales from 1 message per second to 10,000s per second
   - Default retention of messages: 4 days, maximum of 14 days
   - No limit to how many messages can be in the queue
   - Low latency (<10 ms on publish and receive)
   - Horizontal scaling in terms of number of consumers
   - Can have duplicate messages (at least once delivery, occasionally) • Can have out of order messages (best effort ordering)
   - Limitation of 256KB per message sent 

- Delay Queue
    - Delay a message (consumers don’t see it immediately) up to 15 minutes • Default is 0 seconds (message is available right away)
    - Can set a default at queue level
    - Can override the default using the DelaySeconds parameter
    
## Flow Diagram

![Alt_text](../images/sqs.png?raw=true)     

- Long Polling (it can reduce the cost by eliminating empty responses)
    - When a consumer requests message from the queue, it can optionally “wait” for messages to arrive if there are none in the queue
    - Benefit: LongPolling decreases the number of API calls made to SQS while increasing the efficiency and latency of your application.
    - The wait time can be between 1 sec to 20 sec (20 sec preferable)
        - **ReceiveMessageWaitTimeSeconds**
    - Long Polling is preferable to Short Polling
    - Long polling can be enabled at the queue level or at the API level using WaitTimeSeconds
    
- DLQ
    - a message failed to be processed within the Visibility Timeout, then it gets back to the queue
    - if the threshold exceeds the times of a message going back to the queue, then it goes into DLQ    
    
- Visibility Timeout (it gives consumers more time to process the message)   
    - Set between 0 seconds and 12 hours (default 30 seconds)
    - cannot be too high or too low
    - ChangeMessageVisibility API to change the visibility while processing a message 
    - DeleteMessage API to tell SQS the message was successfully processed

### Short Polling

Amazon **SQS uses short polling by default**, querying only a subset of the servers (based on a weighted random distribution) to determine whether any messages are available for inclusion in the response. 

**Short polling works for scenarios that require higher throughput**. However, you can also configure the queue to use Long polling instead, to reduce cost.
    
    
## FIFO Queue

beside std, it is first in -first out

- FIFO queues support up to 3000 messages per second, each message is delivered exactly once, and **message order is preserved**. 
- FIFO queues are designed to enhance messaging between applications when the order of operations and events is critical, or where duplicates can't be tolerated.    

- **content based deduplication and deduplication id**

- Order
    - standard: no ordering
    - fifo: if not use Group ID, then messages are consumed in the order they are sent, with only one consumer
        - if use group ID, then you can scale the number of consumers, similar to Partition Key in Kinesis

