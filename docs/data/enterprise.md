<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->

#
<!--<h2 style="box-shadow: 0px 4px 0px 0px #233c68;">:Enterprise</h2>-->
###:Enterprise
***

Enterprise node is not only limited to user-specific and device-specific information but also enterprise-specific information, mainly the risk profile associated with the SME (digitally dependent, digital enabler, and digitally based). These nodes are used as a metric to calculate GEIGER aggregate score. Detailed content described Section 3.1.4.2 in D1.2.
```
:Enterprise{
	"name"=[
		en=>"University of Aplied Sciences Northwestern Switzerland",
		de=>"Fachhochschule Nordwestschweiz"
	],
      "associatedProfiles"=["Digitally based" or "Digitally enabler” or “digitally dependent”]
      "location"=["UUID of location"],
      "sector"=[Example],
```

###:Enterprise:Users:{User_UUID}
``` 
:Enterprise:users:{User_UUID}{
	"name"=["Example"],
	"firstname"=["Loredana"],
    "UUIDs"=["44e128a5-ac7a-4c9a-be4c-224b6bf81b20,44e128a5-ac7a-4c9a-be4c-224b6bf81b21"],
    "mainUser"=["1"],
    "KnowledgeLevel"=["1"]
}
```