<!--**
 @file
 @copyright FHNW Switzerland 2022, FHNW
 @authors JongGwan An [kman3212@gmail.com]
-->

<h2>GEIGER Indicator Algorithm</h2>

A threat-based cybersecurity risk assessment algorithm must be supported by a data model and data sources that are equally threat- centric. In this section, we describe how a threat-based view of SME cyber-systems produces a data model supporting a threat-based approach to cybersecurity risk assessment. We outline the data required to enable our approach and describe the algorithm that transforms the data into a cybersecurity risk indicator.

The goal's of GEIGER indicator provides three score. This figure shows overview of GEIGER scores. The indicator calculate user and device score from each physical device (Android, Windows, IOS etc.) and owner.
And the indicator provide the aggregate score based on user and device score on physical device. The aggregate score consist of all physical device which is paired on one of device (main device). 
The calculation of SME score is only allow company owner (CEO). In this calculation, the indicator calculate SME score using the aggregate score of the paired employees. 
This calculation is hierarchy as shown on figure.. The supervisor calculate the risk score with employees to aggregate score. The company owner calculate SME score when reach aggregate score of employees under CEO position.

![overview-score](https://user-images.githubusercontent.com/15152117/184838875-bf1d653e-8f5d-4a93-b82b-5b73841124c4.png)

The indicator algorithm designed based on threats metric. The organization of cyber security announce priority of threat and risk which is more dangerous nowadays. 
In GEIGER project, this metric is provided as 'global data' such as ':Global:threat', ':Global:profile', 'Global:recommendation'. This threat impacts consist of three impacts (high, medium, low) about SME actions. 
The indicator algorithm works based on three factor, positive and negative, countermeasures. In this concept, when user do some action on their physical device it would be effect as positive or negative on indicator algorithm. 
This figure shows formula of indicator algorithm. This formula support risk score using global data, metrics data. Please refer this [paper](https://dl.acm.org/doi/10.1145/3465481.3469199) if you want more detailed information.

![algorithm](https://user-images.githubusercontent.com/15152117/184838827-24be329b-b6cf-44a6-80c4-989ec4dcdb72.png)

