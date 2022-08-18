<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->


<h2>Source Code</h2>
The implementation of GEIGER Cybersecurity Risk Indicator develop using Dart and Flutter. The GEIGER Indicator aim to support cross platform, so the Dart and Flutter is good option that provide compiling for several operating system such as Android, IOS, Windows etc. </br>

[pud.dev](https://pub.dev/documentation/toolbox_indicator_test/latest/indicator/indicator-library.html) </br>
[Github](https://github.com/Cftn/GEIGER-indicator)

***

⚙️ [pubspec.yaml](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/.readthedocs.yaml)<br>
Read the GEIGER Indicator Build configuration is stored in `.pubspec.yaml`. This is dart/flutter standard. Add your own configurations here, such as extensions. Remember that many extensions and additional Dart packages to be installed.

***

📚 [lib/src/](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
All source code of GEIGER Indicator lives in `lib/src`, it was generated using Dart defaults. All the `*.dart` make up sections in the documentation.

💡 [../init.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
It consist of initial function to get and check exist data. (`physics user/device UUID, global/score data`)

💡 [../listener.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Setting the listener using Storage API and manage the notification of metric data (e.g, `sensor value`, `implemented recommendation` etc.).

💡 [../sensors_data.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Manage the sensor values (e.g, check exist data and update new `sensor value` etc.).

💡 [../variables.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Include all core values of GEIGER indicator (e.g, `global data`, `score data`, `storage api`, `node path` etc.).

***

💡 [lib/src/score/](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Related calculation of score (`user score`,`device score`,`aggregate score`,`SME score`).

💡 [../aggregate_score.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Calculate `aggregate score` based on `indicator algorithm` with pairing data. 

💡 [../geiger_score.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Calculate and manage `user/device/aggregate score nodes` based on `indicator algorithm`.

💡 [../sme_score.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Calculate and manage `SME score` based on `indicator algorithm` with pairing data. 

***
💡 [lib/src/globaldata/](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Related mapping and pre-calculation data for risk score calculation (`global threat`,`global recommendation`,`global profile`).

💡 [../global_profile.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Mapping and parsing `global profile (company type)` for risk score calculation.

💡 [../global_recommendation.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Calculate, mapping and parsing `global recommendation` for risk score calculation.

💡 [../global_threat.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/api.md)<br>
Mapping and parsing `global threat (unique) based on global profile` for risk score calculation.

***

📍 [lib/indicator.dart](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/docs/requirements.txt) <br>
The main function 'runIndicator' is in this source code. who want to use GEIGER indicator, import this file and call main function.

***

📜 [README.md](https://github.com/readthedocs-examples/example-mkdocs-basic/blob/main/README.rst)<br>
Contents of this `README.md` are visible on Github and included on [the documentation index page](https://example-mkdocs-basic.readthedocs.io/en/latest/) (Don\'t Repeat Yourself).

⁉️ Questions / comments<br>
If you have questions related to this example, feel free to can ask them as a Github issue [here](https://github.com/readthedocs-examples/example-mkdocs-basic/issues).

***

Read the Docs tutorial
----------------------

To get started with Read the Docs, you may also refer to the [Read the Docs tutorial](https://docs.readthedocs.io/en/stable/tutorial/). It provides a full walk-through of building an example project similar to the one in this repository.


Author
----------------------

JongGwan An (kman3212@gmail.com)