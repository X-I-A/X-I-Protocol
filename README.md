# Xeed to Insight Communication Protocol
## Introduction
The protocol describes how data is pushed from any Xeed Agent to Insight Module
## Communication Channel / Limits
* All the data is transfered by Message Service: Kafka, Google Pubsub, etc..
* Each message data body (after compression) shouldn't exceed 1MB.
* The huge message should be sent by the use of message file body.
## Message Structure
### Header (key-value pairs)
#### Definition (1/2) : Identification
* table_id: Source identification. (Unique per topic)
* start_seq: Data Stream Hitory ID.
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
* attribute 'data_format'
  * record : list of dictionary
* attribute 'data_store'
  * body : message body
  * file : uri of single file
  * files : list of uri
  * path : all file under a path
  * paths : path list
* attribute 'data_spec': internal fields definitions
  * slt : Record Number = '_RECNO', Operation Type = 'IUUTOPERATFLAG', defalut operation = 'Insert'
### Body
* Content as is described in the header
