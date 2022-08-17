 <h3 style="box-shadow: 0px 4px 0px 0px #233c68;">Mapping global threat to GEIGER threat with global profiles (company type)</h3>
 
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
     