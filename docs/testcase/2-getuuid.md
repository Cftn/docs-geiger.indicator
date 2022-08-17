 <h3 style="box-shadow: 0px 4px 0px 0px #233c68;">Get current user and device UUID on physical device</h3>
 
  **given**
  > prepared `Local Data Storage instance` to access Local Data Storage.
  
  **when** 
  > call `initStorageController()` function to get current User, Device UUID. 
     
  **then** 
  > get current `user UUID`, `device UUID` on physic device. </br>
  
     ///initialize instance of controller
     ///access LDS on current device (android, windows etc)
      Future initStorageController() async{
    
          final localMaster = (await getGeigerApi("", GeigerApi.masterId, Declaration.doNotShareData))!;
          controller = localMaster.storage;
      }
     