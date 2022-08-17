 <h3 style="box-shadow: 0px 4px 0px 0px #233c68;">Access Local Data Storage</h3>
 
 **given**
 >  add [`geiger_api`](https://pub.dev/packages/geiger_api) package on `pubspec.yaml`.
 
 **when** 
 > call `getGeigerApi()` function to get the instance of Local Storage using `geiger_api` 

 **then** 
 > return `instance of geiger api` **and** get `Master Local Storage` to access GEIGER data.
 
```
 final localMaster = (await getGeigerApi("", GeigerApi.masterId, Declaration.doNotShareData))!;
 var controller = localMaster.storage;
```