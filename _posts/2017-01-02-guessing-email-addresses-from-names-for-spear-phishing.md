---
title: "Guessing Email Addresses from Names for Spear Phishing"
categories: 
  - phishing
tags:
  - recon
  - spear phishing
  - email
toc: false
---

Spear phishing, a targeted form of phishing, is a major security threat for all organizations. Both cybercriminals and nation-state sponsored hackers favor this method. Thus, it is no surprise that this attack vector plays an important part in penetration tests. Email is the most common method for delivering spear phishing messages. Thus, obtaining a list of emails is essential for carrying out a successful spear phishing attack. Ususally, organizations try to keep their email addresses private for this very reason. However, employees often publish their employment status on social media, such as LinkedIn. Organizations tend to have predicatable email addresses for employees, such as kevin.wang@example.com. This makes it possible to convert names into emails, which can be used later in a spear phishing attack.

First, theHarvester is used to obtain a list of employee names from LinkedIn:
```
theHarvester -l 500 -b linkedin -d clemson
```
Next, to figure out the email naming algorithm, an email and corresponding name must be found. An easy way to do this is to use theHarvester to search for emails rather than names.
```
theHarvester -l 500 -b google -d clemson.edu
```
For example, lets say that the harvester returns "kwang2@clemson.edu." If the pattern is not immediately obvious, Google the email to try to find the name of the owner. Hunter.io is another option. 
<figure>
	<a href="/assets/images/402b34a8cc9da2698204ef70ef2a2376.png"><img src="/assets/images/402b34a8cc9da2698204ef70ef2a2376.png"></a>
</figure>
In this case, the email naming convention is first initial + lastname + number(optional).

Finally, once the email naming convention is discovered, the names must be converted to emails. I wrote a simple program in node.js to perform this, including email verification. The emails can be validated by attempting to send an email (which never actually gets sent). Many email servers will respond with 250 if the email exists and 550 if it does not. The tool is called EmailGuesser and has a MIT license. 

[https://github.com/TheKevinWang/EmailGuesser](https://github.com/TheKevinWang/EmailGuesser)

Using EmailGuesser, we can easily convert the names to emails, and even validate the emails.
```
node EmailGuesser.js -f names.txt -d example.com -t {{fi}}{{ln}}@{{domain}}
Kevin Wang <kwang@example.com>
...
```
 Now we have a nice list of names and thier corresponding emails, which is perfect for a spear phishing attack.