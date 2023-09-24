# Requirements for Software Security Engineering

## Part 1

<!--- Josh Bartels --->
### Use Case 1: Authenticate and Authorization (BIND)
The BIND operation identifies the actor to the server.  LDAP will typically allow anonymous BIND operations which may or may not be disabled, depending on business requirements.  ACL’s are applied to the actor that is bound to the system when the BIND is successful.  Binding anonymously would typically be configured with read only access, as there would be no accountability of changes.  Where binding as cn=admin often applies no ACL’s at all.  In this case, we will assume a normal BIND from a standard system user with a password, which would be authenticating to a website via Single Sign On (SSO) software via a website, logging into a Linux server, etc.

#### Use:


![Use-Case-1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use%20Case%201%20-%20Bind.drawio.svg)

The actor wishes to identify themselves to the server and will do so by providing a valid Distinguished Name (DN) and simple password to establish their identity to the LDAP server and perform a BIND.  The LDAP server will then wait for additional operations which will be explored in later uses cases that build on this.

#### UseMisuse:

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case%201%20-Bind.drawio.svg)

The bad actor could sniff the password on the wire on the known port (389).  

Once authenticated, with anonymous access they could also read another user's password attribute.    

Given that weak passwords are in use, a brute force or password spraying attack could discover a password.  

#### Misuse Remedy:

OpenLDAP supports SSL/TLS based on the OpenSSL package that is installed on the host machine.  A strong TLS encryption method should be selected to deter sniffing passwords during BIND requests.    

OpenLDAP can store passwords in clear-txt, encrypted strings, or hashes.  It is suggested that DIGEST-MD5 is used, however SHA, CRYPT, MD5, SMD5, and SASL are offered.  A strong encryption algorithm should be chosen.  Pass-thru authentication is also an option so that passwords are not stored in LDAP at all, this is how a MFA solution would be implemented for the later attack vector (brute force or password spraying).    

OpenLDAP has a [dynamically loaded password policy available.]( https://tobru.ch/openldap-password-policy-overlay/).  A password policy enforcing a minimum length, character set, and complexity should be configured to limit successful brute force or password spraying attacks.  Note that this module also includes the login attempts and lockout.  

The documentation on the configuration of any of these options is not very comprehensive, nor is it part of the setup script which may cause administrators to overlook them, possibly assuming they are implemented by default as they would be in commercial implementations.  

![Misuse-Remedy-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case-Final1%20-Bind.drawio.svg)

<!--- End --->
---
### Use Case 2: An administrator wants to create a new employee record with basic white page information and set a temporary password that the user must change at login. (ADD)

An ADD will follow the BIND use case to identify the actor, evaluate relevant ACLs, and determine if the user has the authority to add this type of record.  If they do, it will then check that the ADD request complies with the schema, e.g. required attributes, optional attributes, no system attributes. So a bad actor could try and add something that already exists (modify), something they are not allow to add, something that violates the schema definition, 

#### Use:

The actor ADD a new user with a valid Distiguished Name (DN) and a valid password to the LDAP server with following steps:
The actore logs in to the LDAP server with appropriate privileges.
They navigate to the appropriate organizational unit (OU) where user accounts are managed.
They create a new entry for new employee, including attributes such as cn (common name), uid (user ID), givenName, sn (surname), mail (email address), userPassword, etc.
They set the password for new employee's account and configure any necessary group memberships or access permissions.
The new employee is now able to log in to company systems using his LDAP credentials.

#### UseMisuse1: Unauthorized Access and Data Manipulation

Scenario:

The Malicious Insider has gained unauthorized access to the LDAP administration interface.
Main Flow:
The Malicious Insider logs in using stolen credentials or exploits a vulnerability.
The Malicious Insider selects the "Add Person" option.
The Malicious Insider fills out the required fields with false or malicious information.
The Malicious Insider submits the form.

#### UseMisuse2: Injection Attack

Scenario:

The External Attacker has identified a vulnerability in the LDAP server's input validation.
Main Flow:
The External Attacker exploits a vulnerability to bypass input validation.
The External Attacker injects malicious code or special characters into the input fields, potentially compromising the LDAP server.
The External Attacker submits the form with the injected data.

#### UseMisuse3: Denial-of-Service (DoS) Attack

Scenario:

The Malicious Attacker aims to disrupt normal LDAP server operations.
Main Flow:

The Malicious Attacker floods the LDAP server with a high volume of requests to add persons.
The server's resources become overloaded due to the excessive processing demands.
The LDAP server becomes unresponsive, impacting legitimate users' ability to access or modify directory information.

#### Misuse Remedy:

Unauthorized Access and Data Manipulation:
Above attacks can be prevented by following remedies:
Review and update access control lists (ACLs) regularly to ensure that only authorized users have the necessary permissions to perform LDAP operations. Configure proper permissions based on roles and responsibilities. Implement role-based access control (RBAC) to grant specific privileges to different user groups. Use LDAP groups and memberships to manage access. Enforce strong authentication mechanisms. Implement multi-factor authentication (MFA) to add an extra layer of security.

Injection Attack:
Implement robust input validation and sanitation techniques. Use parameterized queries to prevent SQL injection attacks. Sanitize input data to remove potentially harmful characters. Keep the LDAP server software and related libraries up-to-date. Regularly check for security updates and patches provided by the LDAP server's vendor. Apply patches promptly to address known vulnerabilities.

Denial-of-Service (DoS) Attack:
Implement traffic filtering and rate limiting to protect against DoS attacks. Use firewalls and network appliances to filter traffic and block suspicious requests. Configure rate-limiting rules to restrict the number of requests from a single source. Implement DoS detection and mitigation strategies. Use intrusion detection systems (IDS) or intrusion prevention systems (IPS) to detect and block suspicious traffic patterns.

### Use Case 3: Deletion (DEL)

DEL is used to remove records from the LDAP server. These deletions may be at the user level or directory level. If a DEL occurs at the user level, then that user will be deleted. This is intuitive and is analogous to removing someone from a subscription service that you pay for and have control over. Since you are the account owner, you can choose to delete users from the account at will. When DEL occurs at the directory level, all leafs associated with that branch are deleted by virtue of the branch being deleted. Continuing with the subscription service analogy, this is akin to you deleting your account as the subscription service owner; when you do this, all those that you allow to access your account also lose their access.

A DEL command cannot be issued by any arbitrary user. Higher authorization levels and special permissions are required to perform the DEL command. Because of this, a DEL will follow the BIND use case to identify if the actor has the appropriate permissions required to perform the DEL. Like the ADD it must verify the ACLs, but it does not need to check schema. It should check recursively that the DEL is allowed, they might to delete an organizational unit (OU) that has multiple leaves. While they would be allowed to delete the leaf, they would not be allowed to delete the OU.

#### Use:
![DEL - use case](https://github.com/bartelsjoshuac/SAPG/blob/main/images/DEL%20-%20use%20case.svg)

A company is using LDAP to manage its employees. One employee was recently terminated due to poor performance. As is standard practice in most companies, the organization wants to remove the terminated employee from its systems so that he no longer has access to company resources. The LDAP administrator must delete the terminated employee from the LDAP server.

The authorized administrator DELs the terminated employee from the LDAP server using that terminated employee's valid Distinguished Name (DN) and appropriate password. The LDAP server will then wait for additional operations and apply access control lists (ACL) based on the identity provided by the administrator. After all security checks have passed, the terminated employee will be removed from the server and will lose access to his company accounts.

#### Misuse:

![DEL - misuse case](https://github.com/bartelsjoshuac/SAPG/blob/main/images/DEL%20-%20misuse%20case.svg)

The terminated employee is vindicative and wants to harm the company by deleting contents of the LDAP server so that the company's ability to allocate internal resources to its employees is inhibited. The company relies on the LDAP server to determine which employees have access to certain resources within the company. He plans to do this by deleting the company's divisions in the LDAP server. Since these divisions are represented as organizational units (informally refered to as branches), the employees associated with them (leafs to the branch) will be deleted as well. He intends to impersonate an administrator to perform this action. When researching LDAP, the disgruntled employee found out that standard LDAP server configuration does not support many security features at all; not even password encryption. He plans to use password sniffing and brute force attack to obtain the administrator's password. After this, he will leverage the fact that standard LDAP servers offer inadequate access controls. This will allow him to delete entries that even an authorized administrator should not be able to do. Third, he will capitalize on the lack of intrusion detection mechanisms within the standard LDAP server to wreak as much havoc as he can before he is kicked from the server. Finally, he is confident that his efforts will be lasting since the standard LDAP server does not come with a backup storage option. He knows that his company is not very tech-savvy and likely has done nothing to enhance the security of its LDAP server.

#### Misuse Remedy:

![DEL - misuse case remedy](https://github.com/bartelsjoshuac/SAPG/blob/main/images/DEL%20-%20misuse%20remedy%20case.svg)

Fortunately, the company quietly hired a tech consulting agency some time ago. This consulting agency provided myriad pointers on how their client can improve their LDAP server to ensure that attackes such as this can be prevented. By implementing strong encryption algorithms based on prime number factorization, the company is better able to safeguard its employees' passwords. The benefit of this implementation is especially useful when it comes to safeguarding the passwords of users with higher level access and permission levels, such as administrators. This prevents the disgruntled employee from obtaining an administrative user's password and theoretically stops the attack in its tracks. The company implemented other security measures as fall back mechanisms in the event that a disgruntles employee is able to bypass this mechanism. For example, if the disgruntled employee is able to coerce an administrator into sharing his password, he could proceed with his attack and circumvent this security measure. Prudently, the company strongly enforced a structured role-back access control (RBAC) protocol, limiting adminstrator priviledges to only operate at the individual level rather than branch level. This prevents the employee from deleting large swathes of employees through applying the DEL to an entire organizational unit. However, this only buys the company time since a determined and persistent actor could still manually delete individual employees if left undetected. This may still cary large ramifications if the attacker has the forethought to delete the accoutns associated with senior leadership. To address this, the company implemented an intrusion detection system to percieve when such an attack is taking place. This system works wonders in limiting the scope of the damage that an attacker can carry out, no matter how capable or persistant he is. Finally, to undo any damage that occurs before the intrusion detection system kicks in, the company has implemented an automatic backup system that updates every few hours. Armed with this stored data, the company can easily undo the damage that took place by loading a stored instance of the data from prior sessions.


##### Example:
uid=user1, ou=HR,dc=company,dc=com
uid=user1, ou=HR,dc=company,dc=com

#### The ACL would not allow them to  delete dc=company or dc=com.

---
<!--- Adam Stemmler --->
### Use Case 4: An employee has been promoted to a new possition, prompting a change in office location, phone number and group memberships. (MDFY)

A MDFY action follows the BIND use case to identify the actor, and will then also apply ACLs. It is like an ADD in that it must verify the schema. Attributes have types- boolean, string, etc. They can also be multi-valued. A bad actor may try to discover the schema or influence. For example, cn is normally a single valued attribute. If it were multivalued and the bad actor could not change or MDFY the cn value, could they ADD a value so that my cn was equal to both user1 and username1? System attributes can almost never be modified, like loginAttempts. If the loginAttempts counter was exceeded, could a bad actor set it back to zero?



---
### Use Case 5: A building supervisor wants to search for the email address of all employees on 2nd floor in the Omaha HQ to notify them of a power outage. (SRCH)

Searches many times would happen with an anonymous BINDs.  Anonymous BINDs are like a everyone group.  But they still have ACLs applied.  Searches can be dangerous and cause a denial of service.  For example cn=user1* is probably not so bad, how many user1’s could there be.  A search of cn=user* might be bad.  A search of objectClass=* is the same as “give me everything”.  That is bad.  LDAP server employee maxResults, and lookThruLimits.  maxResults if the obvious one.  A lookThrulimit 
is less obvious but it means how long should I spend trying to find what  you asked for.  For example take a query of (&(cn=*)(objectClass=groupofNames))

So what does that say.  First it says give me everyone with a common name attribute, AKA, user.  But wait is says AND give me all the objects of the type of group.  Well, groups don’t have a CN, so the LDAP server is going to retrieve all the users and all the groups AND see if any of them match, to which the result is NO.  But it is going to spend a lot of time doing that, AKA, DDoS.  So a reasonable lookThruLimit prevents the LDAP server from wasting it time.  It says you don’t know what you are doing and sent a stupid query to me.

<!--- Josh Bartels --->
### Use Case 6: Identity Management (Extended Operations)
OpenLDAP supports extended operations which are operations not defined by the RFC.  This can be anything you implement for LDAP server to call such as a password policy as mentioned in Use Case #1.  More often than not extended operations are added by an associated Identity Management Suite and for this case we will examine role management such as what SairPoint provides

#### Use:
![Use-Case-6 - EXT]( https://github.com/bartelsjoshuac/SAPG/blob/main/Use-Misuse%20Case%206%20-EXT.drawio.svg)

The IDM suite is bound to the LDAP server as an administrative user and wishes to enforce policy via a reconciliation.  This will cause the LDAP server to initiate a large number of search and modify operations.

#### UseMisuse:

![Use-Misuse-Case 6 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case%201%20-Bind.drawio.svg)

The extended operation has full reign over the LDAP server and can issue poor crafted queries which return inaccurate results, leading to unauthorized modifications

#### Misuse Remedy:

The adminID should be bound by ACLs and limits.  
The extended operation must support a play mode for further evaluation before a commit.
The analysis search must be redirected by referral to a read only copy.

![Misuse-Remedy-Case 6 - Bind]( https://github.com/bartelsjoshuac/SAPG/blob/main/Use-Misuse%20Case-Final6%20-EXTdrawio.svg)

<!--- End --->



## Part 2

The [Security section](https://www.openldap.org/doc/admin26/security.html) of the [OpenLDAP Documentation](https://www.openldap.org/doc/admin26/) is rather brief.  It focuses on Network Security which can be summed up as TLS.  By default passwords in LDAP are stored in clear text to comply with [RFC4519](https://www.rfc-editor.org/rfc/rfc4519.txt) which is not desirable.  While several encryption methods are mentioned, it is suggested to use SSHA, or the salted version of the SHA scheme.

#### [Access Controls](https://www.openldap.org/doc/admin26/access-control.html) lists are the primary security feature of OpenLDAP, or any LDAP.  These control who can do what.  Read, Write, Delete, etc.

#### [Access Logging](https://www.openldap.org/doc/admin26/overlays.html#Access%20Logging) and [Audit](https://www.openldap.org/doc/admin26/overlays.html#Audit%20Logging) are two additional security features of LDAP that must be configured.

<!--- Adam Stemmler --->
## Reflection on Assignment

In our last reflection, we determined we needed to have more and better communication as a group. For this assignment, we were able to work together very well and our work was charecterized by much discussion and sharing of ideas. We had a lot of trouble understanding and identifying the expectations for this assignment, but through lots of discussion and work from the team, we were able to settle on what we determined was the best course of action for this assignment and the project as a whole.

As stated, we had a lot of trouble understanding and identifying the expectations for this assignment. We feel that the information provided on the assignment description on Canvas did not reflect our final understanding of what was expected from the professor based on feedback, meetings, and emails with him, as well as examing previous project deliverables. In addition, we feel that this project and class would greatly benefit from the inclusion of a project roadmap or overview to show how each individual assignment fits into the project and how subsequent assignments will build upon past work. This would greatly aid in understanding the expectations for assignments. As noted, we had trouble determining what our work should look like and accomplish, so we turned to past projects' GitHub repos and examined the work they did- in the end we examined nearly every past project. The past projects did help to illustrate what our diagrams should look like, however, the main issue we ran into is that the work in those repos that dealt with use/misuse cases, diagrams, and requirements for software security engineering did not match the expecations laid out on the canvas page for our assignment. Due to this, we assumed the work in theose repos was work done for a future assignment that we have not yet recieved. However, that took some time to establish and threw us off our goal of understanding what we should be accomplishing. If there had been some sort of roadmap or project overview, we would have been better able to leverage the findings from previous projects to assist in our work. All this is to say that we hope the work we have done adequetly meets and exceeds the expectations for this assignment, but we would feel more confident if we were not recieving mixed messages about what it is we should be doing for this assignment
