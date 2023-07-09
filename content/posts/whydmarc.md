---
title: "Why DMARC?"
date: 2023-07-08T00:19:46+12:00
draft: false
cover:
    image: img/dmarc-policy.png
    alt: 'DMARC Policy Image'
    caption: ''
tags: ["DNS","DMARC","Email"]
categories: ["InfoSec"]
---

## What is DMARC?

In simple terms DMARC is email protection. It is the next evolution in technologies that have been layered on top of the basic SMTP email protocol in an effort to improve email security. It stops attackers sending email from your domain. Commonly called domain spoofing. Fixing SMTP just isn't possible. Email is so prevalent that getting everyone to switch to something new would be almost impossible. Layering on top of SMTP allows for the gradual adoption of new technologies without breaking old ones. Let's explore why you should care about DMARC.

DMARC is made up of 3 technologies:

* **Sender Policy Framework** SPF is a DNS record that you create, that tells the world these are the places my domain sends email from. This should include your mail server but also your website if it uses contact forms, bulk email platforms like Sendgrid and any other web applications that send email using your domain. SPF provides a method for indicating to mail servers checking your DNS records how strictly to adhere to your list of sending servers. This is the policy bit. It appears at the end of the SPF record and commonly looks like this ~all or -all. The tilde is a softfail - don't reject the message but it's likely SPAM. Dash means hardfail - reject this message if it doesn't match any of the sources listed here.

* **DomainKeys Identified Mail** DKIM comprises one or two DNS records that you create and a feature you turn on for any service you use to send email. There is a lot that happens under the hood to make this work, but the reality is, this is all most people will need to do. DKIM cryptographically signs email when it leaves the mail server. It uses a public private key pair to do this. The private key staying on the server, the public key is made available by the DNS records on your domain. Mail servers receiving email from your domain can check the validity of the signature on the email using the public key in your DNS record. DKIM exists to solve a problem that SPF can't address. Sometimes email will pass through other mail servers on it's way to the destination domain. This is done for legitimate reasons. These other mail servers won't be in the sending domains SPF record so the receiving domain has no way of checking that the server it received the email from is legitimate or not. DKIM solves this problem by having the authentication mechanism (the signature) travel with the email allowing it to pass through any server on it's way to its destination with the authentication intact. If we only rely on SPF all email received via third party mail servers would be marked as SPAM or rejected.

* **Domain-based Message Authentication** DMARC is the bit that holds SPF and DKIM together. They are required to make it work. DMARC is a DNS record you create. This DNS record does two things: It tells mail servers receiving email from your domain what to do with email that doesn't meet your policy. It also provides a feedback mechanism for any domain receiving email from your domain to tell you how your email stacks up against your DMARC policy. It's this feedback mechanism that is pure gold and really makes the effort of setting up DMARC worth while. DMARC Reports are sent in a machine readable format so you will want to use a service to turn this formate in to something useful.

The policy defined in your DNS record has 3 states:

**None** do noting. Just send me the reports.

**Quarantine** receive email that doesn't meet my policy but mark it as SPAM.

**Reject** reject email that doesn't meet my policy.

Implementation normally starts at "none" with the goal of getting to "reject".

DMARC isn't hard to implement but does require a little bit of technical know how. The value to a domain is enormous though.

* It provides visibility into where your email to being sent from. This is great for spotting shadow IT and inventorying your IT assets.

* Improves email deliverability as DMARC helps receiving domains sort the good email from the bad coming from your domain.

* Helps stop attackers from abusing your domain.

DMARC is now widely supported by services sending email, particularly the more difficult to implement DKIM portion. DMARC report processing services are often free for a single domain with modest amounts of email and these same products provide solutions for even the most complex email systems. If your organisation hasn't implemented DMARC yet it's time to add it to your to do list. Even if you leave your DMARC policy set to nome you get increased visibility into where your email is being sent from and that can only be a good thing.
