# LogP: HTTP scheme Exchanging application logs   

**LogP** is an HTTP scheme for exchanging application logs.Currently LogP supports JSON format as data exchange format, but it can be easily converted to other formats.

Current Version : **dev0.1**

**LogP** design respects multinode architectures for data exchange, it considers three main component parts.
These components use **GET** and **POST** for sending and recieving **LogP** data.

* **Sender**
* **Reciever**
* **LogBase**

##1.Sender 
Sender **POST** logs from application to **Reciever** node.
LogP data format consists of the following parts : 

**1.type**:  a standard type string identify type of logs, **VERBOSE**, **INFO**, **CRITICAL**, **DEBUG** ,**ERROR**

**2.tag**:  a string to add checkpoint for reference.

**3.message**: a string data part where details are given.

**4.timestamp**: a unix timestamp to track log generate time.

**5.clientId**: a unique application client/sender id string  that genrated the log.

**6.extra**: an optional part for adding extra related data

example data format : 
```json
{
"type" : "VERBOSE",  
"tag" : "login",  
"message" : "button clicked",  
"timestamp" : 1462015096,
"clientId": "0093b52e-f7e9-40f9-b430-e45efd334197"
}
```
Sender log data also includes LogP version and content-type HTTP header.

##2.Reciever
Reciever component accepts LogP data send from clients/senders, and resolves the data.This part deals with storing LogP data using **LogBase**.Checking for LogP version and data validation is 
done in reciever part.  

Reciever accepts only **POST** requests, and deals with LogBase transparently.  

Standard Http response codes:  

**HTTP 200** : Recieving and Log data request success  

**HTTP 400** : Bad request, donot have standard LogP data format  

##3.LogBase
LogBase accepts **GET** requests, data from **Reciever** is stored/managed by **LogBase**.

