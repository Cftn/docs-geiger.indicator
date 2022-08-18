<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->

#
## Mapping threats

*Mapping global threat to GEIGER threat with global profiles (company type)*
 
  **given**
  > prepared `Local Data Storage instance` to access Local Data Storage.
  
  **when** 
  > call `generateThreats()` function to mapping normal threats to geiger threats (12). 
     
  **then** 
  > - get normal threats from `:Global:threats` node. 
  > - parsing and mapping to threats data to `Map<key,value>` in `variables.dart` referred `:Global:profiles`.
 
     ///Parsing ':Global:threats' Node
     ///Normal threats -> GEIGER threats (12)
     ///GEIGER threats (12) defined by CERT
     Future generateThreats() async {
       try {
     
         var globalThreats = await Storage.controller.get(':Global:threats');
         var nodesCsv = await globalThreats.getChildNodesCsv();
     
         if(nodesCsv.isNotEmpty) {
     
           ///get all threat nodes
           var getChildren = await globalThreats.getChildren();
           var threatsIterator = getChildren.entries.iterator;
     
           ///To makes threat uuid list
           ///this uuid use on map as key
           var uuidThreats = nodesCsv.toString().split(',');
           logger.d('uuidThreats => ${uuidThreats.toString()}');
           var uuidIterator = uuidThreats.iterator;
           CoreValues.uuidThreats = uuidThreats;
     
           ///loop threats
           while (threatsIterator.moveNext()) {
             uuidIterator.moveNext();
     
             var threatNameNode = await threatsIterator.current.value.getValue('name');
             var threatName = threatNameNode!.getValue('en');
     
       var geigerThreatNameNode = await threatsIterator.current.value.getValue('GEIGER_threat');  //geigerThreat
       var geigerThreatName = geigerThreatNameNode!.getValue('en');
     
     
       ///Making the threat maps
       CoreValues.threatUUIDThreatName[uuidIterator.current] = threatName;
       CoreValues.threatNameThreatUUID[threatName] = uuidIterator.current;
     
       CoreValues.threatUUIDGeigerName[uuidIterator.current] = geigerThreatName;
     
       if(!CoreValues.geigerNameThreatUUID.containsKey(geigerThreatName)){
       CoreValues.geigerNameThreatUUID[geigerThreatName] = [];
       CoreValues.geigerNameThreatUUID[geigerThreatName].add(uuidIterator.current);
       }else{
       CoreValues.geigerNameThreatUUID[geigerThreatName].add(uuidIterator.current);
       }
     
       }
       }
     
     
       }catch(e) {
       logger.e('generateThreats ${e.toString()}');
       }
     }
     
<!--next-->
***

## Prepare global profiles
*Prepare metrics about company profile(type) => (based, enabler, dependent)*
 
  **given**
  > prepared `Local Data Storage instance` to access Local Data Storage.
  
  **when** 
  > call `getProfiles()` function to parsing `:Global:profiles`. 
     
  **then** 
  > - get profile data from `:Global:profiles` node. 
  > - parsing to profile data to `Map<key,value>` in `variables.dart`.
 
     ///Get global profile data
     ///it is static from cloud and cyber security institute
     ///define threat value base on company type (based, enabler, dependent)
     ///** threat uuid in global profile is match to global threat uuid **
     ///** that mean uuid of global threat is static, fixed **
     Future getProfiles() async {
     
       const path = ':Global:profiles';
     
       try{
     
         Node profilesNode = await Storage.controller.get(path);
     
         var getChildren = await profilesNode.getChildren();
     
         var profileNodeIterator = getChildren.entries.iterator;
     
         ///Get each company type (based, enabler, dependent)
         while (profileNodeIterator.moveNext()) {
     
           var nameNode = await profileNodeIterator.current.value.getValue('name');
           var profileName = nameNode!.getValue('en')!.toLowerCase();
     
       //Profile UUID와 Theat UUID 같은 놈껄로 다른 이름 같은 것도 같은 값을 가
     
       for(var m in CoreValues.threatUUIDGeigerName.entries){
     
       ///Get value of threat in company type using Global threat uuid
       ///m.key => global threat uuid (12)
       ///m.value => value for metrics
       var metrics = await profileNodeIterator.current.value.getValue(m.key);
     
       if(metrics != null){
       if(!CoreValues.geigerThreatUUID.contains(m.key)){
       CoreValues.geigerThreatUUID.add(m.key);
       CoreValues.geigerThreatNameThreatUUID[m.value] = m.key;
       CoreValues.geigerThreatUUIDThreatName[m.key] = m.value;
       }
       ///get threat uuid list using geiger name
       var normalThreatUUIDList = CoreValues.geigerNameThreatUUID[m.value];
       await storeV(profileName, metrics, normalThreatUUIDList);
       }
       }
     
     
       }
     
       logger.d('geigerThreatUUID => ${CoreValues.geigerThreatUUID}');
       logger.d('geigerThreatNameThreatUUID => ${CoreValues.geigerThreatNameThreatUUID}');
       logger.d('geigerThreatUUIDThreatName => ${CoreValues.geigerThreatUUIDThreatName}');
       logger.d('based => ${CoreValues.based}');
       logger.d('enabler => ${CoreValues.enabler}');
       logger.d('dependent => ${CoreValues.dependent}');
     
     
       } on StorageException{
       logger.i('$path does not exist');
     
       }
       catch (e){
     
       logger.e(e);
       }
     }
     
<!--next-->
***

##Prepare global recommendations

*Prepare metrics about recommendation each threat*
 
  **given**
  > prepared `Local Data Storage instance` to access Local Data Storage.
  
  **when** 
  > call `getRecommendation()` function to parsing `:Global:profiles`. 
     
  **then** 
  > - get profile data from `:Global:recommendation` node. 
  > - parsing to profile data to `Map<key,value>` in `variables.dart`.
 
    ///Making Map<String, List<Map>> user, device Recommendation
    ///Parsing global recommendation metrics
    /// key => global threat UUID unique (12)
    /// value => recommendation Map List
    /// recommendation Map List => [{metric, recommendation UUID},....]
    Future getRecommendation() async {
    
      const path = ':Global:Recommendations';
    
      try {
    
        ///Get all global recommendation List
        ///this is static from cloud
        var recommendationNode = await Storage.controller.get(path);
        var getChildren = await recommendationNode.getChildren();
        var recoStrList = await recommendationNode.getChildNodesCsv();
        var recoList = recoStrList.split(',');
        logger.d('reco List => $recoList');
    
        var recoNodeIterator = getChildren.entries.iterator;
    
    
        ///loop each recommendation Node
        while (recoNodeIterator.moveNext()) {
    
          var recommendationTypeNode = await recoNodeIterator.current.value.getValue('RecommendationType');
          var recommendationType = recommendationTypeNode!.getValue('en')!;
      logger.d('a$recommendationType');
    
      var relatedThreatsWeightsNode = await recoNodeIterator.current.value.getValue('relatedThreatsWeights');
      //logger.d('b$relatedThreatsWeightsNode');
      var metricThreats = relatedThreatsWeightsNode!.getValue('en')!.split(';'); //List<String>
      logger.d('c$metricThreats');
    
      for (var metricNode in metricThreats) {
    
      ///{'threatUUID,metricValue({'high', 'medium', 'low'})
      ///metric[0] =>  threatUUID
      ///metric[1] =>  metric {'high' or 'medium' or 'low'}
      var metric = metricNode.split(',');
      logger.d('>>>$metric');
    
      var metricRecoUUID = {};
      metricRecoUUID['metric'] = metric[1];
      metricRecoUUID['recoUUID'] = recoNodeIterator.current.key;
    
    
    
      if (recommendationType == Types.recoTypeDevice) {
      try {
      if(metric[0].isNotEmpty) CoreValues.deviceRecommendation[metric[0]]!.add(metricRecoUUID);
      } catch (e) {
      var temp = <Map>[];
      temp.add(metricRecoUUID);
      CoreValues.deviceRecommendation[metric[0]] = temp;
      }
      } else {
      try {
          if(metric[0].isNotEmpty) CoreValues.userRecommendation[metric[0]]!.add(metricRecoUUID);
      } catch (e) {
          var temp = <Map>[];
          temp.add(metricRecoUUID);
          CoreValues.userRecommendation[metric[0]] = temp;
      }
      }
      }
      }
      logger.d('userRecommendation => ${CoreValues.userRecommendation}');
      logger.d('deviceRecommendation => ${CoreValues.deviceRecommendation}');
    
    
      } on StorageException{
      logger.e('$path does not exist, please check your global data');
    
      }
      catch (e) {
      logger.e(e);
      }
    }     