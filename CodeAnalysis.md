# Part 1: Code Review

We ran the code through the GitHub code scanning which found [14 alerts](https://github.com/bartelsjoshuac/openldap/security/code-scanning), all Critical or High.  Given the small number the fact that 9 of the 14 fall into the category of "Multiplication result converted to larger type".  This really just gives us 5 to look at, albeit 9 of them are in different places to examine they are all in the same ldn.c library file, which is the SleepyCat (Berkley DB back end)

We then selected Sonarcloud as another automated code scanning tool

Document your code review strategy along with responses to the following questions:

What challenges did you expect before starting the code review?  No members of the team and fluent in straightr C programming, although most are familair with C.

How did your code review strategy attempt to address the anticipated challenges?  The github code review categorizes vulnerabilities by easily researchable strings

Document findings from a manual code review of critical security functions identified in misuse cases, assurance cases, and threat models.

>>>>>> Josh will add something abhout the ldapsearch.c vulnerability found in Github

Document findings from automated code scanning (if available). Include links to tool outputs.

>>>>> Monirul insert link and description here or Sonarcloud

# Part 2: Key Findings and Contributions

Provide a summary of findings from manual and/or automated scanning. This summary should include mappings to CWEs to describe significant findings and perceive risk in your hypothetical operational environment.

Describe your planned or ongoing contributions to the upstream open-source project (I.documentation, design changes, code changes, communications, etc.). Your response can be based on any of the prior assignments in the class.

Include a link to your team's GitHub repository that shows your internal project task assignments and collaborations to finish this task. 

[Our GitHub OpenLDAP Fork](https://github.com/bartelsjoshuac/openldap)

[Github Code Scanning](https://github.com/bartelsjoshuac/openldap/security/code-scanning)

[Project Board](https://github.com/users/bartelsjoshuac/projects/2/views/1)


Include a reflection of your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward?
