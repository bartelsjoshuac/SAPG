# Project Name:  OpenLDAP
### https://github.com/openldap/openldap
### https://github.com/bartelsjoshuac/SAPG (ours)
## Operational Environment Description:
#### Every enterprise has some sort of LDAP directory service, even if it is only Active Directory.  LDAP directories are used in Identity Management systems for authentication, authorization, and access control as well as auditing.   Sometimes they are the central IDM or HR source, but more often are used as the “glue” between many different sources of identity information. Numerous applications use LDAP directories in place of a database as well.  

![Diagram](https://github.com/bartelsjoshuac/SOA/blob/main/Systems%20Engineering%20VIew.drawio.svg)
## Security Features:
#### - Access control list (ACLs)s and the Directory Information Tree (DIT)
#### - Audit log (used in lieu of application logs)
#### - API (and JNDI)
#### - Access tool (ldapsearch, ldapmodify, etc)
#### - Configuration items such as lookthroughlimits, referrals (and their ACL’s).

## Team Motivation
#### OpenLDAP has a deep history from the early days at MIT, the University of Michigan, and is the root of every modern commercial LDAP implementation that serves as the core of an Identity and Access Management suite.  Nearly every commercial implementation available today shows signs of its OpenLDAP roots, beginning with Netscape Directory Sever (iPlanet, Sun One, Oracle)   The OpenLDAP Foundation was founded in 1998 during the iPlanet ays.  LDAP has been and will continue to be the foundation in SSP and IdM environments for the foreseeable future.  One team member has experience with every commercial OpenLDAP implementation available, but never OpenLDAP, but the rest of the team was not familiar with LDAP.

## Open-source Project Description
#### LDAP is a protocol that stands for Lightweight Directory Application Protocol.  A LDAP server can be any database on the backend, but is commonly a database type that is optimized for high performance read access vs. write.  Referring to LDAP as a server is a bit of a misnomer, but it common use  OpenLDAP derived with early x.500 work down by MIT, the University of Michigan, and Berkeley.   Several LDAP implementations use BerkelyDB or bTree SleepyCat style backend databases that are a black box to the server component. Today popular commercial implementations of OpenLDAP are available as: Microsoft Active Directory, Oracle, RadiantLogic,  CA Directory, IBM Tivoli Diectory, etc.

#### OpenLDAP is written in C and runs on any platform which has a C compiler.  It is provided as source code only.  However, there are [binary packages available for many popular platforms](https://www.openldap.org/faq/data/cache/108.html)
#### The project has 85 contributors on the GitHub Mirror of OpenLDAP.org and more than 20k commits with weekly commits.  The [OpenLDAP Foundation](https://www.openldap.org/project/) has a list of the core team and contributors.  
#### OpenLDAP is included with RedHat Enterprise Linux (RHEL) an often used or required for advanced features of RHEL

## License
Open LDAP uses a BSD style [license.](https://www.openldap.org/software/release/license.html) which is a permissive free software license with limited restrictions on the use and distribution.

## History of Security Issues
#####  There are [50+ CVE's](https://www.cvedetails.com/vulnerability-list/vendor_id-439/Openldap.html) for OpenLDAP as recent as May 2023 which the vast majority resulting in a denial of service.  They encompass many common vulnerabilities, SQL injection, buffer overflows, input validation, etc.  The associated JNDI access method for LDAP was the root behind the 2020/21 Log4J vulnerabilities as Log4J offers a feature to store log property configuration LDAP.

## Reflection on Teamwork:
#### The team is using Discord as a collaboration tool, which supports txt, voice, sharing, and video on both desktop and mobile platforms and will augment it with Zoom as needed.  The team needs to schedule meetings early in the week because of complex and conflicting schedules of full time day job students, and full time students with evening classes.  Because of the commitments with the instructor and limited availability of real time communications ,the agreed upon meeting times cannot be rescheduled to accommodate individuals.  All project work will be concluded by Sunday at 10pm or within 24 hours of the due date, to allow all team members to start the following weeks activities with a blank plate.

