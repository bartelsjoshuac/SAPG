# Requirements for Software Security Engineering

## Part 1


## Part 2
#### The [Security section](https://www.openldap.org/doc/admin26/security.html) of the [OpenLDAP Documentation](https://www.openldap.org/doc/admin26/) is rather brief.  It focuses on Network Security which can be summed up as TLS.  By default passwords in LDAP are stored in clear text to comply with [RFC4519](https://www.rfc-editor.org/rfc/rfc4519.txt) which is not desireable.  W#hile several encryption methods are mentioned, it is suggested to use SSHA, or the saslted version of the SHA scheme.
