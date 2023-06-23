[
  ```mermaid
---
title: "Aiven Thanos Current Architecture"
---
flowchart
	1[("Customer Databases")] -->|"Telegraf"| 2((("Kafka")))
	style 1 stroke-width: 2px
	2 -->|"Telegraf"| 3(["Thanos Routing Reciever"])
	3 ==>|"AZ-aware ketama hashring"| 4(["Thanos Ingesting Receiver"])
	113702["Thanos Compactor"] --> 141231[("Google Cloud Storage (Object Store)")]
	4 --> 141231
	596267{{"Grafana"}} --> 156003(["Thanos Query Frontend"])
	814016["VMAlert"] --> 156003
	814016 --> 812224["Alertmanager"]
	156003 --> 907649["Thanos Query"]
	907649 --> 209978["Thanos Store"]
	907649 --> 4
	209978 --> 141231

%% Mermaid Flow Diagram 
```
]:#
