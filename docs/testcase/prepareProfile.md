<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->
 
 <h3 style="box-shadow: 0px 4px 0px 0px #233c68;">Prepare metrics about company profile(type) => (based, enabler, dependent)</h3>
 
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
     