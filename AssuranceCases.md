# Assurance Cases for Software Security Engineering
## Part 1

<!--- Josh Bartels --->
### Assurance Case 1: Authenticate and Authorization (BIND)

![Assurance Case 1](https://github.com/bartelsjoshuac/SAPG/blob/main/images/BIND%20Assurance%20Case.svg)

**Top-Level Claim:**
LDAP minimized unauthorized access by authenticating the user

**Sub-Claim 2:** Password 
The user authenticates via a simple password.

**Inference Rule 1:**
Unless Password polices were not enabled and a weak password was used.

**Undercut L**
This is still allowed and we at least know the actors IP address

**Evidence 2:**
Audit log with IP

**Inference Rule 2:**
Pass thru authentication delegated the authentication to some other system and we don’t know if we can trust it

**Evidence 3:**
Audit log with system event log

**Sub-Claim 3:** Certificate
The user authenticates via x.509 certificate.-

**Evidence 1:**
Audit log

**Rebuttal 1:**
The user performed an anonymous BIND which is a special feature of LDAP where we can not differentiate user (this may be disabled)

**Evidence 2:**
Audit log with IP

---
<!--- End- Josh Bartels --->

<!--- Start- Eliya Ablet --->
### Assurance Case 2: Creating New Claims (ADD)

![Assurance Case 2](https://github.com/bartelsjoshuac/SAPG/blob/main/images/ADD.svg)

**Top-Level Claim:**
"The ADD operation in OpenLDAP, within a banking environment, guarantees the secure and accurate addition of new entries to the directory, upholding the highest standards of data integrity and access control."


**Rebuttal RB1:** unless system misconfiguration could cause non-complainant data entries 

**Sub-Claim 2:** Attribute Validation and Compliance. 
"OpenLDAP enforces strict validation of attributes to ensure compliance with banking-specific data schemas and regulatory requirements."

**Inference Rule:** If an attribute is not in compliance with the defined schema, the ADD operation will be rejected.

**Context:** In a banking environment, adherence to specific data standards and regulatory requirements is critical for compliance and data consistency.

**Evidence:** Documentation demonstrating how OpenLDAP enforces attribute validation against banking-specific schemas and regulations.

**Rebuttal RB2:** unless unauthorized access by attackers 

**Sub-Claim 3:** Role-Based Access Control (RBAC) Enforcement:
"The ADD operation in OpenLDAP applies Role-Based Access Control to restrict entry additions to authorized personnel only."

**Inference Rule:** Only users with specific roles and privileges can perform the ADD operation.

**Context:** In a banking environment, ensuring that only authorized personnel can add new entries is crucial for data security and regulatory compliance.

**Evidence:** Documentation showing the implementation of RBAC policies in OpenLDAP.

**Rebuttal RB3:** unless data inconsistencies in directory

**Sub-Claim 4:** Data Encryption and Confidentiality:
"OpenLDAP ensures that all data related to the ADD operation is encrypted both in transit and at rest, safeguarding sensitive information from unauthorized access or interception."

**Inference Rule:** Data transmitted during the ADD operation is encrypted using secure protocols (e.g., SSL/TLS).

**Context:** In a banking environment, protecting sensitive information is paramount to prevent data breaches and maintain customer trust.

**Evidence:** Documentation highlighting OpenLDAP's encryption mechanisms for data in transit and at rest.

**Sub-Claim 5:** Transaction Integrity and Atomicity:
"OpenLDAP guarantees the atomicity of the ADD operation, ensuring that either the entire addition is completed successfully or no changes are made to the directory, preventing partial or inconsistent updates."

**Inference Rule:** If any part of the ADD operation fails, the entire operation is rolled back.

**Context:** In a banking environment, ensuring transactional integrity is critical to prevent data inconsistencies and maintain the accuracy of customer information.

**Evidence:** Documentation or references demonstrating the transactional integrity mechanisms in OpenLDAP.
<!--- End- Eliya Ablet --->


### Assurance Case 3: Delete (DEL)
<!--- Start - Samuel Schneider --->
![Assurance Case 2](https://github.com/bartelsjoshuac/SAPG/blob/main/images/DEL%20assurance%20case.drawio.svg)

**Top-Level Claim:**
LDAP prevents unauthorized DELs through various security mechanisms. Adequate security measures prevent unauthenticated, unauthorized, or by other means malicious actors from performing uncalled for DEL operations in the LDAP server. This applies at both the user and branch levels.

**Sub-Claim 2:** Authentication 
User authentication is required before any operations may be performed in the LDAP server. This prevents non-users from performing operations of any kind on the LDAP server.

**Evidence 1:**
Identifying information such as distinguished name and password required from all users before they may be permitted to perform any operation. This serves to identify users by linking the distinguished name with the associated password.

**Sub-Claim 3:** Permissions 
Permission restrictions prevent unauthorized users from performing unauthorized operations. By limiting the availability of this powerful functionality to only authorized users, the opportunity for malicious actors to perform unauthorized DELs decreases significantly. Furthermore, this allows for easier tracking of which user performed unauthorized DELs.

**Evidence 2:**
Implementing a user hierarchy allows privileges to be allocated based on the class that the user in question belongs to. This allows for convenient grouping of permission groups based on tiers of users.

**Rebuttal 1:**
Credential theft may be prevented by additional and more modern security features and mechanisms. Fortunately, LDAP servers allow for additional security measures to be implemented on top of already present features. The more security features that an organization adds on, the less prone they are to being the victim of a successful attack.

**Evidence 3:**
Examples of these security expansions include two factor authentication. The addition of this feature requires that all prospective users authenticate their login requests with a secondary device, typically a call phone. If a malicious actor is attempting to impersonate a legitimate actor, he will be foiled by this security feature since the legitimate actor will be able to deny the request remotely.

**Rebuttal 2:**
Damage caused by an attack involving stolen credential may be undone if the changes that each user makes are tracked. Tracking changes that each user makes allows administrators to diagnose which user is responsible for unrequested changes to the state of the LDAP server. This is useful in two ways. Not only does this allow administrators to disable hacked accounts, but it allows for action roll backs to be performed to restore the LDAP server to its correct state.

**Evidence 4:**
The presence of Roll Back Access Control allows for administrators to undo damage caused by a malicious actor using a hacked account. This allows for restoration of deleted or manipulated data and records so that the damage may be entirely undone.
<!--- End - Samuel Schneider --->


<!--- Start - Adam Stemmler--->
### Assurance Case 4: Modify (MDFY)

![Assurance Case 4](https://github.com/bartelsjoshuac/SAPG/blob/main/images/MDFY_Assurance_Case.drawio.svg)

**Top-Level Claim:**
OpenLDAP ensures the security of OpenLDAP server data. 

**Context CT1:**
It is crucial for OpenLDAP servers to maintain the confidentiality, integrity, and availability of data, both in transit and at rest, by employing all its suite of tools and proper server settings configurations.

**Rebutal R1:**
Unless there is an unauthorized modification of server data from an unathorized or unauthenticated user.

**Sub-Claim C2:**
Server enforces authentication with credentials, thus both authenticating and authorizing the user based on that user's identity.

**Context CT2**
Depending on configuration, servers can require quite sophisticated passwords, as well as multi factor authentication. OpenLDAP is capable of employing all the latest authentication and authorization standards.

**Rebuttal R4:**
Unless a user binds anonymously, thus skirting the need for authentication.

**Sub-Claim C5:**
Server enforces access control list checking, preventing anonymous users from modifying data. ACLs also ensure legitimate users can only modify data they have a need to modify.

**Evidence E1:**
OpenLDAP servers have a wide variety of quality authetication methods available for their use, and this server employs new, secure authentication techniques.

**Rebuttal R2:**
Unless OpenLDAP server data can be read by anyone, thus degrading its confidentiality.

**Sub-Claim C3:**
OpenLDAP servers can opt to hash its data, thus protecting it from being viewed by those who do not have the need to know.

**Rebuttal R5:**
Unless the network used for communications between the client and LDAP server are being sniffed and monitored.

**Sub-Claim C6:**
Server encrypts data in transit from the client to the LDAP server.

**Evidence E2:**
This server is configured to hash stored data.

**Evidence E3:**
This server is configured for encrypted communications with client.

**Rebuttal R3:**
Unless data is corrupted, thus degrading its integrity and availability.

**Sub-Claim C4:**
OpenLDAP servers can enforce schema checking; this server is configured thusly.

**Context CT3:**
Data and attributes must follow a structure set by the server (the schema); if it does not it would break the server and its data. Data is arranged in attributes, which have data points. These data points can change based on the schema and modification have to fit that schema. If the server is not configured properly, it would break the data. But OpenLDAP has a defualt schema and its configuration settings for the schema can be easily changed.

**Rebuttal R6:**
Unless user can change server attributes. Servers have server attributes, like loginAttempts, that help it keep track of data that it needs to function and employ other features. Normal users should not be able to change these lest it create chaos.

**Sub-Claim C7:**
This server is configured so server attributes cannot be changed by users. It is also configured so that the schema is properly enforced and bad modification requests are thrown away.

**Evidence E4:**
Server configuration settings. The server configuration settings for the server in question are the evidence for two of the final sub-claims.

**Evidence E5:**
The OpenLDAP documentation details all the OpenLDAP features and how to implement them. It is evidence for all three final sub-claims.
<!--- End - Adam Stemmler--->


<!--- Start - Md Monirul Islam--->
### Assurance Case 5: Search (SRCH)
![Assurance Case 5](https://github.com/bartelsjoshuac/SAPG/blob/main/images/SRCH_accurance_case.svg)

**Top-Level Claim:**
OpenLDAP securely enables to generation of a report containing all the employees who have accessed the sensitive data in the last couple of months.

**Rebuttal R1**: Unless the logging service is constantly checked, it might crash or malfunction, leading to periods with no logs.\
**Sub-claim C1**: OpenLDAP provides robust logging capabilities that record all access made to sensitive data. 
<!-- **Inference Rule 1**: If OpenLDAP’s logging audit functionality can reliably record of all access made to sensitive data then OpenLDAP provides robust logging capabilities of all access made to sensitive data. \ -->
**Evidence E1.1**: When logging functionality of OpenLDAP is enabled it can track and log all access made to directory information.

**Rebuttal R2**: Unless access controls are updated regularly, they might become outdated, leading to inappropriate permissions.\
**Sub-claim C2**: Access controls are configured properly to ensure only authorized personnel can access sensitive data.
<!-- **Inference Rule 2**: If access control policies are well-defined in OpenLDAP configuration and ensure fine-grained control, the access controls are configured properly in OpenLDAP to ensure only authorized personnel can access sensitive data.\ -->
**Evidence E2.1**: Access control policies are well defined in OpenLDAP’s configuration to ensure fine-grained control over who can access which data.

**Rebuttal R3**: Unless there is capacity for handling large volumes of logs, generating reports might result in missed entries or delays.\
**Sub-claim C3**: The report generated from the log audit is accurate and encompasses all access instances to the sensitive data. 
<!-- **Inference Rule 3**: If the logs are processed with well-defined and validated queries then the report generated is accurate and lists all access instances to sensitive data.\ -->
**Evidence E3.1**:  Scripts or tools that query OpenLDAP’s log for the required information use appropriate filters and the results are validated as accurate.

**Rebuttal R4**: Unless there's a robust log integrity checking system, an insider with the right permissions might tamper with logs undetected.\
**Sub-claim C4**:  Adequate security mechanisms are in place to prevent tampering with logs.
<!-- **Inference Rule 4**: If OpenLDAP logs are securely stored, access to them is restricted, and the integrity of the log is regularly verified, then security mechanisms are in place to ensure that logs are not tampered with. \ -->
**Evidence E4.1**: OpenLDAP’s logs are stored in a secure location with restricted access. \  
**Evidence E4.2**: Checksums or other integrity-check mechanisms are in place to verify the integrity of the logs.

<!--- Start - Awais--->
### Assurance Case 6: Filtered Employees email search (SRCH)

A building supervisor wants to search for the email address of all employees on 2nd floor in the Omaha HQ to notify them of a power outage.

![Assurance Case 6](https://github.com/bartelsjoshuac/SAPG/blob/main/images/SRCH.svg)

**Top-Level Claim C1:**
OpenLDAP can be utilized by the building supervisor to search for the email addresses of all employees on a specific floor in the Omaha HQ for power outage notifications.

**Rebuttal RB1:**
OpenLDAP contains email addresses which are not uptodate. 

**Sub-claim C2:**
OpenLDAP contains a comprehensive and up-to-date directory of employee information, including email addresses.

**Context CT1:**
OpenLDAP serves as the central repository for employee information, including email addresses. This information is kept up-to-date through regular synchronization processes, ensuring its accuracy and relevance for use in critical notifications.

**Evidence E1:**
Organization's IT policy or documentation specifying OpenLDAP as the authoritative source for employee information.

**Rebuttal RB2:**
Email search is not effective and can send send emails to employees which are on different floor. 

**Sub-claim C3:**
OpenLDAP provides robust search functionality, including the ability to filter employees by floor and location.

**Evidence E2:**
Configuration documentation or system specifications that detail the search capabilities of OpenLDAP, including filtering by attributes.

**Rebuttal RB3:**
Unless Supervisors don't know how to use filtering capability 

**Sub-claim C4:**
Supervisors are well trainined to use the LDAP's email filtering capabilities

**Evidence E3:**
Training materials or documentation provided to the building supervisors on using OpenLDAP's search functionality.

**Rebuttal RB4:**
SRCH functionality is not secure and anyone from the employees can access this list.

**Sub-claim C5:**
Access controls and security measures are in place to ensure that only authorized personnel, like the building supervisor, can access the email addresses retrieved from OpenLDAP.

**Evidence E4:**
Access control policies or documentation specifying who has access to employee email addresses and the permissions associated with it.

**Evidence E5:**
Evidence of security audits or assessments that confirm compliance with security policies.
<!--- End - Awais--->

## Part 2

## Shortcomings of OpenLDAP
NOTE that every action in LDAP has a response as LDAP is a RFC defined protocol.  OpenLDAP is a directory server implmentation which speaks the LDAP protocol.  Therefore, a BIND REQUEST will have a BIND RESPONSE for example.  These must be correlated in the audit log using the messageID using open source tools like logstash/Kabana which are not built in to OpenLDAP.  OpenLDAP uses CEF logging and provides no assistance in corelating a request with the response (outcome).

It is critical to handle the BIND or authentication process properly as this identifies the end user and begins the authorization process (apply ACLs).  Pass thru authentication is a loop hole that removes trust and the associated evidence from the use case and while it may establish identity, the method in which it was completed is indeterminate.  In the case of a authentication failure, the user may step back to an anonymous BIND, and authentication failure followed by a anonymous BIND from the same client must be treated as suspicious.  This requires looping back in the use case which is not possible in LDAP, hence leaving it incomplete as this would normally be the responsibility of a SIEM solution.  Although some LDAP varieties handle this, OpenLDAP does not.  All other cases are handled and evidence is logged appropriately for audit purposes.

While the SRCH report generation employs OpenLDAP's logging capabilities, evidence regarding the search capabilities of the SRCH function is unclear. It is important to ascertain if SRCH can efficiently and securely retrieve logs for queries and whether its outputs align with the report generation mechanism. This gap needs further attention.

While working on this OSS project we realized a few limitations of LDAP that may not meet all the requirements:
Lack of Full Transaction Support:
LDAP lacks full support for transactions. While certain operations within LDAP can be grouped together in a single transaction-like operation, it doesn't provide the same level of transactional integrity as some other database systems.

Limited Complex Query Capabilities:
LDAP is optimized for quick retrieval of information based on known attributes.  However, it may not be as suitable for complex querying operations that involve multiple conditions or advanced search capabilities.

Complex Access Control Lists (ACLs):
Defining and managing complex access control lists in LDAP can be intricate, particularly for large directory services with many users and groups. This complexity can sometimes lead to misconfigurations or unintended security gaps.

Lack of Native Data Validation:
LDAP does not have built-in mechanisms for enforcing data validation rules at the protocol level. It relies on schema definitions, which may not cover all possible validation requirements.

OpenLDAP does have a number of modern security features builtin. However, there are many others, like rate limiting, that it does not support natively. However, by its nature, OpenLDAP can integrate other third-party software to use features like rate lmiting with great ease; these are issues we took into consideration when building our assurance cases.

## Link to project board: https://github.com/users/bartelsjoshuac/projects/2/views/1
This has tasks for each use case and the other parts of the assignment as well, with team members assigned.

## Reflection on work:
This week we improved our communication, being able to get everyone in one palce at the same time for a meeting more often than previously. However, more improvement is needed for our communication. We were all able to meet on Monday to go over everything and review each other's work, offering suggestions and rethinking our own choices based on the dialogue, which was a good change from previous weeks where Mondays were often a mad dash to finish certain work, although we will continue to improve Monday meetings as well as adherence to our internal time lines and due dates apart from the official due date for the assignment. The meeting last week with the professor did help and gave us some much needed feedback which helped guide our decisions when creating our assurance cases. Although, there is room for improvement in our adherence to the instructor feedback and class materials when creating our project contributions, which can and will be improved through increased communication and dialogue in our meetings.
