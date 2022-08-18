<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->

 
 <h3 style="box-shadow: 0px 4px 0px 0px #233c68;">Prepare metrics about recommendation each threat</h3>
 
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