 <!--**
  @file
  @copyright FHNW Switzerland 2022, FHNW
  @authors JongGwan An [kman3212@gmail.com]
 -->
# 

###User/Device/Aggregate Score
 
*Score Calculation (User, Device, Calculation)*
 
 **given**
 > - prepared `Local Data Storage instance` to access Local Data Storage.
 > - prepared `GEIGER data (metrics, recommendation, pairing)` from external plugin.
 
 **when** 
 > call the `updateScoreNode()` function to calculate the risk score, update risk node on local data storage
 
 **then** 
 > - calculate `risk score each threat` on `crtThreatScore()` function.
 > - calculate `total risk score` on `crtTotalThreatScore()` function.
 > - update `geiger score node` on `Local data Storage`. 
 
 
     ///update geiger score node (user, device, aggregate)
     ///To get metrics (external plugin, recommendations)
     Future updateScoreNode(var path, var nodeName,var type) async {
     
       try{
         Node scoreNode = await Storage.controller.get('$path$nodeName');
         scoreNode.owner = Types.owner;
     
         var geigerScore = 0.0;
     
     
         ///Calculate risk score each geiger threats (12)
         ///Based on metric of external plugin and recommendations
         NodeValue geigerThreatScores = NodeValueImpl('threats_score', nodeName == Types.geigerScoreAggregate ? CoreValues.aggregateThreatsScore : await crtThreatScore(type));
         await scoreNode.updateValue(geigerThreatScores);
     
         ///Calculate geiger total risk score based on indicator algorithm - refer user, device, recommendation
         nodeName == Types.geigerScoreAggregate ? geigerScore = CoreValues.gagg : geigerScore = double.parse(await crtTotalThreatScore(type));
         NodeValue geigerScoreNode = NodeValueImpl('GEIGER_score', geigerScore.toString());
         await  scoreNode.updateValue(geigerScoreNode);
     
     
         ///Calculate number of metric to used on risk score
         num len = 0;
         if(type == Types.users){
           var temp = {};
           temp.addAll(CoreValues.userPositive);
           temp.addAll(CoreValues.userNegative);
           for(var e in temp.entries){
             len += e.value.length;
           }
           len += CoreValues.uuidUserImplementedRecommendations.length;
     
         }else{
           var temp = {};
           temp.addAll(CoreValues.devicePositive);
           temp.addAll(CoreValues.deviceNegative);
           for(var e in temp.entries){
             len += e.value.length;
           }
           len += CoreValues.uuidDeviceImplementedRecommendations.length;
         }
         var numberMetrics = '';
         nodeName == Types.geigerScoreAggregate ? numberMetrics = CoreValues.nagg : numberMetrics = len.toString();
         NodeValue geigerNumMetrics = NodeValueImpl('number_metrics', numberMetrics);
         await scoreNode.updateValue(geigerNumMetrics);
     
     
     
         ///If type is user, device score (TRUE)
         if (nodeName != Types.geigerScoreAggregate) {
           var implementedRecommendationsReco = '';
           type == Types.users ? implementedRecommendationsReco = CoreValues.uuidUserImplementedRecommendations.toString().replaceAll('[','').replaceAll(']','').trim() : implementedRecommendationsReco = CoreValues.uuidDeviceImplementedRecommendations.toString().replaceAll('[','').replaceAll(']','').trim();
     
           NodeValue implementedRecommendations = NodeValueImpl('implementedRecommendations',implementedRecommendationsReco);
           await scoreNode.updateValue(implementedRecommendations);
     
     
           await Storage.controller.addOrUpdate(scoreNode);
           logger.i('UPDATE $nodeName Node => ${await Storage.controller.dump('$path$nodeName')}');
     
         }
         ///If type is aggregate score (total score) (FALSE)
         else {
     
           await Storage.controller.addOrUpdate(scoreNode);
           logger.i('UPDATE $nodeName Node => ${await Storage.controller.dump('$path$nodeName')}');
     
     
         }
     
       }on StorageException{
         logger.e('data does not exist');
       }
       catch (e){
         logger.e(e);
       }
     }
<!--next-->
***

###SME Score Calculation     

*MSE Score Calculation (company total risk score)*

 **given**
 > - prepared `Local Data Storage instance` to access Local Data Storage.
 > - prepared `GEIGER data (metrics, recommendation, pairing)` from external plugin.
 > - current physics device owner is company owner (CEO).
 
 **when** 
 > call the `updateScoreMSENode()` function to calculate the SME risk score, update the risk node on local data storage
 
 **then** 
 > - get `aggregate risk score` of `employees` on `getEmployeesSharedScore()` function.
 > - calculate `total risk score refer prepared metrics` on `crtAggregateScore()` function.
 > - update `SME risk score node` on `Local data Storage`. 
  
    ///update geiger MSE score node
    ///it required employee pairing and main user (CEO)
    Future updateScoreMSENode() async {
      try {
    
        if (CoreValues.flagMainUser) {
          var scoreMSENode = await Storage.controller.get(':Enterprise:'+ Storage.geigerIndicatorUUID +':data:GeigerScoreMSE');
          scoreMSENode.owner = Types.owner;
          
          ///create node keys: UserScore, ImplementRecommendations
          /// ignore: non_constant_identifier_names
          NodeValue GEIGERScore = NodeValueImpl('GEIGER_score',
              CoreValues.gagg.toString()); //add node name (name of threat)
          scoreMSENode.updateValue(GEIGERScore);
    
          NodeValue location =
          NodeValueImpl('location', CoreValues.enterpriseNode['location']);
          scoreMSENode.updateValue(location);
    
    
          NodeValue associatedProfiles = NodeValueImpl('associatedProfiles',
              CoreValues.enterpriseNode['associatedProfiles']);
          scoreMSENode.updateValue(associatedProfiles);
    
          await Storage.controller.addOrUpdate(scoreMSENode);
          logger.i('UPDATE GeigerScoreMSE Node => ${await Storage.controller.dump(':Enterprise:'+ Storage.userUUID + ':'+ Storage.geigerIndicatorUUID +':data:GeigerScoreMSE')}');
        }
      } catch (e) {
        logger.e(e);
      }
    }