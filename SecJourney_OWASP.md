# Application Security Training: OWASP Top 10

## Learning Outcomes
1. List the OWASP Top 10 threats to web applications.

2. Explain how to use the OWASP Top 10 for application security.

3. Define broken access control, cryptographic failures, and injection attacks and their mitigations.

### Why do we care?
The OWASP Top 10 is the most well-known application security document and contains the industry definitive list of security risks
OWASP Top 10 aggregates threats applications face, specifically web applications. The community has collected a bunch of data on these types of attacks. They collected information on their prevalence and impact when these threats are exploited. They also sent a survey to the community, asking about important things they needed to know. They gathered all this information and updated the old OWASP Top 10 list with the newest data to ensure we focus on the right areas to protect our applications.

### OWASP Top 10 2021
Broken Access Control, Cryptographic failures, Injection, Insecure Design, Security Misconfigurations, Vulnerable and Outdated Components,  Identification and Authentication Failures, Software and Data Integrity Failures, Security Logging and Monitoring Failures, Server-Side Request Forgery
One of the crucial things to understand with this list is that it is restructured from the old list and covers the top threats. Before, injection was the number one threat. Now, it's been bumped down to number three, and broken access control is the top threat. Number 10 used to be logging, monitoring, and failing. Now, that threat has moved up to number nine. Number 10 is now server-side request forgery—and data did not even bring this one in. The community brought it in by votes, saying it was important for us to understand. It is an entirely new restructured list. Our first lesson in our three-part series will cover the first three threats.

### OWASP Top 10 as a Standard
This is a OWASP Top 10 awareness document. 
The OWASP Top 10 is not a standard but an awareness document. We can use it to learn about these threats and consider their mitigations. It is great as a guide for identifying threats in a threat model or something like that. It is an excellent list of threats for proper threat modeling.

It is not a standard of how to build our application. There are great standards out there. One example is the application security verification standard, which is more detailed and instructional. It should be referenced when using and applying an actual standard for application. We should leverage the OWASP Top 10 to ensure our entire community is aware and educated on the different types of threats out there.

## Broken Access Control
User acess the api endpoint after GET request through access controls. Attacker also accessing the API endpoint and receiving 200OK after DELETE request. 
OWASP Top 10 Number One: Broken Access Control. Access control limits access to certain functionality, sensitive data, or components within our systems. We can have anything from a function-level access control, where only certain processes or people are allowed to use certain functions, all the way up to having access control built into the entire system. We need to validate that you are who you say you are, and then we will allow you to access something once we are sure you have the right privileges. When access control is broken, we are allowing attackers to access data that should be restricted.

Often, access controls can be missing or improperly implemented. Some excellent examples of this are not having a URL checked to ensure access to a certain resource. For example, someone can input and directly access a URL, bypassing a standard access control mechanism. The important part is that we must protect sensitive data and components. When we have broken access control, that data becomes available to attackers.

### Risks
Risks includes Unauthorized information disclosure, Modification/ destruction of data, Elevation of privilege.
Allows someone to do an unintended action.
We can take our access control risks and break them down into three different categories.

The first one is unintended information disclosure. The attacker is going to be able to find information about our company. It may be health data; it may be financial data. We are trying to protect some data the attacker can access because we have improper access control.

The second one is the modification or destruction of data. If an attacker can modify data, they can destroy data that has value to us. They could even go in and destroy logs. If they are in our system, they can have access control to those logs. An attacker could cover up the fact they were ever there, eliminating the traces. Controlling access to our sensitive data and our logging mechanisms is important.

Last is the elevation of privilege. If an attacker can bypass access control, they could gain access or elevated privilege within a system they should not have. They could have admin access within our application if they bypass that access control. The entire premise is that someone can do something that they are not supposed to be able to do.

### Mitigations
Mitigations includes Implement access control as trusted server-side code. Utilize a single access control mechanism. Rate limit API and controller access. Deny access by default. State should invalidate on server after logout. 
There are many steps we can take to help ensure we do access control properly. First, always do it on the server side. Client-side access control mechanisms can be bypassed, especially within web applications, because we can use a proxy to capture that traffic and manipulate it before it goes to the server. Always have access control implemented server-side.

We want to make a single point for access control. We do not want to have a bunch of different access control mechanisms spread out through our application if we can avoid it. It gives us more points of failure or chances of doing it wrong.

We want to do things like rate limit our API or controller access. We can rate limit if someone tries to do a brute force-style attack to bypass and access this control mechanism. Now, we are not saying close out accounts necessarily. However, if someone can only make a specific request over a certain amount of time, a standard brute force attack where an attacker could make a thousand requests within a few seconds gets dramatically reduced. It makes the attack much less effective.

Denied by default is another excellent principle to apply here. The idea is that if something fails within an authorization check, then the default is denied. That way, we do not end up breaking or bypassing the access mechanism, and then we have a default where things open up and have access. Always lean towards denied by default.

Another thing we want to do is whenever we have access control, once a session ends, we need to invalidate it on the server side so that that state of having access control is shut down. It will help stop repeat attacks where someone steals an ID from a session and tries to replay that attack.

## Cryptographic Failures
User transmit data in a clear text or with old and weak cryptographic algorithms to customer database.
OWASP Top 10 Number Two: Cryptographic failures. Cryptographic failures are either when we forget or use outdated or improper cryptography when protecting our data. That leaves our data still vulnerable to being attacked. For example, when we have web traffic, we want to make sure that we use TLS and HTTPS TLS rather than traditional HTTP-style traffic. Cryptographic failures could also be using outdated algorithms like MD5 hashes to protect our data. It would be best to start using something much more complex and advanced algorithm, like AES 256.

### Risks
Attacker can access Passwords, Personal Information, Credit Card Numbers, Health information, Business data, Financial Records because weak cryptography. 
The risks of Cryptographic failures are many. Cryptography is used to protect sensitive and vital data. This data could be things like our passwords, personal information, business data, or financial records. When we store or transport data from one place to another, anything that is sensitive data should be encrypted. When we fail or do this improperly, we expose all that data to the attackers.

### Mitigations
Mitigations includes Classify dataand use appropriate controls.Discard unnecessary data.Use up-to-date cryptographic algorithms.Disable caching sensitive data.Use authenticated encryption.
There are a lot of different strategies we can take to ensure we are doing cryptography appropriately. One of the first steps is identifying and classifying our data based on sensitivity levels. The idea is that not all of our data is sensitive. However, we need to know where our sensitive data is so that we are applying the appropriate cryptography to protect that data.

Along the same lines, identifying our information eliminates the excess data we do not need. The more data we retain, the bigger our attack surface is. If we can eliminate excess data, we can reduce that attack surface. A lot of the time, we collect a lot of information, but we use only some of it. If we are not going to use it, get rid of it.

The next step is to use an up-to-date cryptographic algorithm, something like AES 256, or, when thinking about password hashing, something like Argon2, which is community-verified to be one of the best algorithms for password hashing. We want to ensure we are implementing those algorithms; we especially do not want to create our type of cryptographic algorithms because we want to use algorithms tested by the community.

We also want to turn off caching. We do not want to store sensitive or cache sensitive data if we do not have to. It just adds to our attack surface. We want to do whatever we can to reduce that attack surface.

Finally, we also want to use authenticated encryption. That combines an encryption scheme and a message authentication code. The MAC is a short piece of information used to confirm that the message came from the stated senders. It is authentic and has yet to be changed. It adds a layer on top of encryption that validates it is coming from a safe user and that its integrity has stayed the same. It is a good thing we can implement when we are transporting data.

## Injection
Attacker targetting Application Interface to perform 
XML Injection, XSS, OS Command, SQLi
OWASP Top 10 Number Two: Injection. The injection category does not just cover one type of injection attack; it combines a few from the old list. They moved cross-site scripting into the injection category because it is one of the types of injection attacks.

SQL injection is also included in this category. That is where we take in malformed SQL data that tries to manipulate what will happen in the database to either extract data, delete data, or gain unauthorized access.

It also includes XML injections. We can take an XML to validate what is inside that file. If we deserialize or serialize it improperly, this can open us up to an XML-style injection attack that can execute commands on our system.

Again, cross-site scripting was added to this category. This is where an attacker injects JavaScript into our application that either gets stored in the database or reflects information to the user. It can redirect them to a malicious site or steal their session data.

Finally, we also have OS command injections. This is where an attacker injects an operating system command into our application and has it execute. This could do things like read the password file on a Linux machine or even shut down the system in a denial of service. There are many ways that attackers can inject malicious data to have an uninspected result in our application.

### Risks
Risks includes Authentication Bypass User or administrative authentication bypass.Malicious Code ExecutionAttacker can insert or execute code in unintended places.Data ExfiltrationAttacker extracts data via a legitimate application interface.
There are many risks to injection. One of the first ones is authentication bypass. We use a lot of mechanisms like SQL to be able to retrieve user information from the database that we then use to validate the user's identity and then allow them access to something. If an attacker can manipulate that to come back true, no matter what happens in some injection attack, they can bypass that authentication mechanism.

The second risk is malicious code execution. This happens in attacks like OS command or XML injection, where an attacker can execute something you do not intend to. This will allow them to have remote execution power on our system, something we do not want attackers to have.

The final risk is data exfiltration. An attacker can extract all information from our actual system or drop the entire database on a table and extract that information. We don't want to allow just anybody to be able to do access our data. It would be best if they were a DBA or somebody supposed to be on our system accessing data, not just anybody.

### Mitigations
Mitigations includes Use an ORM correctly.Implement safe-list, server-side input validation.Escape special characters.Limit database queries.
There are a lot of different strategies we can take to help prevent injection attacks. One of my favorites is leveraging ORMs and FRMs. An ORM is an object-relational mapper. An FRM is a functional relational mapper. They allow us to abstract from creating a customized SQL statement to interact with the database. Instead, we provide the data we need. Then, it retrieves that data from the database for us without having to do any SQL concatenation or other things that open up for SQL injection attacks.

Next, we want to use safe-list input validation on the server whenever possible. That is an extremely restrictive way to approach this problem, but the more restrictive we approach it, the more defense we have. The important factor is that we want to do it server-side, do input validation, and, if we can, default to a safe list over doing something like a dangerous list.

Our next mitigation strategy is to escape dangerous characters. This can be for our command injections, as well as for our SQL-style injection, and even cross-site scripting attacks. Suppose an attacker passes data with dangerous special characters that could cause unexpected actions on the system. In that case, we should escape those and either remove them or make them their safe alternatives before we use that data.

The last thing we can do is limit database queries. If someone tries to use an SQL statement to extract all the data from our database, it is limited and only allows one query at a time, so that will not be possible. That is another layer we can add to this defense-in-depth technique.

## Key Takeaways
The OWASP Top 10 list is best used as an awareness tool for securing web applications.
Implement access control using a deny by default approach.
Classify data by sensitivity and implement appropriate cryptographic controls.
Use an ORM, input validation, and escape characters to mitigate injection attacks.
