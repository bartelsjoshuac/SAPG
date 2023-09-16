# Project Name:  OpenLDAP

### ![Link to LDAP repo](https://github.com/openldap/openldap)
### ![Link to our GitHub](https://github.com/bartelsjoshuac/SAPG (ours))
### ![Link to our Project Board](https://github.com/users/bartelsjoshuac/projects/2)

## Operational Environment Description:
#### Every organization of any size, in theory, could employ LDAP (lightweight directory access protocol) servers, even if it is only to facilitate Windows Active Directory usage. LDAP directories are used in Identity Management (IDM) systems for authentication, authorization, and access control as well as auditing. Some typical usage of LDAP severs are: user authentication and authorization, centralizaed directory services like address books, email systems, and phone directories, single sign on (SSO), network management (for storing netowrk device information and manageing network resources), user group management, and integration with external directory services. Sometimes they are the central IDM or human resources (HR) sources, but more often are used as the “glue” between many different sources of identity information. In fact, the University of Nebraska System likely uses LDAP or a similar servise to manage the Single Sign On service. Because of this, we can say that our project is certainly used in many settings such as personal/home, office, enterprise, bank, and government envrionments. Numerous applications use LDAP directories in place of more traditional relational databases as well.

![Diagram]([https://github.com/bartelsjoshuac/SAPG/blob/main/images/Systems%20Engineering%20VIew.drawio.svg)

## Perceived Threats:
#### - Denial of Service
#### - Unauthorized changes to user information (permissions)
#### - Replication and referrals: utilizing internal LDAP protocol to falsify relationship between servers
#### - Modification of system attributes
#### - LDAP injections (similar to SQL injections)
#### - Typical network security threats (replay attacks, brute forcing credentials, man in the middle (MitM) attacks)

## Security Features:
#### - Access control list (ACLs)s and the Directory Information Tree (DIT)
#### - Audit logs (used in lieu of application logs)
#### - API (and Java Naming and Directory Interface (JNDI))
#### - Access tool (ldapsearch, ldapmodify, etc)
#### - Configuration items such as look through limits, referrals (and their ACL’s).
#### - SSL/TLS encryption
#### - Chroot support (confines server to specified directory to limit potential impact of a compromise)
#### - User and server attribute encryption support
#### - Simple Authentication and Security Layer (SASL) authentication and Simple Bind authentication

## Team Motivation
#### OpenLDAP has a deep history from the early days at MIT and the University of Michigan and is the root of every modern commercial LDAP implementation that serves as the core of an Identity and Access Management suite. Nearly every commercial implementation available today shows signs of its OpenLDAP roots, beginning with Netscape Directory Sever (iPlanet, Sun One, Oracle). The OpenLDAP Foundation was founded in 1998 during the iPlanet days. LDAP has been and will continue to be the foundation in SSP and IdM environments for the foreseeable future. While LDAP protocol is used by most people on a day to day basis, many are unfamiliar with its functionality at an architectural and security level. One team member has experience with nearly every commercial OpenLDAP implementation available, but never OpenLDAP, but the rest of the team was not familiar with LDAP. Our main motivation for focusing on OpenLDAP for our project was its wide ranging uses (as it LDAP is used everywhere) and the fact that it was new to a majority of the team- being as ubiquitous as it is, we thought it prudent to familiarize ourselves with LDAP and OpenLDAP.

## Open-source Project Description
#### LDAP is a protocol that stands for Lightweight Directory Application Protocol. A LDAP server can be any database on the backend, but is commonly a database type that is optimized for high performance read access vs. write. It's major components are: 1. slapd: This is the LDAP server. It listens for and processes client requests, performing actions like querying, modifying, and deleting entries in the directory. 2. slurpd: In older versions of OpenLDAP, this daemon handled replication between LDAP servers, though it's been deprecated in favor of the newer replication mechanisms. 3. LDAP Client Utilities: These are tools like ldapsearch, ldapmodify, ldapadd, and ldapdelete that allow users to interact with the LDAP server from the command line. OpenLDAP is scalable and extensible, and emplys replication, access control, and security features not inbuilt with other implementations of LDAP. It can easily be integrated with other external services. OpenLDAP can be run on Unix, Linux, BSD, and Windows systems. Several LDAP implementations use BerkelyDB or bTree SleepyCat style backend databases that are a black box to the server component. Today popular commercial implementations of OpenLDAP are available as: Microsoft Active Directory, Oracle, RadiantLogic, CA Directory, IBM Tivoli Diectory, etc.

#### OpenLDAP is written in C and runs on any platform which has a C compiler.  It is provided as source code only.  However, there are [binary packages available for many popular platforms](https://www.openldap.org/faq/data/cache/108.html)
#### The project has 85 contributors on the GitHub Mirror of OpenLDAP.org and more than 20k commits with weekly commits.  The [OpenLDAP Foundation](https://www.openldap.org/project/) has a list of the core team and contributors.  
#### OpenLDAP is included with RedHat Enterprise Linux (RHEL) an often used or required for advanced features of RHEL

## License
Open LDAP uses a BSD style [license.](https://www.openldap.org/software/release/license.html) which is a permissive free software license with limited restrictions on the use and distribution.

## History of Security Issues
#####  There are [50+ CVE's](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=OpenLDAP) for OpenLDAP as recent as May 2023 which the vast majority resulting in a denial of service.  They encompass many common vulnerabilities, SQL injection, buffer overflows, input validation, etc.  The associated JNDI access method for LDAP was the root behind the 2020/21 Log4J vulnerabilities as Log4J offers a feature to store log property configurations in LDAP. OpenLDAP, over the course of its life, has been subject to denial of service attacks, memory leaks, double free erros, and null-pointer dereferences, data integrity issues, and input validation issues, which have lead to developments with the software to address these issues. 

## Reflection on Teamwork:
#### The team is using Discord as a collaboration tool, which supports text, voice, sharing, and video on both desktop and mobile platforms and we will augment its use with Zoom as needed. The team needs to schedule meetings early in the week because of complex and conflicting schedules of full time day job students, and full time students with evening classes. Because of the commitments with the instructor and limited availability of real time communications, the agreed upon meeting times cannot be rescheduled to accommodate individuals. All project work will be concluded by Sunday at 10pm or within 24 hours of the due date, to allow all team members to start the following weeks activities with a blank plate. 
