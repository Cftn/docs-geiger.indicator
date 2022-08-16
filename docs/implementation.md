The implementation of GEIGER Indicator develop using Dart and Flutter which can find on GitHub (https://github.com/cyber-geiger/toolbox-indicator). The GEIGER Indicator aim to support cross platform, so the Dart and Flutter is good option that provide compiling for several operating system such as Android, IOS, Windows etc. 

<h2>System Architecture</h2>
Figure 11 depicts the system architecture of the GEIGER indicator. It consists of three main modules. From a GEIGER Indicator perspective, all data is collected from the Local Data Storage (LDS) using LDS API, Listener. The output of the GEIGER Indicator is stored in the LDS. The LDS listener notify the events of nodes (Create or Update) to GEIGER Indicator (Step. 1). The Listener of Indicator delivers the metric data selected though node path to core module (Step. 2). The Indicator Core calculates GEIGER score using Indicator Algorithm and sends the results to the storage connector (Step. 3). Finally, the storage connector update GEIGER score node used results on LDS (Step. 4). 

<img width="720" alt="overview-archi" src="">


<h2>Flow Chart</h2>
Figure 12 shows the flow chart of Indicator as how to provide GEIGER score for threat of SMEs. The indicator checks the master node of LDS using GEIGER API to synchronize GEIGER toolbox (Step. 1). The current user and device UUID require access to necessary nodes to calculate (Step. 2). Setup the listener to get a notification of data events using LDS API (Step. 3) and then Preparing the threat data to use on the GEIGER algorithm (Step. 4). Before starting to calculate the GEIGER score, check the existing GEIGER score nodes in LDS for creation or update (Step. 5). These works as upper are initial work on GEIGER Indicator workflow. Now, to calculate GEIGER score as a threat for SMEs, the indicator waits for the metric nodes (“data:metric”) on LDS and uses a listener and checks if it exists (Step. 6).  Finally, The GEIGER score nodes store to LDS and then check the existence of the enterprise nodes for calculation of GEIGER MSE nodes (Step. 7). 

<img width="720" alt="overview-archi" src="">
    

<h2>Source Code</h2>


<h2>Test case</h2>
