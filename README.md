# Xeed to Insight Communication Protocol
## Introduction
The protocol describes how data is pushed from any Xeed Agent to Insight Module
## Communication Channel / Limits
* All the data is transfered by Message Service: Kafka, Google Pubsub, etc..
* Each message body (after compression) shouldn't exceed 1MB.
* The initial load can hold the message with > 1MB data by using storage links.
## Message Structure
### Header
#### Definition
* table_id: Source identification. (Unique per topic)
* start_seq: Data Stream Hitory ID.
*
