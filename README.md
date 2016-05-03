# LogP: HTTP scheme Exchanging application logs   

**LogP** is an HTTP scheme for exchanging application logs.Currently LogP supports JSON format as data exchange format, but it can be easily converted to other formats.

Current Version : **dev0.1**

**LogP** design respects multinode architectures for data exchange, it considers three main component parts.

* **Sender**
* **Reciever**
* **Database**

##1.Sender 
Sender sends logs from application to **Reciever** end.LogP data format consists of the following parts

**1.type**:  To identify type of logs, **VERBOSE**, **INFO**, **CRITICAL**, **DEBUG** 

**2.tag**:  To add checkpoint for reference 

**3.message**: data part where details are given

**4.timestamp**: time of log generation, a unix timestamp

**5.clientId**: a unique application node id of log generation

**6.extra**: an optional part for adding extra related data

example data format : 
```json
{
"type" : "VERBOSE",  
"tag" : "login",  
"message" : "button clicked",  
"timestamp" : "1462015096",
"clientId": "0093b52e-f7e9-40f9-b430-e45efd334197"
}
```
