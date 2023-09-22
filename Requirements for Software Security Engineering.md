# Requirements for Software Security Engineering

## Part 1

### Use Case 1: Authenticate and Authorization (BIND)
The BIND operation identifies the actor to the server.  LDAP will typically allow anonymous BIND operations which may or may not be disabled, depending on business requirements.  ACL’s are applied to the actor that bound to the system when the BIND is successful.  Binding anonymously would typically be configured with read only access, as there would be no accountability of changers.  Where binding as cn=admin often applies no ACL’s at all.  Is this case we will assume a normal BIND from a standard system user with a password, which would be authenticating to a website via Single Sign On (SSO) software via a website, logging into a Linux server, etc.

#### Use:

![Use-Case-1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use%20Case%201%20-%20Bind.drawio.svg)

The actor wishes to identify themselves to the server and will do so by providing a valid Distinguished Name (DN) and simple password to establish their identity to the LDAP server and perform a BIND.  The LDAP server will them wait for additional operations which will be explored in later uses cases that build on this.

#### UseMisuse:

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case%201%20-Bind.drawio.svg)

The bad actor could sniff the password on the wire on the know port (389).  

Once authenticated, with anonymous access they could also read another users password attribute.    

Given that weak passwords on in use, a brute force of password spraying attack could discover a password.  

#### Misuse Remedy:

OpenLDAP supports SSL/TLS based on the OpenSSL package that is installed on the host machine.  A strong TLS encryption method should be selected to deter snigging passwords during BIND requests.    

OpenLDAP can store passwords in clear-txt, encrypted strings or hashes.  It is suggested that DIGEST-MD5 is used, however SHA, CRYPT, MD5, SMD5, and SASL are offered.  A strong encryption algorithm should be chosen.  Pass-thru authentication is also an option so that passwords are not stored in LDAP at all is is how a MFA solution would be implemented for the later attack vector (brute force or password spraying).    

OpenLDAP has a [dynamically loaded password policy available.]( https://tobru.ch/openldap-password-policy-overlay/).  A password policy enforcing a minimum length, character set, and complexity should be configured to limit successful brute force or password spraying attacks.  Note that this module also includes the   login attempts and lockout.  

The documentation on the configuration of any of these options is not very comprehensive, nor is it part of the setup script which may cause administrators to overlook them, possibly assuming they are implemented by default as they would be in commercial implementations.  

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case-Final1%20-Bind.drawio.svg)






---
### Use Case 2: An administrative wants to entry a new employee record with basic white page information and set a temporary password that the user must change at login. (ADD)

An ADD will follow the BIND use case to identify the actor to evaluate the ACL’s to determine if the user has the authority to add this type of record.  If they do, it will then check that the ADD request complies with the schema, e.g. required attributes, optional attributes, no system attributes.So  a bad actor could try and add something that already exists (modify), something they are not allow to add, something that violates the schema definition, 

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

### Use Case 3: An administrator wants to delete an employee entry that is no longer with the company. (DEL)

DEL is used to remove records from the LDAP server. A DEL will follow the BIND use case to identify the actor.  Like the ADD it must verify the ACLs, but it does not need to check schema.  It should check recursively that the DEL is allowed, they might to delete an organizational unit (OU) that has multiple leaves.  While they would be allowed to delete the leaf, they would not be allowed to delete the OU.

#### Use:

![Use-Case - DEL](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use%20Case%20-%20DEL.svg)

The administrator DELs a user with a valid Distinguished Name (DN) and appropriate password from the LDAP server. This DN and password belong to an employee who is no longer with the company. The LDAP server will them wait for additional operations, and apply access control lists (ACL) based on that identity.

#### Misuse:

![Misuse-Case - DEL](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Misuse%20Case%20-%20DEL.svg)

A malicious actor is attempting to impersonate the administrator. This will allow him to perform several unauthorized actions, such as gathering user information, performing unauthorized deletes, and even deleting entire organizational units (OU). This unauthorized access can compromise the integrity of the system and lead to lack of user trust at best and identity theft and impersonation at worse.

#### Misuse Remedy:

![Misuse-Case-Remedy - DEL](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Misuse%20Case%20Remedy%20-%20DEL.svg)

Such an attack by a malicious actor may be prevented through various security features integral to LDAP protocol. LDAP requires authentication to ensure that users are only acting within the scope of their defined permissions. This is ensured via Kerberos for secure authentication using tickets. Certificates may also be used to add an extra layer of security. Additionally, LDAP uses authorization mechanisms to control what authenticated users can do within the directory. This is handled via the access control lists (ACL) or directory server access control instructions (ACI). These mechanisms allow administrators to define who can access what information and impose restrictions on administrator roles. Third, auditing and logging may be used to track user activities, further reducing the risk of administrator impersonation. Fourth and finally, LDAP supports IP whitelisting, allowing administrators to configure the LDAP server to only accept connections from trusted IP addresses. This limits the server accessibility to only those with connections trusted by the organization.


##### Example:
uid=user1, ou=HR,dc=company,dc=com
uid=user1, ou=HR,dc=company,dc=com

#### The ACL would not allow them to  delete dc=company or dc=com.

---
### Use Case 4: An employee has been promoted to a new possition, prompting a change in office location, phone number and group memberships. (MDFY)

An MDFY will follow the BIND use case to identify the actor.  It will also apply ACL’s.  It is like an ADD in that it must verify the schema.   Attributes have types; boolean, string, etc.  They can also be multi-valued. A bad actor may try to discover the schema or influence.  For example cn is normally a single valued attribute.  If it were multivalued and the bad actor could not change the MDFY the cn value, could they ADD a value so that my cn was equal to both user1 and username1?  System attributes can almost never be modified, like loginAttempts.  If the loginAttempts counter was exceeded, could a bad actor set it back to zero?

---
### Use Case 5: A building supervisor wants to search for the email address of all employees on 2nd floor in the Omaha HQ to notify them of a power outage. (SRCH)

Searches many times would happen with an anonymous BINDs.  Anonymous BINDs are like a everyone group.  But they still have ACLs applied.  Searches can be dangerous and cause a denial of service.  For example cn=user1* is probably not so bad, how many user1’s could there be.  A search of cn=user* might be bad.  A search of objectClass=* is the same as “give me everything”.  That is bad.  LDAP server employee maxResults, and lookThruLimits.  maxResults if the obvious one.  A lookThrulimit 
is less obvious but it means how long should I spend trying to find what  you asked for.  For example take a query of (&(cn=*)(objectClass=groupofNames))

So what does that say.  First it says give me everyone with a common name attribute, AKA, user.  But wait is says AND give me all the objects of the type of group.  Well, groups don’t have a CN, so the LDAP server is going to retrieve all the users and all the groups AND see if any of them match, to which the result is NO.  But it is going to spend a lot of time doing that, AKA, DDoS.  So a reasonable lookThruLimit prevents the LDAP server from wasting it time.  It says you don’t know what you are doing and sent a stupid query to me.

### Use Case 6: Extended Operations - Not sure on this one yet.

If we feel we need 6, I can do something for extended operations.  They are a family of uncommon operations and differ based on vendor.  Many of them can be very poorly implanted and I can find one for openLDAP that probably sux.


## Part 2

The [Security section](https://www.openldap.org/doc/admin26/security.html) of the [OpenLDAP Documentation](https://www.openldap.org/doc/admin26/) is rather brief.  It focuses on Network Security which can be summed up as TLS.  By default passwords in LDAP are stored in clear text to comply with [RFC4519](https://www.rfc-editor.org/rfc/rfc4519.txt) which is not desirable.  While several encryption methods are mentioned, it is suggested to use SSHA, or the salted version of the SHA scheme.

#### [Access Controls](https://www.openldap.org/doc/admin26/access-control.html) lists are the primary security feature of OpenLDAP, or any LDAP.  These control who can do what.  Read, Write, Delete, etc.

#### [Access Logging](https://www.openldap.org/doc/admin26/overlays.html#Access%20Logging) and [Audit](https://www.openldap.org/doc/admin26/overlays.html#Audit%20Logging) are two additional security features of LDAP that must be configured.

