# Part 1: Code Review

As a team, decide your code review strategy. So, what do I mean by a code review strategy? While there is no one-size-fits-all approach, here is what has worked well in the past. 

Scope your code review effort to specific code modules/files relevant to misuse cases, assurance claims, and threat models. This is a scenario or weakness-based approach.

Identify a list of 5-10 CWEs (as specific as possible) that would be most important for findings from your manual or automated code review. The selection of CWEs will depend on the type of programming language, platform, and architecture of your project. Knowing what you are looking for in a large codebase will help you focus your efforts. This is a checklist-based approach. 

Select automated code-scanning tools based on the software composition of your project. One tool may not be enough. If no free and open-source tools are available, see if you can get a free trial version for a few days. 
Put more emphasis on systematic manual code review if your project does not support automated code scanning. 

Document your code review strategy along with responses to the following questions:

What challenges did you expect before starting the code review?

How did your code review strategy attempt to address the anticipated challenges?

Document findings from a manual code review of critical security functions identified in misuse cases, assurance cases, and threat models.

Document findings from automated code scanning (if available). Include links to tool outputs.

# Part 2: Key Findings and Contributions

Provide a summary of findings from manual and/or automated scanning. This summary should include mappings to CWEs to describe significant findings and perceive risk in your hypothetical operational environment.

Describe your planned or ongoing contributions to the upstream open-source project (I.documentation, design changes, code changes, communications, etc.). Your response can be based on any of the prior assignments in the class.

Include a link to your team's GitHub repository that shows your internal project task assignments and collaborations to finish this task. 

Include a reflection of your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward?
