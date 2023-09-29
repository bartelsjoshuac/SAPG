# Assurance Case for Software Security Engineering
<!---Expecations of Assignment for reference --->
## Part 1
* Using your findings from the requirements assignment, develop five to six (depending on the number of team members) top-level claims. The number of top-level claims can be adjusted to the number of students in your team. List these claims in your report.

  * Top-level claims pertain to high-risk critical security properties  
  * Having the claims focus on unique entities and related security properties will allow you to have broad coverage of the assurance needs from your OSS project. 

* Prepare an argument for each top-level claim that would convince stakeholders in your assumed/hypothetical operational environment. Use this as a planning exercise to figure out what sort of assurances would be necessary. They don't have to match what is currently available in the open-source software project. Document the argument and the needed evidence using an assurance case for each top-level claim. 

  * You may use this sample diagram Download sample diagram (open in http://app.diagrams.netLinks to an external site.) to get quick access to all the shapes needed for an assurance case
  * Use proper notations and ensure proper wording of assurance case elements. Avoid typos and grammatical errors as they can be distracting to stakeholders reading the assurance case for making trust decisions. 
  * Assurance cases should have reasonable depth and breadth to convince a technical expert. It is expected that the argument is coherent and convincing. 

---

<!--- End - Expecations, this can be removed later --->



<!--- Josh Bartels --->
### Assurance Case 1: Authenticate and Authorization (BIND)

![Assurance Case 1](https://github.com/bartelsjoshuac/SAPG/blob/main/images/BIND%20Assurance%20Case.svg)

C1: LDAP will perform the authentication request properly and handle all scenarios possible.

IR1: Pass thru authentication is a bit of a wildcard, and if enabled circumvents/interferes the anticpated process.

UC1: The process is further undercut as it will return a response which LDAP must assume to be true, but further examination is needed

R1: Assume ass thru authentication is not enabled.

SC2: Assume the user has a password.

E1: Authentication and log success.

R2: Unless the user is disabled

SC#: The user can not log in

E2: Log the failure and end

R3: Password was not valid.

E3: Log the failure and retry

IR2: Anonympous Authentication occured

UC2: Default to Read only or deny

SC4: Indeterminate handling as the client should not do this but can on E3 if desired (sceptical) 

---

<!--- End- Josh Bartels --->

## Part 2
<!---Expecations of Assignment for reference --->

* Assess the alignment of the evidence you identified in your diagrams with that available (or can be made available) from the OSS project. Highlight the gaps.
( Include a link to your team's GitHub repository that shows your internal project task assignments and collaborations to finish this task. 
* Include a reflection on your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward? 

---

<!--- End - Expecations, this can be removed later --->

It is critical to handle the BIND or authentication process properly as this identifies the end user and begins the authorization process (applie ACLs).  Pass thru authentication is a loop hole that removes trust and the associated evidence from the use case and while it may establish identity, the method in which it was completed is indeterminate.  In the case of a authentication failure, the user may step back to an anonympus BIND, and and authentication failure followed by a anonympus BIND from the same client must be treated as suspcious.  This requires looping back in the use case which is not possible in LDAP, hence leaving it incomplete as this would normally be the responsibility of a SIEM solution.  Although some LDAP varieties handle this, OpenLDAP does not.  All other cases are handled and evidence is logged appropriately for audit purposes.

