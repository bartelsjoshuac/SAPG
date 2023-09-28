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

<!--- End --->
---

## Part 2

Assess the alignment of the evidence you identified in your diagrams with that available (or can be made available) from the OSS project. Highlight the gaps.
Include a link to your team's GitHub repository that shows your internal project task assignments and collaborations to finish this task. 
Include a reflection on your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward? 
