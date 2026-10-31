# IDOR-Vulnerability-in-HamroCSIT

Status: Fixed
Reported To : info@hamrocsit.com

# TL;DR
I found a IDOR vulnerability that allowed any unauthorized user to view other user's private information like their quiz reports, results, question answered/unanswered/what they answered, names and other information. I reported them and the issue was fixed. This writeup covers what IDOR are in general, how I found one and its impacts.

# Summary
Hamrocsit is an educational platform aimed to assist CS students of Nepal. It has its own various functionalities and among which is mock tests and quizes for entrance exams. A user can modify the identifier in the quiz report URL/request and access quiz reports belonging to other users without proper authorization. An attacker may be able to access other users' quiz information, including user's name, test reports, selected answers, correct/incorrect answers, total score, unattempted questions and other information in the reports page. This issue allows unauthorized access to multiple user's quiz reports.

IDOR stands for Insecure Direct Object Reference, which is a common security flaw in web applications and application programming interfaces (APIs). It happens when an app uses an identifier supplied by a user like a database ID, order number, or file name—to directly access an internal object without checking if that user has permission to see it or in some cases, modify it.

# Discovery
I am also a CS student and I was normally browsing the site, not looking for bugs at all, just doing some quizzes and analyzing my quiz results via their "generate report" feature. I noticed an id in the url as "/reports/{id}". Being somewhat involved in web security, my security instincts kicked in and I quickly opened Burp Suite and began testing with it. I made my own another test account and quickly made a garbage report just to get the report id. I switched back to my main account. I modified the parameter and boom, I was able to see my test acc's test reports, and  quiz data as mentioned above. Since this two accounts had no correlation at all, it should not have let me access it. In simpler terms, I was authorized as A, tried to view B's data and I did. The server did not properly checked if that data belonged to me in the first place. I then stopped tested and reported right away. 

# Proof of concept:
I have attached a few screenshot here as proof:
<img width="1117" height="797" alt="Screenshot 2026-09-07 175511" src="https://github.com/user-attachments/assets/06006b85-33e3-43db-b63c-bd0c9fa3adea" />
<img width="1041" height="76" alt="Screenshot 2026-09-07 175338" src="https://github.com/user-attachments/assets/eff37422-6ecc-459c-a327-acf9c578c35f" />
<img width="507" height="196" alt="Screenshot 2026-09-07 175225" src="https://github.com/user-attachments/assets/f65d5441-c3be-4ba8-8776-fef551ce9620" />

# Impact
1)An attacker is able to access other users' quiz information, including user's name, test reports, selected answers, correct/incorrect answers, total score, unattempted questions and other information in the reports page. This issue allows unauthorized access to multiple user's quiz reports.
2)Severe Privacy Violations: Unauthorized viewing of individual test reports breaches user privacy, potentially violating strict academic data protection regulations or corporate confidentiality agreements.
3)Targeted Social Engineering: Malicious actors can use specific performance data, names, and unattempted question metrics to craft highly convincing, targeted phishing attacks against users.

# Timeline
Vulnerability found: August 4, 2026
Vulnerability Reported: August 4 , 2026
Vulnerability fixed: August 5, 2026
Writeup written: September 9, 2026

# Remediation:
1)Enforce Server-Side Object-Level Access Control
2)Implement Indirect Object References (ID Mapping)
3) Use Server-Side Sessions for Context

# Lessons
ALWAYS trust your instincts. If something feels weird, then it probably is. Sometimes the the vulnerability doesnot need complex payloads or business logic but just some id tampering!


