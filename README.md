# Xeed to Insight Communication Protocol
## Introduction
The protocol describes how data is pushed from any Xeed to Insight flow and Insight to Agent flow.
* Xeed Module can accept all encoder, format and data specification input flow
* This protocol onlys talks about the Xeed Module's outflow
## Communication Channel / Limits
* All the data is transfered by Message-like Service: Kafka, Google Pubsub, etc..
* All data should be record format (json array)
* Each message data body (after compression) shouldn't exceed 1MB.
* Big message can also be sent directly by files. Each file shouldn't handle more than 128 MB Raw data (before compression).
* The above two sizing recommendations are used to meet the cloud based serverless architecture (Highly distributed small service end-point).
## Message Structure
### Header (key-value pairs)
#### Definition (1/2) : Identification
* table_id: Source identification. (Unique per topic)
* start_seq: Data Stream Hitory ID.
* aged: True = Strong Data Consistency / False = Weak Data Consistency
* age: Data sequence within a single history ID. 
  * age = 1 means the record contains header information
* end_age: End of age of the message (when not set, end_age is considered equal to age)
#### Data Consistency / Lifecycle
* Strong Data Consistency: streaming data must start by age = 2 and without any gap during the whole life-cycle.
  * Natural growing up garanteed (age by age)
  * File Type Message body: please leave enough age room for big file.
  * For example: age = 100 and end_age = 1100 for a file of 500 MB ( *2 is a safe multiplificateur) 
  * Data lifecycle: A new message header with age = 1 and a newer start_seq indicates an end-of-life the old history
* Weak Data Consistency: No age specified (except for header of which age must be equal to 1).
  * Data operation is sorted by start_seq of the same key (key definition could be found at the header)
  * Obsolete operations will not be executed. (same key but a bigger start_seq already exists)
#### Definition (2/2) : Data Description
* attribute 'data_encode'
  * gzip : gzipped byte array
  * b64g : base64-encoded gzipped char array 
  * flat : plein-text
  * blob : binary data
* attribute 'data_format'
  * record : json - list of dictionary
* attribute 'data_store'
  * body : message body
  * file : uri of single file
* attribute 'data_spec': x-i-a
### Data Spec x-i-a Body part
* Data field type is described in the header
* Here is the definition of the case of data_spec = 'x-i-a' (Standard Data Type) 
* Standard Data Body: internal fields might be added for each line:
  * '_AGE' : age value for strong data consistency
  * '_SEQ' : start_seq value for weak data consistency
  * '_NO' : The Sequence number of the data entries of the same merge key. Empty = Order is not important
  * '_OP' : Operation Flag. 'I' = Insert, 'U' = Update, 'D' = Delete, Empty = Insert without a predefined Order (_NO)
