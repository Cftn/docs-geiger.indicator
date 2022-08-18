<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->


#
<!--<h2 style="box-shadow: 0px 4px 0px 0px #233c68;">:Local</h2>-->
###:Local
*** 
The “:Local” node is the UUID of the currently logged-in user and the current device in use. The GEIGER indicator uses UUID on node path to get, create and update node data. It is certain that some users might have more than one device paired with their GEIGER account, a score is calculated for each device, however detailed GEIGER threats scores are only shown for the current device in use.
```
:Local{
	currentUser=["44e128a5-ac7a-4c9a-be4c-224b6bf81b20"],
	currentDevice=["123e4567-e89b-42d3-a456-556642440000"]
}
```

###:Local:plugin
The “:Local: plugin” node contains the name of plugin, company. The plugin creates this node to LDS using GEIGER API. The indicator access sensor value nodes used this information (e.g., company UUID).  
```
:Local:plugin:$CompanyUUID{ 
            name={en=>"ATOS RAE"},
            company={en=>"ATOS"}
} 
```