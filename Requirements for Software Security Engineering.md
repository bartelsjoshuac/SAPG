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

![ADD - Use Case](https://github.com/bartelsjoshuac/SAPG/blob/main/images/ADD%20-%20Use%20Case.svg)

The actor ADD a new user with a valid Distiguished Name (DN) and a valid password to the LDAP server with following steps:
The actore logs in to the LDAP server with appropriate privileges.
They navigate to the appropriate organizational unit (OU) where user accounts are managed.
They create a new entry for new employee, including attributes such as cn (common name), uid (user ID), givenName, sn (surname), mail (email address), userPassword, etc.
They set the password for new employee's account and configure any necessary group memberships or access permissions.
The new employee is now able to log in to company systems using his LDAP credentials.

#### Misuse Cases

![ADD - Misuse Case](https://github.com/bartelsjoshuac/SAPG/blob/main/images/ADD%20-%20Misuse%20Case.svg)

**Scenario 1:**
The Malicious Insider has gained unauthorized access to the LDAP administration interface.

* The Malicious Insider logs in using stolen credentials or exploits a vulnerability.
* The Malicious Insider selects the "Add Person" option.
* The Malicious Insider fills out the required fields with false or malicious information.
* The Malicious Insider submits the form.

**Scenario 2:** 
The External Attacker has identified a vulnerability in the LDAP server's input validation.

* The External Attacker exploits a vulnerability to bypass input validation.
* The External Attacker injects malicious code or special characters into the input fields, potentially compromising the LDAP server.
* The External Attacker submits the form with the injected data.

**Scenario 3:**
The Malicious Attacker aims to disrupt normal LDAP server operations.

* The Malicious Attacker floods the LDAP server with a high volume of requests to add persons.
* The server's resources become overloaded due to the excessive processing demands.
* The LDAP server becomes unresponsive, impacting legitimate users' ability to access or modify directory information.


#### Misuse Remedy:


![Add Missuse remedy](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Add%20-%20Misuse%20Remedy.svg)

**Unauthorized Access and Data Manipulation:**
Above attacks can be prevented by following remedies:
Review and update access control lists (ACLs) regularly to ensure that only authorized users have the necessary permissions to perform LDAP operations. Configure proper permissions based on roles and responsibilities. Implement role-based access control (RBAC) to grant specific privileges to different user groups. Use LDAP groups and memberships to manage access. Enforce strong authentication mechanisms. Implement multi-factor authentication (MFA) to add an extra layer of security.

**Injection Attack:**
Implement robust input validation and sanitation techniques. Use parameterized queries to prevent SQL injection attacks. Sanitize input data to remove potentially harmful characters. Keep the LDAP server software and related libraries up-to-date. Regularly check for security updates and patches provided by the LDAP server's vendor. Apply patches promptly to address known vulnerabilities.

**Denial-of-Service (DoS) Attack:**
Implement traffic filtering and rate limiting to protect against DoS attacks. Use firewalls and network appliances to filter traffic and block suspicious requests. Configure rate-limiting rules to restrict the number of requests from a single source. Implement DoS detection and mitigation strategies. Use intrusion detection systems (IDS) or intrusion prevention systems (IPS) to detect and block suspicious traffic patterns.



### Use Case 3: Deletion (DEL)

DEL is used to remove records from the LDAP server. These deletions may be at the user level or directory level. If a DEL occurs at the user level, then that user will be deleted. This is intuitive and is analogous to removing someone from a subscription service that you pay for and have control over. Since you are the account owner, you can choose to delete users from the account at will. When DEL occurs at the directory level, all leaves associated with that branch are deleted by virtue of the branch being deleted. Continuing with the subscription service analogy, this is akin to you deleting your account as the subscription service owner; when you do this, all those that you allow to access your account also lose their access.

A DEL command cannot be issued by any arbitrary user. Higher authorization levels and special permissions are required to perform the DEL command. Because of this, a DEL will follow the BIND use case to identify if the actor has the appropriate permissions required to perform the DEL. Like the ADD it must verify the ACLs, but it does not need to check schema. It should check recursively that the DEL is allowed, they might to delete an organizational unit (OU) that has multiple leaves. While they would be allowed to delete the leaf, they would not be allowed to delete the OU.

#### Use:
![DEL - use case](https://github.com/bartelsjoshuac/SAPG/blob/main/images/DEL%20-%20use%20case.svg)

A company is using LDAP to manage its employees. One employee was recently terminated due to poor performance. As is standard practice in most companies, the organization wants to remove the terminated employee from its systems so that he no longer has access to company resources. The LDAP administrator must delete the terminated employee from the LDAP server.

The authorized administrator DELs the terminated employee from the LDAP server using that terminated employee's valid Distinguished Name (DN) and appropriate password. The LDAP server will then wait for additional operations and apply access control lists (ACL) based on the identity provided by the administrator. After all security checks have passed, the terminated employee will be removed from the server and will lose access to his company accounts.

#### Misuse:

![DEL - misuse case](https://github.com/bartelsjoshuac/SAPG/blob/main/images/DEL%20-%20misuse%20case.svg)

The terminated employee is vindicative and wants to harm the company by deleting contents of the LDAP server so that the company's ability to allocate internal resources to its employees is inhibited. The company relies on the LDAP server to determine which employees have access to certain resources within the company. He plans to do this by deleting the company's divisions in the LDAP server. Since these divisions are represented as organizational units (informally referred to as branches), the employees associated with them (leaves to the branch) will be deleted as well. He intends to impersonate an administrator to perform this action. When researching LDAP, the disgruntled employee found out that standard LDAP server configuration does not support many security features at all, not even password encryption. He plans to use password sniffing and brute force attack to obtain the administrator's password. After this, he will leverage the fact that standard LDAP servers offer inadequate access controls. This will allow him to delete entries that even an authorized administrator should not be able to do. Third, he will capitalize on the lack of intrusion detection mechanisms within the standard LDAP server to wreak as much havoc as he can before he is kicked from the server. Finally, he is confident that his efforts will be lasting since the standard LDAP server does not come with a backup storage option. He knows that his company is not very tech-savvy and likely has done nothing to enhance the security of its LDAP server.

#### Misuse Remedy:

![DEL - misuse case remedy](https://github.com/bartelsjoshuac/SAPG/blob/main/images/DEL%20-%20misuse%20remedy%20case.svg)

Fortunately, the company quietly hired a tech consulting agency some time ago. This consulting agency provided myriad pointers on how their client can improve their LDAP server to ensure that attackes such as this can be prevented. By implementing strong encryption algorithms based on prime number factorization, the company is better able to safeguard its employees' passwords. The benefit of this implementation is especially useful when it comes to safeguarding the passwords of users with higher level access and permission levels, such as administrators. This prevents the disgruntled employee from obtaining an administrative user's password and theoretically stops the attack in its tracks. The company implemented other security measures as fall-back mechanisms if a disgruntles employee is able to bypass this mechanism. For example, if the disgruntled employee can coerce an administrator into sharing his password, he could proceed with his attack and circumvent this security measure. Prudently, the company strongly enforced a structured role-back access control (RBAC) protocol, limiting adminstrator priviledges to only operate at the individual level rather than branch level. This prevents the employee from deleting large swathes of employees through applying the DEL to an entire organizational unit. However, this only buys the company time since a determined and persistent actor could still manually delete individual employees if left undetected. This may still cary large ramifications if the attacker has the forethought to delete the accoutns associated with senior leadership. To address this, the company implemented an intrusion detection system to percieve when such an attack is taking place. This system works wonders in limiting the scope of the damage that an attacker can carry out, no matter how capable or persistant he is. Finally, to undo any damage that occurs before the intrusion detection system kicks in, the company has implemented an automatic backup system that updates every few hours. Armed with this stored data, the company can easily undo the damage that took place by loading a stored instance of the data from prior sessions.

##### Example:
uid=user1, ou=HR,dc=company,dc=com
uid=user1, ou=HR,dc=company,dc=com

#### The ACL would not allow them to delete dc=company or dc=com.

---
<!--- Adam Stemmler --->
### Use Case 4: An employee has been promoted to a new possition, prompting a change in office location, phone number and group memberships. (MDFY)

MDFY actions can only take place by a user once the BIND action has taken palce and the use has been identified. LDAP clients can request modifications to make changes to data. These requests specify the distinguished name (DN) of the entry and a lsit of modifications. There are four modifications types: 1. add, 2. delete, 3. replace, 4. increment. Add can add a new attribute or add a new value to am existing attribute. Delete will delete values or whole attribute. Replace is much the same, but an alter sets of values within attributes. Increment can increase or decrease an integer value (like times loginAttempts) by a set amount. An important aspect of all modify requests is that the inforamtion they contain for modification must match the established schema on the LDAP server, as they will be rejected if they do not match. Once the server recieves the request and analyzes it for matching, it sets about changing the entry. 

#### Use:
An employee has recently been promoted to a new position, prompting a change in office location, phone number, and group memberships on the LDAP server. The employee can inteface with the company's LDAP server through an online web-based portal through which they can update their personal information. The employee sends modify requests to the LDAP server with their new information so the system is up to date with their newest contact information etc.

#### Misuse:
A disgruntled former employee, with exceptional hacking and network knowledge, who had been let go due to misconduct, wants to watch his former company suffer. To this end, this employee wants to disrupt company functions by influencing the data stored on the LDAP server. He is still in the LDAP server as an employee, as he has not yet been deleted, and he knows his login information, but he does not have access to the online portal to make changes. However, he can communicate directly with the server.

![MDFY_Full_Diagram](https://github.com/bartelsjoshuac/SAPG/blob/main/images/MDFY_Full_Diagram.svg)


#### Assessment:
As is the case with nearly all interactions with LDAP servers, clients who wish to issue requests to the server have to be authenticated to some degree and their permissions must be checked, via ACLs, to ensure the client has the authority to issue certain requests and act upon and request certain data. In this example, the disgruntled former employee tries to make unauthorized changes to data on the LDAP server. Since he still has access to the server and is able to communicate with it, he attempts to increment important server attributes that only the server itself should be able to change. He is stopped by ACL checking by the server and good defualt server configurations, which do not allow users to change server attributes, so he is unable to make those modifications. To make changes to data entries, the correct schema must be followed, or the request will be discarded. The attacker does not know the schema, and tries to determine it by sniffing the network for legitimate users making changes. However, due to data encryption, he is unable to do so. Failing this, he decides to simply issue change request, but as they do not follow the correct schema, they are discarded. The attacker could issue requests to change data with an attribute specified, but the values left empty, which by default would make the entire attribute and its fields empty. But with correct server configuration, no data attributes can be reset. Thus, the attacker is stopped.

---
### Use Case 5: A building supervisor wants to search for the email address of all employees on 2nd floor in the Omaha HQ to notify them of a power outage. (SRCH)

#### Use:
In this use case, a building supervisor located at the Omaha HQ needs to efficiently notify all employees on the 2nd floor about an ongoing power outage. To accomplish this task, the supervisor leverages LDAP. The process begins with the supervisor logging into an LDAP client application, which grants access to the organization's LDAP directory. The supervisor then initiates a search query, specifying the criteria to find all employees located on the 2nd floor within the Omaha HQ. The LDAP server processes this query and returns a list of employee entries that match the criteria. Subsequently, the supervisor extracts the email addresses from the retrieved LDAP entries. If no employees are found on the 2nd floor, the system notifies the supervisor accordingly. With the obtained email addresses, the building supervisor can efficiently notify the affected employees about the power outage, ensuring timely communication and necessary actions are taken.

![SRCH - use case](https://github.com/bartelsjoshuac/SAPG/blob/main/images/SRCH%20-%20use%20case.svg)

#### Misuse 1:
A malicious user attempts to misuse the system that the building supervisor uses to search for employee email addresses on the 2nd floor of the Omaha HQ. Instead of legitimate purposes like notifying employees of a power outage, the malicious user has harmful intentions, potentially involving spamming or phishing. Below are the steps for this misuse. 

*	**Unauthorized Access:** The malicious user gains unauthorized access to the LDAP client application, possibly by exploiting a vulnerability in the system, using stolen credentials, or other illicit means.
*	**Bypassing Authentication:** The malicious user circumvents the authentication and authorization mechanisms, gaining access to the LDAP server.
*	**Performing Unauthorized Search:** Once inside the LDAP client application, the malicious user initiates a search query with malicious intent, trying to retrieve email addresses for employees on the 2nd floor.
*	**Data Harvesting:** The LDAP server processes the unauthorized search query, returning a list of employee entries. The malicious user extracts the email addresses of employees on the 2nd floor without any legitimate reason.
*	**Misuse of Email Addresses:** With the obtained email addresses, the malicious user could engage in harmful activities such as sending spam, phishing emails, or other unauthorized communications, potentially causing disruption and harm to the organization and its employees.

The consequences of this misuse include but are not limited to unauthorized access to sensitive employee data, risk of data breaches and privacy violations, potential harm to employees through spam, phishing, or other malicious activities, and reputation damage to the organization.

![SRCH-Misuse Case 1](https://github.com/bartelsjoshuac/SAPG/blob/main/images/SRCH-Misuse%20Case%201.svg)

#### Remedy:

*	Strengthen access controls and authentication mechanisms to prevent unauthorized access to the LDAP client application.
*	Implement robust authorization policies to ensure that only authorized users can perform searches and access sensitive employee data.
*	Regularly monitor and audit LDAP access to detect and respond to suspicious activities.
*	Educate employees about security best practices to avoid falling victim to phishing or other malicious activities.

![SRCH-Misuse Case 1 Remedy](https://github.com/bartelsjoshuac/SAPG/blob/main/images/SRCH-Misuse%20Case%201%20Remedy.svg)

#### Misuse 2:
A disgruntled employee with insider knowledge of the organization's systems and procedures attempts to misuse the process of retrieving email addresses for employees on the 2nd floor. Instead of using this information for legitimate purposes, the employee has malicious intentions, potentially involving data theft or disclosure. Below are the steps for this misuse.

*	**Authorized Access:** The disgruntled employee already has authorized access to the LDAP client application as part of their job role.
*	**Legitimate Search:** Initially, the employee initiates a legitimate search query for email addresses to notify employees of the power outage.
*	**Unauthorized Data Copy:** After obtaining the email addresses, the employee decides to misuse their access by copying and saving the email addresses for unauthorized use later.
*	**Unauthorized Disclosure:** The disgruntled employee may choose to disclose this information to unauthorized individuals, sell it on the black market, or use it for personal gain, such as in competing organizations or for phishing attempts.
*	**Data Breach:** This misuse can lead to a data breach and compromise the privacy and security of employees on the 2nd floor. It may also have legal and financial repercussions for the organization.

![SRCH-Misuse Case 2](https://github.com/bartelsjoshuac/SAPG/blob/main/images/SRCH-Misuse%20Case%202.svg)
#### Remedy:

To mitigate the risk of this misuse case, the organization should:

*	Implement strict data access controls and monitor data extraction activities.
*	Conduct regular security awareness training to educate employees about the importance of data security and the consequences of misuse.
*	Monitor and log access to sensitive data, especially if multiple extraction attempts occur.
*	Implement a clear data handling policy that restricts the use and storage of sensitive information for unauthorized purposes.

This misuse case underscores the significance of not only external threats but also the importance of monitoring and managing insider threats within an organization.

![SRCH-Misuse Case 2 Remedy](https://github.com/bartelsjoshuac/SAPG/blob/main/images/SRCH-Misuse%20Case%202%20Remedy.svg)

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

![Use-Misuse-Case 6 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/Use-Misuse%20Case%206%20-EXT.drawio.svg)

The extended operation has full reign over the LDAP server and can issue poor crafted queries which return inaccurate results, leading to unauthorized modifications

#### Misuse Remedy:

The adminID should be bound by ACLs and limits.  
The extended operation must support a play mode for further evaluation before a commit.
The analysis search must be redirected by referral to a read only copy.

![Misuse-Remedy-Case 6 - Bind]( https://github.com/bartelsjoshuac/SAPG/blob/main/Use-Misuse%20Case-Final6%20-EXTdrawio.svg)

<!--- End --->

<!--- Monirul --->
### Use Case 7: A security Analyst wants to generate a report containing a list of employees who have accessed sensitive date in last couple of months (SRCH).
In this use case, a security analyst want to generate a routine report for all the users of system who have access the sensitive data. Routine check on this log is critical to identify any suspecious user and activity.
![use case 7](https://github.com/bartelsjoshuac/SAPG/blob/main/images/use_case_7.drawio.svg)
<!--- Misuse cases--->
Misuse Case 1: A malicious actor gains access to the system and prepares a report of all the employees who have accessed the sensitive information in the past couple of months. Now he is targeting these employees with phishing attacks.

Misuse Case 2: A supervisor generates a report of all employees who have accessed sensitive information in last couple of months. Then he tracks down who misuse their privilege to access information that is beyond their access limit and track their activity.

Misuse Case 3: A disgruntled employee generates a report of all employees who have accessed the sensitive information last couple of months even though they are not authorized to do so then leaks this information to the media.
![misuse case 7](https://github.com/bartelsjoshuac/SAPG/blob/main/images/use_case_7_attacks.drawio.svg)
<!--- Remedy--->
#### Misuse Remedy:
Importance of implementing security controls to protect sensitive data and prevention of unauthorized access  is highlighted in these misuse cases. Apart from education emplyess about the risks of cyber attacks some security remedies are listed below which can mitigate these risks:

* Misuse Case 1:

  - Implement Multi-factor authentication (MFA)
  - Monitor the system for suspicious activity such as unusual access patterns or failed login attempts
  - Educate employees about the importances of protecting passwords and risks of phishing attacks

* Misuse Case 2:

  - Implement role-based access control (RBAC)
  - Audit and log all access to sensitive data. So even supervisors will require authorization to access sensitive data
* Misuse Case 3:

  - Implement data encryption for the protection of sensitive data
  - Audit and log all access to sensitive data. Therefore disgruntled employees cannot go undetected if they access any sensitive data.

![use case 7 remedy](https://github.com/bartelsjoshuac/SAPG/blob/main/images/use_case_7_remedy.drawio.svg)


<!--- End --->

## Part 2

The [Security section](https://www.openldap.org/doc/admin26/security.html) of the [OpenLDAP Documentation](https://www.openldap.org/doc/admin26/) is rather brief.  It focuses on Network Security which can be summed up as TLS.  By default passwords in LDAP are stored in clear text to comply with [RFC4519](https://www.rfc-editor.org/rfc/rfc4519.txt) which is not desirable.  While several encryption methods are mentioned, it is suggested to use SSHA, or the salted version of the SHA scheme.

#### [Access Controls](https://www.openldap.org/doc/admin26/access-control.html) lists are the primary security feature of OpenLDAP, or any LDAP.  These control who can do what.  Read, Write, Delete, etc.

#### [Access Logging](https://www.openldap.org/doc/admin26/overlays.html#Access%20Logging) and [Audit](https://www.openldap.org/doc/admin26/overlays.html#Audit%20Logging) are two additional security features of LDAP that must be configured.

<!--- Adam Stemmler --->
## Reflection on Assignment

In our last reflection, we determined we needed to have more and better communication as a group. For this assignment, we were able to work together very well and our work was charecterized by much discussion and sharing of ideas. We had a lot of trouble understanding and identifying the expectations for this assignment, but through lots of discussion and work from the team, we were able to settle on what we determined was the best course of action for this assignment and the project as a whole.

As stated, we had a lot of trouble understanding and identifying the expectations for this assignment. We feel that the information provided on the assignment description on Canvas did not reflect our final understanding of what was expected from the professor based on feedback, meetings, and emails with him, as well as examing previous project deliverables. In addition, we feel that this project and class would greatly benefit from the inclusion of a project roadmap or overview to show how each individual assignment fits into the project and how subsequent assignments will build upon past work. This would greatly aid in understanding the expectations for assignments. As noted, we had trouble determining what our work should look like and accomplish, so we turned to past projects' GitHub repos and examined the work they did- in the end we examined nearly every past project. The past projects did help to illustrate what our diagrams should look like, however, the main issue we ran into is that the work in those repos that dealt with use/misuse cases, diagrams, and requirements for software security engineering did not match the expecations laid out on the canvas page for our assignment. Due to this, we assumed the work in theose repos was work done for a future assignment that we have not yet recieved. However, that took some time to establish and threw us off our goal of understanding what we should be accomplishing. If there had been some sort of roadmap or project overview, we would have been better able to leverage the findings from previous projects to assist in our work. All this is to say that we hope the work we have done adequetly meets and exceeds the expectations for this assignment, but we would feel more confident if we were not recieving mixed messages about what it is we should be doing for this assignment
