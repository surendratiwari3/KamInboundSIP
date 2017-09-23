# KamInboundSIP

KamInboundSIP is an Open Source VoIP Inbound DID Call Routing Solution.This Project will provide the inbound sip using that we can route did call to customer ip or customer Phone number
It is available with the following features

1. Inbound Termination with Carrier IP validation.
2. Carrier LCR for DID/TFN to PSTN forwarding.
3. Inbound Abuse Block.
4. CDR in MongoDB.
5. IPTables Block for Sip-Scanners.
6. Integration with RtpEngine.
7. RedisDB for quick DB Access.

This solution is well tested using SIPP with 50 cps. The backbone of this router is RedisDb which provides quick access to resources for validation of routing content.
It hence reduces the latency and overhead of other sql database engines. The cdr in recorded in MongoDB (nosql database).


### Installation

## Step 1: Setup a Redis Instance and build Redis Structure as below :

###The DID / TFN number key that holds its information  
```
 HMSET did:<DID/TFN> custId <custId> carrierId <carrierId> countryId <countryId> billType <billType> channel <channel> rtpEngine <1/0> routingType 1
``` 

###The Carrier IP address as given by the carrier will be stored in a redis set.   
```
 SADD carrierIPAddrs:<carrierId> <IPAddress1> <IPAddress_n> 
``` 

###How is the inbound TFN/DID routed
```
 HMSET direct:<DID/TFN> dstUri <DID/TFN> dnis <DID/TFN>
```
eg. HMSET direct:<DID/TFN> dstUri <IP:PORT> dnis <DID/TFN/Entity_on_dstUri>
 
####The customer in system to whom the DID/TFN belongs 
``` 
 HMSET customer:<custId> routeId <routeId> balance 700
``` 

###The Outbound Carriers will be stored in a Z set ; this gives us control to have priority control.
```
 ZADD routes:<routeId>:0 <priority> <out_carrier_id> 
```

###The outbound Carrier IP addresses to route the call out
```
 SADD carrierOutIps:<out_carrier_id> <IP:PORT>
```

###The Rate Card of the carrier as given by the carrier itself; can be a dialcode or entire number (Incase a priority needs to be defined)
```
 HMSET carrierRates:<out_carrier_id>:0 <dialcode> <cost>
```

###Inbound Numbers are often abused by callers. A set in Redis corresponding to the TFN/DID will contain such numbers
```
 SADD abuse:<called_number> <caller_id>
```

## Step 2 : Edit the vars.cfg file for your Environment
```
#----------------------- Custom Defined Variables ----------------------------#
#!substdef "!MY_IP_ADDR!192.168.2.46!g"
#!substdef "!MY_OB_PORT!5060!g"
#!define DBURL "mongodb://192.168.2.46:27017/kamailio"
#!define REDIS "name=redis;addr=127.0.0.1;port=6379;db=0"
#!define RTPENGINE "udp:192.168.2.46:25061"
#!define MONGOPATH "name=mgs1;uri='mongodb://192.168.2.46:27017/kamailio'"
```

## Step 3 : Initialise MongoDB Instance
 
1. Create a database with name kamailio
2. Create collection version and put the below values

```
/* 1 */
{    
    "table_name" : "location",
    "table_version" : 8
}

/* 2 */
{
    "table_name" : "dialog",
    "table_version" : 7
}

/* 3 */
{
    "table_name" : "dialog_vars",
    "table_version" : 1
}

```

#### This makes the Proxy Ready for Use
#### Note : if you find any difficulty while using KamInboundSIP please let us know at surendratiwari3@gmail.com
## We are Working on Web part we will update the frontend repository soon.
