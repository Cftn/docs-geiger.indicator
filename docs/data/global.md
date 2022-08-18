<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->

#
<h2 style="box-shadow: 0px 4px 0px 0px #233c68;">:Global</h2>

The “Global data” contains sub-nodes with globally available information (e.g., a common threat list). It is fed by Local Data Storage or 
the cloud connector depend on the GEIGER toolbox option either Local only or Cloud. More information regarding the “Global Data” and the 
motivation behind the need of acquiring such data for calculating the GEIGER score can be found in [Deliverable D1.2]().

This figure describes how to design the `GEIGER threat` from threats list. The concept of GEIGER risk score is based on threats so the designing threats is 
important. The GEIGER threat designed by cyber security organzation it would be delivered by CERT. 
So, the mapping is required between `normal threats` and `GEIGER threat`. The `GEIGER threat` defined on `:Global:profiles`.
The external plugin (e.g, anti virous tool, educational tool etc.) have to send the validated threat uuid on their metric data refer `:Global:threat` and `:Global:profiles`.   

<img width="600" alt="geiger-threat" src="https://user-images.githubusercontent.com/15152117/184838984-cb99901c-8161-460f-8932-43a98c090681.png">

***    

###:Global:threat
The “:Global:threat” node includes the name, type of threat and description. The GEIGER global threat is unique designed by GEIGER cloud. It is 12 now (16,Aug,2022).
Therefore, the geigerThreat can be same and duplicate with other nodes. But, UUID of GEIGER global threat defined on 'Global:profiles' node.
```
:Global:threats:80efffaf-98a1-4e0a-8f5e-th89388368cj{[
		"name"=[en=>"Cryptojacking"],
		"geigerThreat"=[en=>"Malware"],
		"description"=[en=>"Threat description"]
]}
```

###:Global:profiles
The “:Global:profiles” node includes the risk profile weights for each different SME profile (digitally dependent, digital enabler, and digitally based). The risk profile weights are used as a metric to calculate GEIGER total score.
```
:Global:Profiles:3e88c7b7-5bdb-4503-b963-f36333e0224f:{ 
        "name":["Digitally based" or "Digitally enabler” or “digitally dependent”], 
        "threat1_UUID"=["0.1"], "threatN_UUID"=["0.8"]
}
```

***
###:Global:recommendation
The “:Global:recommendation” node has two main cyber-systems ‘User’ and ‘Device’ and it outputs a score and recommendations threat in each cyber system. The indicator refers to this node when calculating GEIGER recommendation nodes for each threat. Detailed content described Section 3.1.3.4 in D1.2.
```
:Global:Recommendations:e430cb4e-dcf4-4169-b0d3-94ea3a3df528{ 
      short=["short desciption"], 
      long=["long description"], 
      Action=["geiger://mainRecommender/12371284376t21"], 
      relatedThreatsWeights=["threat1_UUID,Low;...;threatN_UUID,Meduim"],
      costs=["False"], 
      RecommendationType=["Device"]
}
```