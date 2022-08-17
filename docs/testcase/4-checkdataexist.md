 <h3 style="box-shadow: 0px 4px 0px 0px #233c68;">Check Risk score data exist </h3>
 
  **given**
  > prepared `Local Data Storage instance` to access Local Data Storage.
    
  **when** 
  > call `checkExistNodes` function to check the GEIGER data. 
     
  **then** 
  > - get current `user UUID`, `device UUID` on physic device. </br>
  > - parsing and mapping `:Global:threats` data. </br>
  > - check exist `geiger score node` on Local Data Storage. </br>
 
     ///check indicator nodes whether exist or not
     ///geiger score node
     ///which is created by indicator
     Future checkExistNodes() async {
        
       await isExistParentNode(Types.userScoreNodePath);
       await isExistParentNode(Types.deviceScoreNodePath);
       await isExistParentNode(Types.enterpriseScoreNodePath);
     
       await getExistScoreNodes(Types.userScoreNodePath, Types.geigerScoreUser,Types.users);
       await getExistScoreNodes(Types.deviceScoreNodePath, Types.geigerScoreDevice,Types.devices);
     
       await getExistScoreNodes(Types.userScoreNodePath, Types.recommendations,Types.users);
       await getExistScoreNodes(Types.deviceScoreNodePath, Types.recommendations,Types.devices);
     
       await getExistScoreNodes(Types.userScoreNodePath, Types.geigerScoreAggregate,Types.users);
       await getExistScoreNodes(Types.enterpriseScoreNodePath,Types.geigerScoreMSE,'');
     }
     