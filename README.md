#Kamailio Proxy

##Customers


##Inbound Numbers
Holds the attributes of an inbound number
```
HMGET phoneNumber:<called_number> customerId 7040 carrierId 1234 countryId 24 billType 1 channels 2 rtpProxied 1 routStrat 1
```
##Inbound Carrier IP Addresses
Set of carrier IP addresses mapped to carried Id in system
```
SADD carrierIPAddrs:<carrierId>
```

##Abuse Check
To Add/Check calling numbers that abuse DIDs/TFNs of Customer
```
SADD abuse:<called_number> <caller_id>
```

##Routing Information of a DID/TFN
Contains routing information of DID/TFN  
```
HMGET directRoute:<called_number> dstUri dnis
```
dstUri=1.2.3.4:5060
dstUri=919890123456
dnis=919890123456
dnis=919890123457
Imp : 1. dnis will always be the number used as a request uri; it can be the TFN/DID incase of IP forwarding 
      2. dnis will always be the number used as a request uri; it can be a other PSTN number incase of PSTN forwarding

##Carrier Rates
Contains Carrier Rates wrt DialCodes
```
HMSET carrierRates:<carrierId> 44 0.44 4412 0.47
```
##Carrier Rates Extra
Contains Carrier Charging data
```
HMSET carrierRatesExtra:<carrierId> initPulse 60 subsPulse 60 connCost 0 
```

##Carrier Currency 
Contains Carrier Currency 
```
HMSET carrierCurrency <carrrierId> <currency> <carrrierId1> <currency1>
```

##Outbound Carrier Endpoints
Outbound Carrier Endpoints
```
SMEMBERS carrierEndpoints:<carrier_id>
```

#### Protect the redis instance 
```bash
iptables -A INPUT -p tcp --destination-port 6379 -j DROP
iptables -A INPUT -s 127.0.0.1 -p tcp -m tcp --dport 6379 -j ACCEPT
```
