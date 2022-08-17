 <h3 style="box-shadow: 0px 4px 0px 0px #233c68;"> MSE Score Calculation (company total risk score)</h3>

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