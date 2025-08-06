Device code phishing is seriously underrated in my opinion. 
Yes, we've seen various device code phishing attacks [targeting Microsoft cloud](https://www.microsoft.com/en-us/security/blog/2025/02/13/storm-2372-conducts-device-code-phishing-campaign/) and 
(Salesforce)[https://cloud.google.com/blog/topics/threat-intelligence/voice-phishing-data-extortion], but the attack vector has a lot more potential, since many other services, such as github, 
AWS (requires knowing the Start Url of the target), google cloud (requires a code from the login site for the last step, but this can be phished as well), discord, twitch, etc. are all vulnerable to
device code phishing. For mature organizations with certificate based authentication, device code phishing is one of the few ways to gain access without using malware. I've seen great success
with it for Microsoft, even for phishing higher tiered accounts (ie Azure admin accounts) by asking the user to login with their privileged account in the phishing message.
