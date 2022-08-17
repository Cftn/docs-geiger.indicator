 <h3 style="box-shadow: 0px 4px 0px 0px #233c68;">Set up Storage Listener</h3>
 
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