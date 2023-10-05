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

C1: LDAP will perform the authentication request properly and handle all scenarios possible.

IR1: Pass thru authentication is a bit of a wildcard, and if enabled circumvents/interferes the anticpated process.

UC1: The process is further undercut as it will return a response which LDAP must assume to be true, but further examination is needed

SC1: Indterminate

R1: Assume ass thru authentication is not enabled.

SC2: Assume the user has a password.

E1: Authentication and log success.

R2: Unless the user is disabled

SC3: The user can not log in

E2: Log the failure and end

R3: Password was not valid.

E3: Log the failure and retry

IR2: Anonymous Authentication occurred

UC2: Default to Read only or deny

SC4: Indeterminate handling as the client should not do this but can on E3 if desired (skeptical) 

---

<!--- End- Josh Bartels --->

### Assurance Case 2: Creating New Claims (ADD)

**Top-Level Claim:**
"The ADD operation in OpenLDAP, within a banking environment, guarantees the secure and accurate addition of new entries to the directory, upholding the highest standards of data integrity and access control."

**Sub-Claims:**

**Sub-Claim 1:** Attribute Validation and Compliance. 
"OpenLDAP enforces strict validation of attributes to ensure compliance with banking-specific data schemas and regulatory requirements."

**Inference Rule:** If an attribute is not in compliance with the defined schema, the ADD operation will be rejected.

**Context:** In a banking environment, adherence to specific data standards and regulatory requirements is critical for compliance and data consistency.

**Evidence:** Documentation demonstrating how OpenLDAP enforces attribute validation against banking-specific schemas and regulations.

**Sub-Claim 2:** Role-Based Access Control (RBAC) Enforcement:
"The ADD operation in OpenLDAP applies Role-Based Access Control to restrict entry additions to authorized personnel only."

**Inference Rule:** Only users with specific roles and privileges can perform the ADD operation.

**Context:** In a banking environment, ensuring that only authorized personnel can add new entries is crucial for data security and regulatory compliance.

**Evidence:** Documentation showing the implementation of RBAC policies in OpenLDAP.

**Sub-Claim 3:** Audit Trail and Logging:
"OpenLDAP maintains a comprehensive audit trail and logging mechanism for all ADD operations, providing a transparent record of changes for accountability and regulatory compliance."

**Inference Rule:** All ADD operations are logged, including the identity of the requester, the timestamp, and the details of the operation.

**Context:** In a banking environment, having a detailed audit trail is essential for compliance with regulatory requirements and for tracking any suspicious or unauthorized activities.

**Evidence:** Documentation demonstrating the audit trail capabilities of OpenLDAP.

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

![image](https://github.com/bartelsjoshuac/SAPG/assets/104176036/5ad19444-2a1d-4bbe-90b9-443a34fce4ac)

### Assurance Case 3: Delete (DEL)
<!--- Start - Samuel Schneider --->
![Assurance Case 2](https://github.com/bartelsjoshuac/SAPG/blob/main/images/DEL%20assurance%20case.drawio.svg)
<!--- End - Samuel Schneider --->
### Assurance Case 4: Modify (MDFY)


## Part 2
<!---Expecations of Assignment for reference --->

* Assess the alignment of the evidence you identified in your diagrams with that available (or can be made available) from the OSS project. Highlight the gaps.
( Include a link to your team's GitHub repository that shows your internal project task assignments and collaborations to finish this task. 
* Include a reflection on your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward? 

---

<!--- End - Expecations, this can be removed later --->


NOTE that every action in LDAP has a response as LDAP is a RFC defined protocol.  OpenLDAP is a directory server implantation which speaks the LDAP protocol.  Therefore, a BIND REQUEST with have a BIND RESPONSE for example.  These must be correlated in the audit log using the messageID.  Open source tools like logstash/Kabana.  OpenLDAP uses CEF logging and provides no assistance in corelating a request with the response (outcome).


It is critical to handle the BIND or authentication process properly as this identifies the end user and begins the authorization process (apply ACLs).  Pass thru authentication is a loop hole that removes trust and the associated evidence from the use case and while it may establish identity, the method in which it was completed is indeterminate.  In the case of a authentication failure, the user may step back to an anonymous BIND, and authentication failure followed by a anonymous BIND from the same client must be treated as suspicious.  This requires looping back in the use case which is not possible in LDAP, hence leaving it incomplete as this would normally be the responsibility of a SIEM solution.  Although some LDAP varieties handle this, OpenLDAP does not.  All other cases are handled and evidence is logged appropriately for audit purposes.




