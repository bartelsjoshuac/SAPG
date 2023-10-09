# Assurance Cases for Software Security Engineering
<!---Expecations of Assignment for reference --->
## Part 1
* Using your findings from the requirements assignment, develop five to six (depending on the number of team members) top-level claims. The number of top-level claims can be adjusted to the number of students in your team. List these claims in your report.

  * Top-level claims pertain to high-risk critical security properties  
  * Having the claims focus on unique entities and related security properties will allow you to have broad coverage of the assurance needs from your OSS project. 

* Prepare an argument for each top-level claim that would convince stakeholders in your assumed/hypothetical operational environment. Use this as a planning exercise to figure out what sort of assurances would be necessary. They don't have to match what is currently available in the open-source software project. Document the argument and the needed evidence using an assurance case for each top-level claim. 

  * You may use this sample diagram Download sample diagram (open in http://app.diagrams.netLinks to an external site.) to get quick access to all the shapes needed for an assurance case
  * Use proper notations and ensure proper wording of assurance case elements. Avoid typos and grammatical errors as they can be distracting to stakeholders reading the assurance case for making trust decisions. 
  * Assurance cases should have reasonable depth and breadth to convince a technical expert. It is expected that the argument is coherent and convincing. 

---

<!--- End - Expecations, this can be removed later --->

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

### Assurance Case 2: Creating New Claims (ADD)

**Top-Level Claim:**
"The ADD operation in OpenLDAP, within a banking environment, guarantees the secure and accurate addition of new entries to the directory, upholding the highest standards of data integrity and access control."

**Sub-Claims:**

**Sub-Claim 2:** Attribute Validation and Compliance. 
"OpenLDAP enforces strict validation of attributes to ensure compliance with banking-specific data schemas and regulatory requirements."

**Inference Rule:** If an attribute is not in compliance with the defined schema, the ADD operation will be rejected.

**Context:** In a banking environment, adherence to specific data standards and regulatory requirements is critical for compliance and data consistency.

**Evidence:** Documentation demonstrating how OpenLDAP enforces attribute validation against banking-specific schemas and regulations.

**Sub-Claim 3:** Role-Based Access Control (RBAC) Enforcement:
"The ADD operation in OpenLDAP applies Role-Based Access Control to restrict entry additions to authorized personnel only."

**Inference Rule:** Only users with specific roles and privileges can perform the ADD operation.

**Context:** In a banking environment, ensuring that only authorized personnel can add new entries is crucial for data security and regulatory compliance.

**Evidence:** Documentation showing the implementation of RBAC policies in OpenLDAP.

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

**Rebutal R1:**
Unless there is an unauthorized modification of server data.

**Sub-Claim C2:**
Server enforces authentication with credentials.

**Rebuttal R4:**
Unless a user binds anonymously, thus skirting the need for authentication.

**Sub-Claim C5:**
Server enforces access control list checking, preventing anonymous users from modifying data.

**Evidence E1:**
OpenLDAP servers have a wide variety of quality authetication methods available for their use.

**Rebuttal R2:**
Unless OpenLDAP server data can be read by anyone, thus degrading its confidentiality.
**Sub-Claim C3:**
OpenLDAP servers can opt to hash its data, thus protecting it from being viewed by those who do not have the need to know.


**Sub-Claim C3:**
Server enforces schema checking for modification requests.


**Rebuttal R3:**

<!--- End - Adam Stemmler--->

<!--- Start - Md Monirul Islam--->
### Assurance Case 5: Search (SRCH)
![Assurance Case 5](https://github.com/bartelsjoshuac/SAPG/blob/main/images/SRCH_accurance_case.svg)

**Top-Level Claim:**
OpenLDAP securely enables to generation of a report containing all the employees who have accessed the sensitive data in the last couple of months.

**Rebuttal 1**: Unless the logging service is constantly checked, it might crash or malfunction, leading to periods with no logs.\
**Sub-claim 1.1**: OpenLDAP provides robust logging capabilities that record all access made to sensitive data. 
<!-- **Inference Rule 1**: If OpenLDAP’s logging audit functionality can reliably record of all access made to sensitive data then OpenLDAP provides robust logging capabilities of all access made to sensitive data. \ -->
**Evidence 1.1**: When logging functionality of OpenLDAP is enabled it can track and log all access made to directory information.

**Rebuttal 2**: Unless access controls are updated regularly, they might become outdated, leading to inappropriate permissions.\
**Sub-claim 2**: Access controls are configured properly to ensure only authorized personnel can access sensitive data.
<!-- **Inference Rule 2**: If access control policies are well-defined in OpenLDAP configuration and ensure fine-grained control, the access controls are configured properly in OpenLDAP to ensure only authorized personnel can access sensitive data.\ -->
**Evidence 2.1**: Access control policies are well defined in OpenLDAP’s configuration to ensure fine-grained control over who can access which data.

**Rebuttal 3**: Unless there is capacity for handling large volumes of logs, generating reports might result in missed entries or delays.\
**Sub-claim 3**: The report generated from the log audit is accurate and encompasses all access instances to the sensitive data. 
<!-- **Inference Rule 3**: If the logs are processed with well-defined and validated queries then the report generated is accurate and lists all access instances to sensitive data.\ -->
**Evidence 3.1**:  Scripts or tools that query OpenLDAP’s log for the required information use appropriate filters and the results are validated as accurate.

**Rebuttal 4**: Unless there's a robust log integrity checking system, an insider with the right permissions might tamper with logs undetected.
**Sub-claim 4**:  Adequate security mechanisms are in place to prevent tampering with logs.
<!-- **Inference Rule 4**: If OpenLDAP logs are securely stored, access to them is restricted, and the integrity of the log is regularly verified, then security mechanisms are in place to ensure that logs are not tampered with. \ -->
**Evidence 4.1**: OpenLDAP’s logs are stored in a secure location with restricted access. \  
**Evidence 4.2**: Checksums or other integrity-check mechanisms are in place to verify the integrity of the logs.



<!--- End - Md Monirul Islam--->

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


<!--- End - Md Monirul Islam--->
## Part 2
<!---Expecations of Assignment for reference --->

* Assess the alignment of the evidence you identified in your diagrams with that available (or can be made available) from the OSS project. Highlight the gaps.
( Include a link to your team's GitHub repository that shows your internal project task assignments and collaborations to finish this task. 
* Include a reflection on your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward? 

---

<!--- End - Expecations, this can be removed later --->


NOTE that every action in LDAP has a response as LDAP is a RFC defined protocol.  OpenLDAP is a directory server implantation which speaks the LDAP protocol.  Therefore, a BIND REQUEST with have a BIND RESPONSE for example.  These must be correlated in the audit log using the messageID.  Open source tools like logstash/Kabana.  OpenLDAP uses CEF logging and provides no assistance in corelating a request with the response (outcome).


It is critical to handle the BIND or authentication process properly as this identifies the end user and begins the authorization process (apply ACLs).  Pass thru authentication is a loop hole that removes trust and the associated evidence from the use case and while it may establish identity, the method in which it was completed is indeterminate.  In the case of a authentication failure, the user may step back to an anonymous BIND, and authentication failure followed by a anonymous BIND from the same client must be treated as suspicious.  This requires looping back in the use case which is not possible in LDAP, hence leaving it incomplete as this would normally be the responsibility of a SIEM solution.  Although some LDAP varieties handle this, OpenLDAP does not.  All other cases are handled and evidence is logged appropriately for audit purposes.




