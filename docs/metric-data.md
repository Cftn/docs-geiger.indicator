<h2>GEIGER Data</h2>

The LDS listener and GEIGER score nodes create on initial work before check or wait the metric data. Table 1 shows nodes necessary to calculate GEIGER scores. The nodes contain each data as below.

<!--##:Local </br>-->
<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">:Local</h3>
The “:Local” node is the UUID of the currently logged-in user and the current device in use. The GEIGER indicator uses UUID on node path to get, create and update node data. It is certain that some users might have more than one device paired with their GEIGER account, a score is calculated for each device, however detailed GEIGER threats scores are only shown for the current device in use.
```
:Local{
	currentUser=["44e128a5-ac7a-4c9a-be4c-224b6bf81b20"],
	currentDevice=["123e4567-e89b-42d3-a456-556642440000"]
}
```

**:Local:plugin** </br>
The “:Local: plugin” node contains the name of plugin, company. The plugin creates this node to LDS using GEIGER API. The indicator access sensor value nodes used this information (e.g., company UUID).  
```
:Local:plugin:$CompanyUUID{ 
            name={en=>"ATOS RAE"},
            company={en=>"ATOS"}
} 
```
***
<!--##:Global</br>-->
<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">:Global</h3>
The “Global data” contains sub-nodes with globally available information (e.g., a common threat list). It is fed by LDS or the cloud connector depend on the GEIGER toolbox option either Local only or Cloud. More information regarding the “Global Data” and the motivation behind the need of acquiring such data for calculating the GEIGER score can be found in Deliverable D1.2. 


**:Global:threat** </br>
The “:Global:threat” node includes the name, type of threat and description. 
```
:Global:threats:80efffaf-98a1-4e0a-8f5e-th89388368cj{[
		"name"=[en=>"Cryptojacking"],
		"geigerThreat"=[en=>"Malware"],
		"description"=[en=>"Threat description"]
]}
```

**:Global:profiles** </br>
The “:Global:profiles” node includes the risk profile weights for each different SME profile (digitally dependent, digital enabler, and digitally based). The risk profile weights are used as a metric to calculate GEIGER total score.
```
:Global:Profiles:3e88c7b7-5bdb-4503-b963-f36333e0224f:{ 
        "name":["Digitally based" or "Digitally enabler” or “digitally dependent”], 
        "threat1_UUID"=["0.1"], "threatN_UUID"=["0.8"]
}
```

**:Global:recommendation** </br>
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
***
<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">:Enterprise</h3>
<!--##:Enterprise-->
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


**:Enterprise:Users:{userUUID}** </br>
``` 
:Enterprise:users:{User_UUID}{
	"name"=["Example"],
	"firstname"=["Loredana"],
      "UUIDs"=["44e128a5-ac7a-4c9a-be4c-224b6bf81b20,44e128a5-ac7a-4c9a-be4c-224b6bf81b21"],
      "mainUser"=["1"],
      "KnowledgeLevel"=["1"]
}
```
<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">:Devices</h3>
**:Devices:{deviceUUID}** </br>
```
:Devices:{Device_UUID}{
   name={en=>"Samsung Mobile Ana"}, 
   type={en=>"mobile"}, 
   owner={en=>"328161f6-89bd-49f6-user-5a375ff56ana"}
}
```

<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">:Senser Values</h3>

**:Devices:{deviceUUID}:{companyUUID}:data:metrics:{metricUUID}** </br>
```
:Devices:device_UUID:company_UUID:data:metrics:metric_UUID{
  name=["numberOfInstalledApps"],
  minValue=["0"],
  maxValue=["200"],
  value:Geigervalue=["65"],
  valueType=["int"],
  relation=["user"],
  threatsImpact=["afde4567-e89b-e2d3-e456-556642569800","low";"afdd4567-e89b-42d3-a456-556642fe000a","high"],
  flag=["0"]
}
```
**:Users:{userUUID}:{companyUUID}:data:metrics:{metricUUID}**
```
:Users:user_UUID:company_UUID:data:metrics:metric_UUID{
  name=["numberOfInstalledApps"],
  minValue=["0"],
  maxValue=["200"],
  value:Geigervalue=["65"],
  valueType=["int"],
  relation=["user"],
  threatsImpact=["afde4567-e89b-e2d3-e456-556642569800","low";"afdd4567-e89b-42d3-a456-556642fe000a","high"],
  flag=["0"]
}
```
<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">GEIGER Score Nodes</h3>
**:Users:{userUUID}:{indicatorUUID}:data:GeigerScoreUser**
```
:Users:772df149-0226-450b-8c7a-de938dfaea03:1234-1234-1234:data:GeigerScoreUser{
   GEIGER_score={en=>"47.78946694003076"}, 
   threats_score={en=>"threatUUID,score; …. "}, 
   number_metrics={en=>"7"}, 
   implementedRecommendations={en=>""}
}
```
**:Devices:{userUUID}:{indicatorUUID}:data:GeigerScoreDevice**
```
: Devices:92ebe4bf-65d2-4106-8445-0a539fc85ec9:1234-1234-1234:data: GeigerScoreDevice{
   GEIGER_score={en=>"62.25655561250642"}, 
   threats_score={en=>"threatUUID,score; …. "},
   number_metrics={en=>"5"}, 
   implementedRecommendations={en=>""}
}
```
**:Users:{userUUID}:{indicatorUUID}:data:GeigerScoreAggregate**
```
:Users:772df149-0226-450b-8c7a-de938dfaea03:1234-1234-1234:data:GeigerScoreAggregate{
   GEIGER_score={en=>"47.78946694003076"}, 
   threats_score={en=>"threatUUID,score; …. "}, 
   number_metrics={en=>"10"}, 
}
```
**:Users:{userUUID}:{indicatorUUID}:data:GeigerMSEScore**
```
:Users:772df149-0226-450b-8c7a-de938dfaea03:1234-1234-1234:data:GeigerMSEScore{
  associatedProfiles={en=>"digitally based"}, 
  GEIGER_score={en=>"59.18799727912817"}, 
  location={en=>"ce82b724-b62a-4mse-9loc-aa7c4755e6CH"}, 
  sector={en=>"Hair-Dresser"}
}
```
<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">GEIGER Recommendation Nodes</h3>
The indicator suggests recommendations as checking impact between threat and recommendation. The user can select to accept the recommendation or not to determine the Implemented recommendation.

**:Users:{userUUID}:{indicatorUUID}:data:recommendations**
```
:Users:1ec418f3-f581-4ad6-9005-78ba8acd0552:1234-1234-1234:data:recommendations{
   threatUUID={en=>"recommendationUUID,Impact;…"}, 
   80efffaf-98a1-4e0a-8f5e-gr89388354sp={en=>"123e4567-e89b-42d3-a4us-556642440rec-003,1.0;123e4567-e89b-42d3-a4us-556642440rec-019,0.1;123e4567-e89b-42d3-a4us-556642440rec-006,1.0;123e4567-e89b-42d3-a4us-556642440rec-001,1.0;"},
…
}
```
**: Devices:{userUUID}:{indicatorUUID}:data:recommendations**
```
:Devices:92ebe4bf-65d2-4106-8445-0a539fc85ec9:1234-1234-1234:data:recommendations{
   threatUUID={en=>"recommendationUUID,Impact;…"}, 
   80efffaf-98a1-4e0a-8f5e-gr89388353wa={en=>"123e4567-e89b-42d3-a4dv-556642440rec-027,1.0;123e4567-e89b-42d3-a4dv-556642440rec-002,0.5;"}, 
…
}
```
