# GEIGER <span style='color:#d6604d'>Indicator</span> 

[![Documentation](https://nexus.lab.fiware.org/repository/raw/public/badges/chapters/documentation.svg)](https://fiware-tutorials.rtfd.io)
<!--[![NGSI LD](https://img.shields.io/badge/NGSI-LD-d6604d.svg)](https://www.etsi.org/deliver/etsi_gs/CIM/001_099/009/01.04.01_60/gs_cim009v010401p.pdf)
[![JSON LD](https://img.shields.io/badge/JSON--LD-1.1-f06f38.svg)](https://w3c.github.io/json-ld-syntax/)
[![Support badge](https://img.shields.io/badge/tag-fiware-orange.svg?logo=stackoverflow)](https://stackoverflow.com/questions/tagged/fiware)
-->

GEIGER is an innovation project that aims to develop a solution that allows small businesses to become aware of their risks related to cybersecurity, data protection, and privacy, and get help in reducing these risks.

The project collaborates with security specialists, cyber ranges, and national cybersecurity centres to develop GEIGER, a “Geiger counter” for cybersecurity, which will help small businesses to become aware of cyber threats. 
A small business can use GEIGER on the web or the smartphone, and it dynamically shows the level of current risks for the company. Seeing high risk allows the user to react immediately and to take simple measures or get help in lowering the risk exposure significantly.

Figure 1 depicts the overall GEIGER ecosystem. Our focus in this tutorial will be on the GEIGER indicator as shown in the central cloud of Figure 1. The **GEIGER Indicator solution** allows users to calculate their **GEIGER score**, a measure of the cybersecurity risk to 
which they and their MSE are exposed. Based on the characteristics of an MSE and the results of the GEIGER Indicator calculation, users receive recommendations for actions to mitigate cybersecurity risk.
We have detailed the algorithmic and cybersecurity concepts behind the GEIGER indicator, the data model that facilitates the algorithm, and the translation of these concepts to code. We provided a theoretical grounding to 
our work in a scientific paper presented at the 2021 International Conference on Availability, Reliability, and Security (ARES).


![overviewArchi]()


<h3>How to Use</h3>

Each tutorial is a self-contained learning exercise designed to teach the developer about a single aspect of FIWARE. A
summary of the goal of the tutorial can be found in the description at the head of each page. Every tutorial is
associated with a GitHub repository holding the configuration files needed to run the examples. Most of the tutorials
build upon concepts or enablers described in previous exercises the to create a complex smart solution which is
_"powered by FIWARE"_.

The tutorials are split according to the chapters defined within the
[FIWARE catalog](https://www.fiware.org/developers/catalogue/) and are numbered in order of difficulty within each
chapter hence an introduction to a given enabler will occur before the full capabilities of that element are
explored in more depth.

It is recommended to start with reading the full **Core Context Management: The NGSI-LD Interface** Chapter before
moving on to other subjects, as this will give you a fuller understanding of the role of context data in general.
However, it is not necessary to follow all the subsequent tutorials sequentially - as FIWARE is a modular system, you
can choose which enablers are of interest to you.


## List of modules

<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">GEIGER Toolbox</h3>

These first tutorials are an introduction to the NGSI-LD Context Brokers, and are an essential first step when learning
to use NGSI-LD.

&nbsp; 101. [Understanding `@context`](understanding-@context.md)<br/>
&nbsp; 102. [Working with `@context`](working-with-@context.md)<br/>
&nbsp; 103. [CRUD Operations](ngsi-ld-operations.md)<br/>
&nbsp; 104. [Entity Relationships](entity-relationships.md)<br/>
&nbsp; 106. [Subscriptions](subscriptions.md)<br/>
&nbsp; 107. [Temporal Operations](short-term-history.md)<br/>

<h3 style="box-shadow: 0px 4px 0px 0px #5dc0cf;">Cloud</h3>

In order to make a context-based system aware of the state of the real world, it will need to access information from
Robots, IoT Sensors or other suppliers of context data such as social media. It is also possible to generate commands
from the context broker to alter the state of real-world objects themselves.

&nbsp; 201. [Introduction to IoT Sensors](iot-sensors.md)<br/>
&nbsp; 202. [Provisioning the Ultralight IoT Agent](iot-agent.md)<br/>
&nbsp; 203. [Provisioning the JSON IoT Agent](iot-agent-json.md)<br/>

<h3 style="box-shadow: 0px 4px 0px 0px #233c68;">External Plugin</h3>

These tutorials show how to manipulate and store context data, so it can be used for further processing.

&nbsp; 304. [Querying Time Series Data (Crate-DB)](time-series-data.md)<br/>
&nbsp; 305. [Big Data Analysis (Flink)](big-data-flink.md)<br/>
&nbsp; 306. [Big Data Analysis (Spark)](big-data-spark.md)<br/>
