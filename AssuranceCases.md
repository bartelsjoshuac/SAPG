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
<!--- End - Expecations, this can be removed later --->


<!--- Josh Bartels --->
### Assurance Case 1: Authenticate and Authorization (BIND)

![Assurance Case 1](https://github.com/bartelsjoshuac/SAPG/blob/main/images/BIND%20Assurance%20Case.svg)

C1: LDAP will perform the authentication request.

 IR1: This is not handled by LDAP and indeterminate.

  UC1: This case is not handled by LDAP.

R1: Assuming pass thru authentication is not enabled.

SC2: And the user has a password.

E1: Authentication and log success.

R2: Unless the user is disabled and.

E2: Log failure and reason code.

R33: Password was not valid.

IR2: Anonymous authentication is allowed.

UC2: Default to Read only or Deny.

SC4: This is indeterminate.

R3: Or the user has the wrong password.

E3: Log fail and repeat.


#### Use:

<!--- End- Josh Bartels --->
---

## Part 2
<!---Expecations of Assignment for reference --->

* Assess the alignment of the evidence you identified in your diagrams with that available (or can be made available) from the OSS project. Highlight the gaps.
( Include a link to your team's GitHub repository that shows your internal project task assignments and collaborations to finish this task. 
* Include a reflection on your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward? 

<!--- End - Expecations, this can be removed later --->
