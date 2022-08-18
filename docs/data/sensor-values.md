<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->

#
<h2 style="box-shadow: 0px 4px 0px 0px #233c68;">Senser Values</h2>
As explained in D1.2, sensor values related to each cyber system ‘User’ or ‘Device’, either positively or negatively. In other words, the sensor values sent by different tools can either contribute in a positive way and improve the ‘User’ or ‘Device’ score by decreasing the overall risk or contribute negatively and worsen the score. Additionally, each tool sends one or more sensor values, where each value relates to a specific GEIGER threat(s) with a specific impact either low, medium, or high. More information regarding tools and sensor value can be found in D1.2 and D2.1. 

###Device metrics
:Devices:{deviceUUID}:{companyUUID}:data:metrics:{metricUUID}
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

###User metrics
:Users:{userUUID}:{companyUUID}:data:metrics:{metricUUID}
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