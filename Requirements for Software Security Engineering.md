# Requirements for Software Security Engineering

## Part 1

### Use Case 1: Authenticat and Authorization (BIND)
The BIND operation identifies the actor to the server.  LDAP will typically allow anonymous BIND operations which may or may not be disabled, depending on business requirements.  ACL’s are applied to the actor that bound to the system when the BIND is successful.  Binding anonymously would typically be configured with read only access, as there would be no accountability of events.  Where binding as cn=Directory Manager often applies no ACL’s at all.  Is this case we will assume a normal BIND from a standard system user, which would be authenticating to a website via Single Sign On (SSO) software via a website, logging into a Linux server, etc.

#### Use:

![Use-Case-1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use%20Case%201%20-%20Bind.drawio.svg)

The actor wishes to identify themselve to the server and will do so by providing a valid Distinguished Name (DN) and simple password to establish their identity to the LDAP server and perform a BIND.  The LDAP server will them wait for additional operations, and apply ACL’s based on that identity.

#### UseMisuse:

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case%201%20-Bind.drawio.svg)

The bad actor is attempting to determine via a brute force attack, the password of a good actor’s DN they obtained from a previous anonymous BIND and SRCH request by performing a password spraying attack.  By obtaining the password they can then BIND as this user and perform actions on behalf of this user, who likely has additional privileges than they would as an anonymous user.

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case-Final1%20-Bind.drawio.svg)

.  The LDAP server will prevent this attack from being successful by applying and tracking loginAttempts and allowing a max of X attempts, before disabling the account.  It will not assist the attacker by differentiating to the bad actor if it was the username (DN) or password that was incorrect by informing them of the lock out.  The lockout shall remain in place for Y number of minutes, or until cleared by a Directory Manager preventing the attacker from succeeding.  Audit logs will also indicate a high number of failed attempts for this user, and a lockout, an abnormal activity for a standard user.


#### UseMisuse:

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case%201%20-Bind.drawio.svg)

A bad actor with no valid credenmtials is attempting to determine the password of a good actor they obtained from a previous anonymous BIND and SRCH request by performing a brute force password attack.  The LDAP server will prevent this attack from being successful by tracking loginAttempts and allowing a max of X, before disabling the account.  It will not assist the bad actor by differentiating to the bad actor if it was the username or password that was incorrect by informing them of the lock out.  The lockout shall remain in place for Y number of minutes, or until cleared by a Directory Manager.

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case-Final1%20-Bind.drawio.svg)

---
### Use Case 2: An administrative wants to entry a new employee record with basic white page information and set a temporary password that the user must change at login. (ADD)

An ADD will follow the BIND use case to identify the actor to evaluate the ACL’s to determine if the user has the authority to add this type of record.  If they do, it will then check that the ADD request complies with the schema, e.g. required attributes, optional attributes, no system attributes.So  a bad actor could try and add something that already exists (modify), something they are not allow to add, something that violates the schema definition, 

### Use:

The actor ADD a new user with a valid Distiguished Name (DN) and a valid password to the LDAP server.

---
### Use Case 3: An administrator wants to delete an employee entry that is no longer with the company. (DEL)

An DEL will follow the BIND use case to identify the actor.  Like the ADD it must verify the ACLs, but it does not need to check schema.  It should check recursively that the DEL is allowed, they might to delete an organizational unit (OU) that has multiple leaves.  While they would be allowed to delete the leaf, they would not be allowed to delete the OU.

#### Use:

![Use-Case - DEL](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use%20Case%20-%20DEL.svg)

The actor DELs with a valid Distinguished Name (DN) and appropriate password to the LDAP server. This DN and password belong to the user who is going to be deleted. The LDAP server will them wait for additional operations, and apply access control lists (ACL) based on that identity.

#### Misuse:

![Misuse-Case - DEL](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Misuse%20Case%20-%20DEL.svg)

A malicious actor is attempting to impersonate the administrator. This will allow him to perform several unauthorized actions, such as gathering user information and performing unauthorized deletes. This unauthorized access can compromise the integrity of the system and lead to lack of user trust at best and identity theft and impersonation at worse.

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

