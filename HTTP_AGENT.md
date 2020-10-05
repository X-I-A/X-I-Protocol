# Xeed HTTP Agent Protocol
## Introduction
It happens that data output stream in message format is a little awkward to develop. HTTP flow might a much simpler solution. 
This protocol defines the HTTP flow accepted by Xeed Http Agent. Xeed Http Agent do the job to push the stream to Insight Module (message service)
## Restiful Message Ressource Definition
* Kafka : /clusters/<cluster_name>/topics/<topic_name>
* Google Cloud Platform: /projects/<project_name>/topics/<topic_name>
## HTTP Header common requirements
* Content-Type : Accepted value: text/plain
* Content-Encoding : Accepted value: gzip / It is also possible to send the request without compression
## HTTP Header Xeed related definitions
The same as the message header of [X-I-Protocol](README.md) with the following adaptation:
* Prefix of 'XEED-'
* Using '-' instead of '_'.
* Example: 'XEED-DATA-ENCODE' insteal of 'data_encode'. Because the content-type is text/plain. 'XEED-DATA-ENCODE' must be 'flat'
## HTTP Body
Exactly the same as message body of [X-I-Protocol](README.md)
