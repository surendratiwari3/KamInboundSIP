# KamInboundSIP
This Project will provide the inbound sip using that we can route did call to customer ip or customer Phone number

KamInboundSIP - Open source Inbound DID Call Routing

KamInboundSIP is an Open Source VoIP Inbound DID Call Routingsolution for Freeswitch. It supports call forwarding to
customer ip and customer phone number. It also provides many other features such as integration with rtpengine,CDR in mongodb,
blocking the attacker,LCR for carriers.

This solution is well tested using sipp with 50 cps.in this solution we have used redis to get the routing details so we are 
actually removed the latency and overhead of other sql database engine.we are storing the cdr in mongodb (nosql database).

we have used iptabeles to block the attackers.

# Installation

## Step 1: Setting up the Redis Keys and Redis Script for KamInboundSIP 


## Step 2 : Setting up the kamailio cfg

Copy the all files in your kamailio directory.set the redis url and mongodb url,rtpengine socket in cfg.restart the kamailio.


