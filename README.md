# Xeed to Insight Communication Protocol
## Introduction
The protocol describes how data is pushed from any Xeed Agent to Insight Module
## Communication Channel / Limits
* All the data is transfered by Message Service: Kafka, Google Pubsub, etc..
* Each message data body (after compression) shouldn't exceed 1MB.
* The huge message should be sent by the use of message file body.
## Message Structure
### Header (key-value pairs)
#### Definition
* table_id: Source identification. (Unique per topic)
* start_seq: Data Stream Hitory ID.
* age: Data sequence within a single history ID. 
  * age = 1 => Header
* end_age: End of age of the message (when not set, end_age = age par default)
#### Use cases / Common agreements
* Strong Data Consistency: streaming data must start by age = 2 and without any gap during the whole life-cycle.
  * Message File body: please leave enough age room for big file.
  * For example: age = 100 and end_age = 1100 for a file of 500 MB ( *2 is a safe multiplificateur) 
* Data lifecycle: A new message header with age = 1 and a newer start_seq indicates an end-of-life the old history

### Message Body (json string)
#### Definition
* attribute 'data_encode'
  * b64g : base64-encoded gzipped content
  * flat : plein-text
* attribute 'data_format'
  * record : list of dictionary
* attribute 'data_specification'
  * standard :
  * slt :
* attribute 'data' : data in the content
* attribute '
