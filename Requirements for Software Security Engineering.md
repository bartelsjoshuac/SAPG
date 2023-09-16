# Requirements for Software Security Engineering

## Part 1

### Use Case 1: BIND (authenticate and authorize)
The BIND operation identifies the actor to the server.  LDAP will typical allow anonymous BIND operations which may or may not be disabled, depending on business requirements.  ACL’s are applied to the actor that bound to the system when the BIND is successful.  Binding anonymously would typically be configured with read only access, as there would be no accountability of events.  Where binding as cn=Directory Manager often applies no ACL’s at all.  Is this case we will assume a normal BIND from a standard system user, which would be authenticating to a website via Single Sign On (SSO) software via a website, logging into a Linux server, etc.

#### Use:

![Use-Case-1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use%20Case%201%20-%20Bind.drawio.svg)

The actor BINDS with a valid Distinguished Name (DN) and simple password to establish their identity to the LDAP server.  The LDAP server will them wait for additional operations, and apply ACL’s based on that identity.

#### UseMisuse:

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case%201%20-Bind.drawio.svg)

The bad actor is attempting to determine the password of a good actor’s DN they obtained from a previous anonymous BIND and SRCH request by performing a password spraying attack.  By obtaining the password they can then BIND as this user and perform actions on behalf of this user, who likely has additional privileges than they would as an anonymous user.

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case-Final1%20-Bind.drawio.svg)

.  The LDAP server will prevent this attack from being successful by applying and tracking loginAttempts and allowing a max of X attempts, before disabling the account.  It will not assist the attacker by differentiating to the bad actor if it was the username (DN) or password that was incorrect by informing them of the lock out.  The lockout shall remain in place for Y number of minutes, or until cleared by a Directory Manager preventing the attacker from succeeding.  Audit logs will also indicate a high number of failed attempts for this user, and a lockout, an abnormal activity for a standard user.


#### UseMisuse:

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case%201%20-Bind.drawio.svg)

The bad actor is attempting to determine the password of a good actor’s they obtained from a previous anonymous BIND and SRCH request by performing a password spraying attack.  The LDAP server will prevent this attack from being successful by applying tracking loginAttempts and allowing a max of X, before disabling the account.  It will not assist the attacker by differentiating to the bad actor if it was the username or password that was incorrect by informing them of the lock out.  The lockout shall remain in place for Y number of minutes, or until cleared by a Directory Manager.

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/images/Use-Misuse%20Case-Final1%20-Bind.drawio.svg)

---
### Use Case 2: ADD

An ADD will follow the BIND use case to identify the actor to evaluate the ACL’s to determine if the user has the authority to add this type of record.  If they do, it will then check that the ADD request complients with the schema, e.g. required attributes, optional attributes, no system attributes.So  a bad actor could try and add something that already exists (modify), something they are not allow to add, something that violates the schema definition, 

---
### Use Case 2: DEL

An DEL will follow the BIND use case to identify the actor.  Like the ADD it must verify the ACLs, but it does not need to check schema.  It should check recursively that the DEL is allowed, they might to delete an organizationalUnit (ou) that has multiple leaves.  While they probably would be allowed to delete the leaf, they probably would not be allowed to delete the ou.

##### Example:
uid=user1, ou=HR,dc=company,dc=com
uid=user1, ou=HR,dc=company,dc=com

#### They ACL would not allow them to  delete dc=company or dc=com.

---
### Use Case 4: MDFY

An MDFY will follow the BIND use case to identify the actor.  It will also apply ACL’s.  It is like an ADD in that it must verify the schema.   Attributes have types; boolean, string, etc.  They can also be multi-valued. A bad actor may try to discover the schema or influence.  For example cn is normally a single valued attribute.  If it were multivalued and the bad actor could not change the MDFY the cn value, could they ADD a value so that my cn was equal to both user1 and username1?  System attributes can almost never be modified, like loginAttempts.  If the loginAttempts counter was exceeded, could a bad actor set it back to zero?

---
### Use Case 5: SRCH

Searches many times would happen with an anonymous BINDs.  Anonymous BINDs are like a everyone group.  But they still have ACLs applied.  Searches can be dangerous and cause a denial of service.  For example cn=user1* is probably not so bad, how many user1’s could there be.  A search of cn=user* might be bad.  A search of objectClass=* is the same as “give me everything”.  That is bad.  LDAP server employee maxResults, and lookThruLimits.  maxResults if the obvious one.  A lookThrulimit 
is less obvious but it means how long should I spend trying to find what  you asked for.  For example take a query of (&(cn=*)(objectClass=groupofNames))

So what does that say.  First it says give me everyone with a common name attribute, AKA, user.  But wait is says AND give me all the objects of the type of group.  Well, groups don’t have a CN, so the LDAP server is going to retrieve all the users and all the groups AND see if any of them match, to which the result is NO.  But it is going to spend a lot of time doing that, AKA, DDoS.  So a reasonable lookThruLimit prevents the LDAP server from wasting it time.  It says you don’t know what you are doing and sent a stupid query to me.

### Use Case 6: Extended Operations

If we feel we need 6, I can do something for extended operations.  They are a family of uncommon operations and differ based on vendor.  Many of them can be very poorly implanted and I can find one for openLDAP that probably sux.


## Part 2

The [Security section](https://www.openldap.org/doc/admin26/security.html) of the [OpenLDAP Documentation](https://www.openldap.org/doc/admin26/) is rather brief.  It focuses on Network Security which can be summed up as TLS.  By default passwords in LDAP are stored in clear text to comply with [RFC4519](https://www.rfc-editor.org/rfc/rfc4519.txt) which is not desirable.  While several encryption methods are mentioned, it is suggested to use SSHA, or the salted version of the SHA scheme.

#### [Access Controls](https://www.openldap.org/doc/admin26/access-control.html) lists are the primary security feature of OpenLDAP, or any LDAP.  These control who can do what.  Read, Write, Delete, etc.

#### [Access Logging](https://www.openldap.org/doc/admin26/overlays.html#Access%20Logging) and [Audit](https://www.openldap.org/doc/admin26/overlays.html#Audit%20Logging) are two additional security features of LDAP that must be configured.

