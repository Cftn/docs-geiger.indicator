#

<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">GEIGER Recommendation Nodes</h3>

The indicator suggests recommendations as checking impact between threat and recommendation. The user can select to accept the recommendation or not to determine the Implemented recommendation.

###User Recommendations
:Users:{userUUID}:{indicatorUUID}:data:recommendations
```
:Users:1ec418f3-f581-4ad6-9005-78ba8acd0552:1234-1234-1234:data:recommendations{
   threatUUID={en=>"recommendationUUID,Impact;…"}, 
   80efffaf-98a1-4e0a-8f5e-gr89388354sp={en=>"123e4567-e89b-42d3-a4us-556642440rec-003,1.0;123e4567-e89b-42d3-a4us-556642440rec-019,0.1;123e4567-e89b-42d3-a4us-556642440rec-006,1.0;123e4567-e89b-42d3-a4us-556642440rec-001,1.0;"},
…
}
```

###Device Recommendations
:Devices:{userUUID}:{indicatorUUID}:data:recommendations
```
:Devices:92ebe4bf-65d2-4106-8445-0a539fc85ec9:1234-1234-1234:data:recommendations{
   threatUUID={en=>"recommendationUUID,Impact;…"}, 
   80efffaf-98a1-4e0a-8f5e-gr89388353wa={en=>"123e4567-e89b-42d3-a4dv-556642440rec-027,1.0;123e4567-e89b-42d3-a4dv-556642440rec-002,0.5;"}, 
…
}
```
