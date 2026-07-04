# Meerkat HTB

![meerkat.png](/MeerkatHTB/screenshots/meerkat1.png)

# Sherlock Scenario
> As a fast-growing startup, Forela has been utilising a business management platform. Unfortunately, our documentation is scarce, and our administrators aren't the most security aware. As our new security provider we'd like you to have a look at some PCAP and log data we have exported to confirm if we have (or have not) been compromised.

# Task 1: We believe our Business Management Platform server has been compromised. Please can you confirm the name of the application running?

```bash
jq .[].alert.signature meerkat-alerts.json
```

![jq.png](/MeerkatHTB/screenshots/jq.png)
