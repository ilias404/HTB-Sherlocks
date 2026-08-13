# Meerkat HTB

![meerkat.png](/MeerkatHTB/screenshots/meerkat1.png)

# Sherlock Scenario
> As a fast-growing startup, Forela has been utilising a business management platform. Unfortunately, our documentation is scarce, and our administrators aren't the most security aware. As our new security provider we'd like you to have a look at some PCAP and log data we have exported to confirm if we have (or have not) been compromised.

# Task 1: We believe our Business Management Platform server has been compromised. Please can you confirm the name of the application running?

Upon extraction, we have two files: a PCAP file and a JSON file. Let's start with the JSON file using the `jq` command tool.

To identify the targeted application, all alert signatures contained in the Suricata alert log were enumerated. One of the signatures referenced a known vulnerability affecting Bonitasoft.

> `jq` is a command-line tool used to process and analyze JSON data.

```bash
jq .[].alert.signature meerkat-alerts.json
```

![jq.png](/MeerkatHTB/screenshots/jq.png)

Ans: `Bonitasoft`

# Task 2: We believe the attacker may have used a subset of the brute forcing attack category - what is the name of the attack carried out?

Let's open the PCAP file and search for POST requests using the filter `http.request.method == "POST"`.

![credstuff.png](/MeerkatHTB/screenshots/credstuff.png)

By navigating through each request, we can find a set of usernames and passwords that appear to have been stolen in a data breach. 

> Credential stuffing is a cyberattack where criminals use automated bots to test massive lists of stolen usernames and passwords against unrelated websites.

Ans: `Credential Stuffing`
# Task 3: Does the vulnerability exploited have a CVE assigned - and if so, which one?

![jq.png](/MeerkatHTB/screenshots/jq.png)

Ans: `CVE-2022-25237`
# Task 4: Which string was appended to the API URL path to bypass the authorization filter by the attacker's exploit?
Searching for the CVE online will provide more information on this detail.

![cveb.png](/MeerkatHTB/screenshots/cveb.png)


Ans: `i18ntranslation`
# Task 5:How many combinations of usernames and passwords were used in the credential stuffing attack?

By reviewing all HTTP POST authentication requests, 56 unique username/password combinations were submitted during the credential stuffing attack.

Ans: `56`
# Task 6: Which username and password combination was successful?

Most authentication attempts returned HTTP 401 (Unauthorized). Only one request returned HTTP 204 (No Content), indicating the credentials were accepted successfully.

![seb.png](/MeerkatHTB/screenshots/seb.png)

> The HTTP status code 204 No Content indicates that the server has successfully processed the request, but there is no payload body or content to send back to the client.

Ans: `seb.broom@forela.co.uk:g0vernm3nt`
# Task 7: If any, which text sharing site did the attacker utilise?

Reviewing the HTTP traffic revealed outbound requests to pastes.io, indicating the attacker retrieved or uploaded data using the paste-sharing service.

![pastes.png](/MeerkatHTB/screenshots/pastes.png)

Ans: `pastes.io`
# Task 8: Please provide the filename of the public key used by the attacker to gain persistence on our host.

The attacker downloaded an SSH public key from `pastes.io` and saved it locally as `hffgra4unv`, which was later appended to the victim's `authorized_keys` file to establish SSH persistence.

![pubkey.png](/MeerkatHTB/screenshots/pubkey.png)


Ans: `hffgra4unv`
# Task 9: Can you confirm the file modified by the attacker to gain persistence?

We can find the modified file from the screenshot of the previous task.

Ans: `/home/ubuntu/.ssh/authorized_keys`
# Task 10: Can you confirm the MITRE technique ID of this type of persistence mechanism?

![t1098.png](/MeerkatHTB/screenshots/t1098.png)


Ans: `T1098.004`

# Attack Timeline

```text
Credential Stuffing (56 credential pairs)
                │
                ▼
Successful Login
(seb.broom@forela.co.uk:g0vernm3nt)
                │
                ▼
Exploit CVE-2022-25237
(/i18ntranslation authorization bypass)
                │
                ▼
Download SSH Public Key
(from pastes.io)
                │
                ▼
Append Key to
/home/ubuntu/.ssh/authorized_keys
                │
                ▼
Persistent SSH Access
(MITRE ATT&CK T1098.004)
```

# Conclusion: 
The attacker performed a credential stuffing attack against the Bonitasoft application, successfully authenticating with stolen credentials. They then exploited `CVE-2022-25237` to bypass authorization controls and execute privileged actions. Finally, they established persistence by downloading an SSH public key from `pastes.io` and appending it to `/home/ubuntu/.ssh/authorized_keys`, enabling future passwordless access.

![meerkatpwned.png](/MeerkatHTB/screenshots/meerkatpwned.png)
