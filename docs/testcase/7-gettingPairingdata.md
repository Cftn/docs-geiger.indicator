<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->
 
 <h3 style="box-shadow: 0px 4px 0px 0px #233c68;">Getting pairing data (Devices, Employees)</h3>
                                                                     
 **given**
 > - prepared `Local Data Storage instance` to access Local Data Storage.
 > - registered `specific node (pairing)` to received new data from Local Data Storage.
 
 **when** 
 > Occurring creation or update on node registered subscription by `Storage Listener`.
 
 **then** 
 > - delivered `new data node` to `gotStorageChange` callback function.
 > - checked `new data node` based on each conditions
 > - call `getScoreAll()` function to parsing and calculate risk score. 
 
       ///Received metric data using Storage Listener
       @override
       void gotStorageChange(EventType event, Node? oldNode, Node? newNode) async {
     
           logger.i('new event from =>> ${newNode!.owner} ${newNode.path} ');
     
     
         ///'owner' is ID
         /// Should compare the owner as me
         /// Avoid infinite recursion loop by indicator own
         if(newNode.owner != Types.owner){
     
           ....
              ///get device pairing information
              }else if(newNode.path.contains(':devicesPairing')){
      
                    logger.i('got event devicesPairing => ${newNode.name}');
      
                    await getScoreAll();
                    await crtAggregateScore();
                    await updateScoreNode(Types.userScoreNodePath, Types.geigerScoreAggregate,Types.users);
      
              ///get employee pairing information
              }else if(newNode.path.contains(':employeesPairing')){
      
                    logger.i('got event employeesPairing => ${newNode.name}');
      
                    await getScoreAll();
                    await getEmployeesSharedScore();
                    await crtAggregateScore();
                    await updateScoreMSENode();
      
      
              }
            ....
          }
       }
     
     ------------------------------
     
     ///Preparation Aggregate Score (Total Score) from User, Device Nodes
     ///To check current User, Device Node and Pairing Node (shared my another device score)
     Future getScoreAll() async{
     
       ///Prevent duplication
       CoreValues.aggregate.clear();
       CoreValues.aggregateThreatsScore = '';
     
       ///For shared threat score from another my device score
       var sharedthreatsScoreList = [];
     
       ///For current device my threat scores
       var currentThreatsScoreList = [];
     
       try {
     
         ///Get current all information of device, user risk score
         currentThreatsScoreList = await getCurrentScore();
         logger.d('indiC getScore threatsScoreAggregate getCurrentScore $currentThreatsScoreList');
     
         ///Sum threat risk score from current device, user score
         for(var i = 0; i < CoreValues.geigerThreatUUID.length; i++){
     
           var tempDevice = currentThreatsScoreList[0][i].split(',');
           var tempUser = currentThreatsScoreList[1][i].split(',');
           var score = (double.parse(tempDevice[1])+double.parse(tempUser[1]))/2;
     
           CoreValues.aggregateThreatsScore += '${tempDevice[0]},$score;';
         }
     
         try {
           ///Get shared risk score of another my device
           ///regarding sharing information to check on GEIGER wiki
           var devicesPairingNode = await Storage.controller.get(':Enterprise:Users:${Storage.userUUID}:devicesPairing');
           var getChildren = await devicesPairingNode.getChildren();
           var devicesIterator = getChildren.entries.iterator;
     
           ///Check shared risk score of another my devices (0...*)
           ///Based on GEIGER wiki
           while (devicesIterator.moveNext()) {
     
     //          var userNameNode = await devicesIterator.current.value.getValue('userName');
     //          var useruuidNode = await devicesIterator.current.value.getValue('userUUID');
     //          var devicenameNode = await devicesIterator.current.value.getValue('deviceName');
             var sharedGEIGERScoreNode = await devicesIterator.current.value.getValue('sharedGeigesharedThreatsScorerScore');
             var sharedThreatsScoreNode = await devicesIterator.current.value.getValue('');
             var sharedNumberOfMetricNode = await devicesIterator.current.value.getValue('sharedNumberOfMetric');
     //          var sharedScoreDateNode = await devicesIterator.current.value.getValue('sharedScoreDate');
     
     //          var username = userNameNode!.getValue('en');
     //          var useruuid = useruuidNode!.getValue('en');
     //          var devicename = devicenameNode!.getValue('en');
             var sharedGEIGERScore = sharedGEIGERScoreNode!.getValue('en');
       var sharedThreatsScore = sharedThreatsScoreNode!.getValue('en');
       var sharedNumberOfMetric = sharedNumberOfMetricNode!.getValue('en');
     //          var sharedScoreDate = sharedScoreDateNode!.getValue('en');
     
     
       var tempSharedThreatsScore = sharedThreatsScore!.split(';');
       sharedthreatsScoreList.add(tempSharedThreatsScore);
     
       ///Makes Map {Risk Score: Number of Metric}
       ///this store at global value 'CoreValues.aggregate'
       var tempMap = {};
       tempMap[sharedGEIGERScore] = sharedNumberOfMetric;
       CoreValues.aggregate.add(tempMap);
     
       }
     
       ///Sum shared risk threat score from shared devices
       for(var threatsScoreAggregate in sharedthreatsScoreList ){
     
       for(var i = 0; i < CoreValues.geigerThreatUUID.length; i++){
     
       var tempTSA = threatsScoreAggregate[i].split(',');
       var score = double.parse(tempTSA[1]);
     
       CoreValues.aggregateThreatsScore += '${tempTSA[0]},$score;';
       }
       }
     
       } on StorageException {
       logger.i("Doesn't exists your Pairing devices, Please pair your devices");
     
       }
     
     
     
       }catch (e){
       logger.i('indiA getScore ${e.toString()}');
       }
     
     }