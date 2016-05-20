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

###ClientId Generation   
`clientId` generation must be done on `remote node` other than `client node`.By generating `clientId`, we can minimise extra data sending over LogP, that is uniq to client.Consider an example of an android app, the OS version or build number is data part that is uniq to device, so instead of sending this data over LogP, send this data to generate a uniq `clientId`.

example data format : 
```js
{
// type canbe custom not restricted
"type" : "VERBOSE", 
// tags are to identify the event trigger or 
// an application screen or any other 
"tag" : "login",  
// then event details or something to be logged
"message" : "button clicked", 
// a unix timestamp
"timestamp" : 1462015096,
// this part can be any type of unique id
// this case uses a uuid4 string
"clientId": "0093b52e-f7e9-40f9-b430-e45efd334197" 
}
```
Sender log data should include LogP version and content type headers.  
```  
Content-Type : application/json  
X-LogP-Version : dev0.1  
```
##2.Reciever
Reciever component accepts LogP data send from clients/senders, and resolves the data.This part deals with storing LogP data using **LogBase**.Checking for LogP version and data validation is done in reciever part.  

Reciever accepts only **POST** requests, and deals with LogBase transparently.  

Standard Http response codes:  

**HTTP 200** : Recieving and Log data request success  

**HTTP 400** : Bad request, donot have standard LogP data format  

##3.LogBase
LogBase accepts **GET** requests, data from **Reciever** is stored/managed by **LogBase**.  
Using LogBase, clients can search or filter any logs based on the conditions given.
**LogP** simplifies the search in a specified URL scheme.  
  

**type** : {LogBase URL}/?type=typename  
**tag** : {LogBase URL}/?tag=tagname  
**clientId** : {LogBase URL}/?clientId=clientidstring  
**message** : {LogBase URL}/?message=string  
