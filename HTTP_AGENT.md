# Xeed HTTP Agent Protocol
## Introduction
It happens that data output stream in message format is a little awkward to develop. HTTP flow might a much simpler solution. 
This protocol defines the HTTP flow accepted by Xeed Http Agent. Xeed Http Agent do the job to push the stream to Insight Module (message service)
## Restiful Message Ressource Definition
* Kafka : /clusters/<cluster_name>/topics/<topic_name>
* Google Cloud Platform: /projects/<project_name>/topics/<topic_name>
## HTTP Header definition
The same as the message header of [X-I-Protocol](README.md)
