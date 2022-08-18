<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->
 
#
##GEIGER <span style='color:#d6604d'>Indicator</span> 

[![Documentation](https://nexus.lab.fiware.org/repository/raw/public/badges/chapters/documentation.svg)](https://fiware-tutorials.rtfd.io)
<!--[![NGSI LD](https://img.shields.io/badge/NGSI-LD-d6604d.svg)](https://www.etsi.org/deliver/etsi_gs/CIM/001_099/009/01.04.01_60/gs_cim009v010401p.pdf)
[![JSON LD](https://img.shields.io/badge/JSON--LD-1.1-f06f38.svg)](https://w3c.github.io/json-ld-syntax/)
[![Support badge](https://img.shields.io/badge/tag-fiware-orange.svg?logo=stackoverflow)](https://stackoverflow.com/questions/tagged/fiware)
-->

GEIGER is an innovation project that aims to develop a solution that allows small businesses to become aware of their risks related to cybersecurity, data protection, and privacy, and get help in reducing these risks.

The project collaborates with security specialists, cyber ranges, and national cybersecurity centres to develop GEIGER, a “Geiger counter” for cybersecurity, which will help small businesses to become aware of cyber threats. 
A small business can use GEIGER on the web or the smartphone, and it dynamically shows the level of current risks for the company. Seeing high risk allows the user to react immediately and to take simple measures or get help in lowering the risk exposure significantly.

The figure as below depicts the overall GEIGER ecosystem. Our focus in this tutorial will be on the GEIGER indicator as shown in the central cloud of the figure. The **GEIGER Indicator solution** allows users to calculate their **GEIGER score**, a measure of the cybersecurity risk to 
which they and their SME are exposed. Based on the characteristics of an SME and the results of the GEIGER Indicator calculation, users receive recommendations for actions to mitigate cybersecurity risk.
We have detailed the algorithmic and cybersecurity concepts behind the GEIGER indicator, the data model that facilitates the algorithm, and the translation of these concepts to code. We provided a theoretical grounding to 
our work in a [scientific paper](https://dl.acm.org/doi/10.1145/3465481.3469199) presented at the 2021 International Conference on Availability, Reliability, and Security (ARES).


<img width="720" alt="overview-archi" src="https://user-images.githubusercontent.com/15152117/184707491-b7e3261c-4bcc-4338-bb64-ec4249f18895.png">



##List of GEIGER modules

<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">GEIGER Toolbox</h3>

The GEIGER Toolbox is a tool enabling the user to assess their Cyberthreat exposure and forms an open ecosystem allowing any interested party to extend this tool by adding plugins. 

  + [GEIGER Storage](https://pub.dev/packages/geiger_localstorage)<br/>
  + [GEIGER Communication API](https://pub.dev/packages/geiger_api)<br/>
  + [GEIGER Toolbox UI](https://github.com/cyber-geiger/toolbox-ui-flutter)<br/>
  + [GEIGER Indicator](https://pub.dev/packages/toolbox_indicator_test)<br/>


<h3 style="box-shadow: 0px 4px 0px 0px #5dc0cf;">Cloud</h3>

There is an additional component named GEIGER Cloud Adapter that will oversee the process of searching and synchronizing data between the GEIGER Toolbox and GEIGER Cloud. Besides, the Cloud Adapter also allows for searching and synchronizing data with the Toolbox Core.

  + [GEIGER Cloud Adapter](https://github.com/cyber-geiger/cloud-adapter)<br/>


<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">External Plugin</h3>

The plugins may be added at any time by the user and integrate seamlessly into the framework. No modification on the core application is required to add new indicators of cyber threats, recommendations, or functionalities. They all may be added using project external code.

  + [CyberRange (MI)]()<br/>
  + [KSP (MI)]()<br/>
  + [Chatbot (KPMG)]()<br/>
