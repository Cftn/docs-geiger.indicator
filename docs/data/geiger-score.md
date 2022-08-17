#
<h2 style="box-shadow: 0px 4px 0px 0px #233c68;">GEIGER Score Nodes</h2>

###GeigerScoreUser
:Users:{userUUID}:{indicatorUUID}:data:GeigerScoreUser
```
:Users:772df149-0226-450b-8c7a-de938dfaea03:1234-1234-1234:data:GeigerScoreUser{
   GEIGER_score={en=>"47.78946694003076"}, 
   threats_score={en=>"threatUUID,score; …. "}, 
   number_metrics={en=>"7"}, 
   implementedRecommendations={en=>""}
}
```

###GeigerScoreDevice
:Devices:{userUUID}:{indicatorUUID}:data:GeigerScoreDevice  
```
: Devices:92ebe4bf-65d2-4106-8445-0a539fc85ec9:1234-1234-1234:data: GeigerScoreDevice{
   GEIGER_score={en=>"62.25655561250642"}, 
   threats_score={en=>"threatUUID,score; …. "},
   number_metrics={en=>"5"}, 
   implementedRecommendations={en=>""}
}
```

###GeigerScoreAggregate
:Users:{userUUID}:{indicatorUUID}:data:GeigerScoreAggregate
```
:Users:772df149-0226-450b-8c7a-de938dfaea03:1234-1234-1234:data:GeigerScoreAggregate{
   GEIGER_score={en=>"47.78946694003076"}, 
   threats_score={en=>"threatUUID,score; …. "}, 
   number_metrics={en=>"10"}, 
}
```

###GeigerSMEScore
:Users:{userUUID}:{indicatorUUID}:data:GeigerSMEScore
```
:Users:772df149-0226-450b-8c7a-de938dfaea03:1234-1234-1234:data:GeigerSMEScore{
  associatedProfiles={en=>"digitally based"}, 
  GEIGER_score={en=>"59.18799727912817"}, 
  location={en=>"ce82b724-b62a-4mse-9loc-aa7c4755e6CH"}, 
  sector={en=>"Hair-Dresser"}
}
```