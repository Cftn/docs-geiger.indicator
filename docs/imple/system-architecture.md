<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->

<h2>System Architecture</h2>
Figure 11 depicts the system architecture of the GEIGER indicator. It consists of three main modules. From a GEIGER Indicator perspective, all data is collected from the Local Data Storage (LDS) using LDS API, Listener. The output of the GEIGER Indicator is stored in the LDS. The LDS listener notify the events of nodes (Create or Update) to GEIGER Indicator (Step. 1). The Listener of Indicator delivers the metric data selected though node path to core module (Step. 2). The Indicator Core calculates GEIGER score using Indicator Algorithm and sends the results to the storage connector (Step. 3). Finally, the storage connector update GEIGER score node used results on LDS (Step. 4). 

![system-archi](https://user-images.githubusercontent.com/15152117/184823544-c2dfe8e0-6c37-46f2-ba6d-c10f855570d6.png)
