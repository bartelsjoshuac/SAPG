# Assurance Case for Software Security Engineering

## Part 1
Using your findings from the requirements assignment, develop five to six (depending on the number of team members) top-level claims. The number of top-level claims can be adjusted to the number of students in your team. List these claims in your report.
Top-level claims pertain to high-risk critical security properties  
Having the claims focus on unique entities and related security properties will allow you to have broad coverage of the assurance needs from your OSS project. 
Prepare an argument for each top-level claim that would convince stakeholders in your assumed/hypothetical operational environment. Use this as a planning exercise to figure out what sort of assurances would be necessary. They don't have to match what is currently available in the open-source software project. Document the argument and the needed evidence using an assurance case for each top-level claim. 
You may use this sample diagram Download sample diagram (open in http://app.diagrams.netLinks to an external site.) to get quick access to all the shapes needed for an assurance case
Use proper notations and ensure proper wording of assurance case elements. Avoid typos and grammatical errors as they can be distracting to stakeholders reading the assurance case for making trust decisions. 
Assurance cases should have reasonable depth and breadth to convince a technical expert. It is expected that the argument is coherent and convincing. 

<!--- Josh Bartels --->
### Assurance Case 1: Authenticate and Authorization (BIND)
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

## Part 2

Assess the alignment of the evidence you identified in your diagrams with that available (or can be made available) from the OSS project. Highlight the gaps.
Include a link to your team's GitHub repository that shows your internal project task assignments and collaborations to finish this task. 
Include a reflection on your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward? 
