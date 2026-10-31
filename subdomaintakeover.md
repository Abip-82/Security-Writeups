# Subdomain Takeover on link.arsenic.hackclub.com
Status: Fixed
Reported to: Hack Club Security 
Disclosure: Responsible disclosure, published with permission from Hack Club Security

# TL;DR
Found a dangling DNS record on a *.hackclub.com subdomain that let me take control of the subdomain and serve my own content on it. Reported it through Hack Club's bug bounty platform; the team fixed it within one day. This writeup covers how I found it, how I confirmed it, and what subdomain takeovers are in general.


# Background
Hack Club runs a large number of programs and community projects, each often getting its own subdomain under hackclub.com (e.g. ysws.hackclub.com, slack.hackclub.com). Subdomains are provisioned through a public DNS repo where CNAME/A records point to whatever service the program is using — Vercel, Netlify, GitHub Pages, custom infra, etc.

When a service is retired but the DNS record pointing to it isn't cleaned up, you get a dangling DNS record — and that's the root cause of a subdomain takeover.

# Discovery
While doing subdomain enumeration against *.hackclub.com using subfinder ( a subdomain enumerating command line tool. I paired it along with other tools such as dnsx and httpx), I found link.arsenic.hackclub.com resolving via CNAME to xyz.short.io, which was unclaimed.
After getting a 404 domain not found output from httpx, I used a tool called dig to dig CNAME up which resolved to a third party service provider Short.io. It was unclaimed, meaning anyone could claim the domain by configuring short.io domain settings. 

# Verification / Proof of Concept
I registered the unclaimed resource on Short.io and pointed it to my own page (i.e. I configured it such that anyone who visits the subdomain gets redirected it to my own site) confirming control of the subdomain(link.arsenic.hackclub.com), without altering or accessing any other data.

Video PoC:
https://github.com/user-attachments/assets/4657e748-51f4-40a6-96cf-5bc394fe4339

# Impact
A subdomain takeover on a trusted domain like hackclub.com could be used for:
- Phishing and Social Engineering: An attacker can host convincing phishing pages on a trusted subdomain, increasing the likelihood that users will disclose  sensitive information or credentials.
- Brand Reputation Damage: Malicious or inappropriate content served from the organization's subdomain can undermine user trust and negatively impact the organization's reputation
- Malware Distribution: A compromised subdomain can be used to host or distribute malware, potentially exposing users to malicious downloads while leveraging the trust associated with the parent domain.
- Security Control Bypass: Trusted subdomains are often allowlisted by browsers, email gateways, or enterprise security solutions. An attacker may abuse this trust to evade certain security controls and increase the effectiveness of malicious campaigns.
- Loss of Domain Integrity: Control of an organizational subdomain enables attackers to impersonate legitimate services, deceive users, and abuse the organization's domain for unauthorized or malicious activities, increasing overall security risk.

# Timeline
Vulnerability discovered - July 15, 2026
Issue Reported - July 15, 2026
Issue Resolved - July 15, 2026
Disclosure published - July 19, 2026

# Remediation
Hack Club removed the dangling DNS record, resolving the takeover risk.

# Lessons / Takeaways
Old, "forgotten" subdomains are exactly where this class of bug hides — DNS hygiene needs to cover retired projects, not just active ones
Organizations with crowdsourced or public DNS provisioning (like Hack Club's open DNS repo) are especially worth checking for this, since there's a large surface of subdomains created over time by many different contributors.

# Thanks
Thanks to the Hack Club Security team for the fast response, the fix, and for permitting this disclosure.
