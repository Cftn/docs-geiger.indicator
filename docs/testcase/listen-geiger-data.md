<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->

#

##Set up Storage Listener
 
 **given**
 > prepared `Local Data Storage instance` to access Local Data Storage.
 
 **when** 
 > call the `setValueListener()` function to register the specific node to receive notification . 
 
 **then** 
 > call the `registerListener()` function with callback function, path and primary key(attribute name).
 
     ///Set the trigger to get data for calculation using Local Storage Listener
     Future setValueListener() async{
     
       ///To receive Metric data (Positive, Negative)
       ///The calculation algorithm consist of positive, negative and countermeasures)
       await registerListener(Listener.indicatorListener, Types.userNode,'GEIGERvalue');
       await registerListener(Listener.indicatorListener, Types.deviceNode,'GEIGERvalue');
     
       ///To receive Implemented recommendations from UI
       ///This data effect to risk score as Positive metric
       ///And effect to each threats score which is related Implemented Recommendations
       await registerListener(Listener.indicatorListener, Types.userNode,'implementedRecommendations');
       await registerListener(Listener.indicatorListener, Types.deviceNode,'implementedRecommendations');
     
       ///To receive dynamic recommendations from external plugin
       ///Chatbot, KSP provide their onw recommendation
       await registerListener(Listener.indicatorListener, Types.deviceNode,'pluginName');
     
       ///To receive Pairing information
       ///It include device, employee paring and it used on aggregate score and MSE score
       await registerListener(Listener.indicatorListener, ':' + Types.enterprise + ':Users:' + Storage.userUUID,'sharedGeigerScore');
     }
     
     ///Register Listener based on path, Value using Local Storage Listener
     Future registerListener(var l, var path, var type) async{
     
       ///Register parent path of target Node
       SearchCriteria s = SearchCriteria(searchPath: path);
       ///Set specific attribute of target Node
       s.nodeValueKey = type;
       Storage.controller.registerChangeListener(l, s);
     
       logger.d(s);
     }
     
<!--next-->
***
    
##Getting metrics data

*Getting metric data (Sensor Value, Recommendation*
 
 **given**
 > - prepared `Local Data Storage instance` to access Local Data Storage.
 > - registered `specific node (metrics)` to received new data from Local Data Storage.
 
 **when** 
 > Occurring creation or update on node registered subscription by `Storage Listener`.
 
 **then** 
 > - delivered `new data node` to `gotStorageChange` callback function.
 > - checked `new data node` based on each conditions
 > - call `updateData()` function to parsing and calculate risk score. 
 
       ///Received metric data using Storage Listener
       @override
       void gotStorageChange(EventType event, Node? oldNode, Node? newNode) async {
     
           logger.i('new event from =>> ${newNode!.owner} ${newNode.path} ');
     
     
         ///'owner' is ID
         /// Should compare the owner as me
         /// Avoid infinite recursion loop by indicator own
         if(newNode.owner != Types.owner){
     
     
               ///get metrics from external plugin
               if(newNode.path.contains(':data:metrics') && newNode.name != 'metrics' ){
     
                     logger.i('got event metrics => ${newNode.path}');
                     //await Future.delayed(const Duration(milliseconds: 1000));
                     Storage.getValuePath.add(newNode.parentPath);
                     await updateData(newNode);
     
               }
               ..... 
          }
       }
     
     ------------------------------
     
     ///Update risk score as latest
     ///Refer latest metrics from external plugins
     Future updateData(Node? metricNode) async{
     
       try {
     
           ///split entire path using ':'
           ///':~:~:~:....'
           ///path List [0] => ''
           ///path LIst [1] => Users or Devices etc.
           ///...
           var pathList = metricNode!.path.toString().split(':');
     
           var type = '';
           var nodePath = '';
           var nodeName = '';
     
     
           ///Set metrics type, path and name
           if(pathList[1] == Types.users){
             type =Types.users;
             nodePath = Types.userScoreNodePath;
             nodeName = Types.geigerScoreUser;
           }else{
     
             type =Types.devices;
             nodePath = Types.deviceScoreNodePath;
             nodeName =Types.geigerScoreDevice;
           }
     
           ///Check metric value (GEIGERvalue) whether exist or not
           if((await metricNode.getValue('GEIGERvalue')) != null) {
     
             //await clearPN();
     
             await updateMetrics(type, metricNode);
             await updateScoreNode(nodePath,nodeName,type);
             await updateRecommendationNode(nodePath,Types.recommendations,type);
             await aggregateScore();
     
           }else{
             logger.i('updateMetrics $metricNode GEIGERvalue is not existed');
             return;
           }
     
       }catch(e){
         logger.e(e);
     
       }
     
     }
     
<!--next-->
***     

##Getting pairing data

 
*Getting pairing data (Devices, Employees)*
                                                                     
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