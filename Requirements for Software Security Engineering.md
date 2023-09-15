# Requirements for Software Security Engineering

## Part 1

### Use Case 1: BIND
#### The BIND operation identifies the actor to the server.  LDAP will typical allow anonymous BIND operations which may or may not be disabled, depending on business requirements.  ACL’s are applied to the actor that bound to the system.  So binding anonymously would typically be configured with read only access, where binding as cn=Directory Manager often applies no ACL’s at all.

#### Use:

![Use-Case-1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/Use%20Case%201%20-%20Bind.drawio.svg)

#### The actor bind with a valid Distinguished Name (DN) and simple password to establish their identity to the LDAP server.  The LDAP server will them wait for additional operations, and apply ACL’s based on that identity.


#### UseMisuse:

![Use-Misuse-Case 1 - Bind](https://github.com/bartelsjoshuac/SAPG/blob/main/Use-Misuse%20Case%201%20-Bind.drawio.svg)

#### The bad actor is attempting to determine the password of a good actor’s they obtained from a previous anonymous BIND and SRCH request by performing a password spraying attack.  The LDAP server will prevent this attack from being successful by applying tracking loginAttempts and allowing a max of X, before disabling the account.  It will not assist the attacker by differentiating to the bad actor if it was the username or password that was incorrect by informing them of the lock out.  The lockout shall remain in place for Y number of minutes, or until cleared by a Directory Manager.

![Use-Misuse-Case Final](https://github.com/bartelsjoshuac/SAPG/blob/main/Use-Misuse%20Case-Final1%20-Bind.drawio.svg)

---
### Use Case 2: ADD

---
### Use Case 2: DEL

---
### Use Case 4: MDFY

---
### Use Case 5: SRCH


## Part 2
#### The [Security section](https://www.openldap.org/doc/admin26/security.html) of the [OpenLDAP Documentation](https://www.openldap.org/doc/admin26/) is rather brief.  It focuses on Network Security which can be summed up as TLS.  By default passwords in LDAP are stored in clear text to comply with [RFC4519](https://www.rfc-editor.org/rfc/rfc4519.txt) which is not desirable.  While several encryption methods are mentioned, it is suggested to use SSHA, or the salted version of the SHA scheme.

#### [Access Controls](https://www.openldap.org/doc/admin26/access-control.html) lists are the primary security feature of OpenLDAP, or any LDAP.  These control who can do what.  Read, Write, Delete, etc.

#### [Access Logging](https://www.openldap.org/doc/admin26/overlays.html#Access%20Logging) and [Audit](https://www.openldap.org/doc/admin26/overlays.html#Audit%20Logging) are two additional security features of LDAP that must be configured.

