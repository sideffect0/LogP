# LogP: HTTP scheme Exchanging application logs   

**LogP** is an HTTP scheme for exchanging application logs.Currently LogP supports JSON format as data exchange format, but it can be easily converted to other formats.

Current Version : **dev0.1**

**LogP** design respects multinode architectures for data exchange, it considers three main component parts.

* **Sender**
* **Reciever**
* **Database**

common data format : 
```json
{
"type" : "verbose",  
"tag" : "login",  
"message" : "button clicked",  
"timestamp" : "1462015096"
}
```
