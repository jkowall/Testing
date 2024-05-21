Testing


[//]: # (   ```mermaid) 
[//]: # ( ---)
[//]: # ( title: "Aiven Thanos Current Architecture")
[//]: # ( ---)
[//]: # ( flowchart)
[//]: # ( 	1[("Customer Databases")] -->|"Telegraf"| 2((("Kafka"))))
[//]: # (   style 1 stroke-width: 2px)
[//]: # ( 	2 -->|"Telegraf"| 3(["Thanos Routing Reciever"]))
[//]: # (   3 ==>|"AZ-aware ketama hashring"| 4(["Thanos Ingesting Receiver"]))
[//]: # ( 	113702["Thanos Compactor"] --> 141231[("Google Cloud Storage (Object Store)")])
[//]: # (   4 --> 141231)
[//]: # (   596267{{"Grafana"}} --> 156003(["Thanos Query Frontend"]))
[//]: # ( 	814016["VMAlert"] --> 156003)
[//]: # ( 	814016 --> 812224["Alertmanager"])
[//]: # ( 	156003 --> 907649["Thanos Query"])
[//]: # ( 	907649 --> 209978["Thanos Store"])
[//]: # ( 	907649 --> 4)
[//]: # ( 	209978 --> 141231)
[//]: # ( )
[//]: # ( %% Mermaid Flow Diagram)
[//]: # ( ```)

```mermaid
stateDiagram-v2
    state "needs-triage" as needs_triage {
        state "needs-triage" as triage
        [*] --> triage : Issue created
        triage --> needs_info : Command: needs-info
        note right of triage
            Issue is pending review and triage by maintainers
        end note
    }
    state "needs-info" as needs_info {
        state "needs-info" as info
        info --> needs_triage : Activity
        info --> stale : Timeout (30 days)
        stale --> closed : Timeout (30 days)
        note right of info
            Waiting for additional information from OP or non-maintainer
        end note
    }
    state "help-wanted" as help_wanted {
        state "help-wanted" as wanted
        wanted --> in_progress : Command: /in-progress
        wanted --> stale : Timeout (90 days)
        stale --> closed : Timeout (90 days)
        wanted --> wanted : Activity
        note right of wanted
            Issue is accepted, waiting for volunteers to pick up
        end note
    }
    state "in-progress" as in_progress {
        state "in-progress" as progress
        progress --> stale : Timeout (30 days)
        stale --> help_wanted : Timeout (30 days)
        progress --> progress : Activity
        note right of progress
            Issue is being worked on
        end note
    }
    needs_triage --> help_wanted
    needs_info --> needs_triage : Comment
    stale --> needs_triage : Comment
    help_wanted --> needs_triage : Comment
    in_progress --> help_wanted : Comment
    closed --> [*]
```
