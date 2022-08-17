 <h3 style="box-shadow: 0px 4px 0px 0px #233c68;">Getting metric data (Sensor Value, Recommendation)</h3>
 
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
     
     
     