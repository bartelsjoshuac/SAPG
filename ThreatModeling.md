# Threat Modeling and Observations
## Part 1: Threat Modeling

<!--- Josh Bartels --->
### Threats

## DFD 1

![Bind Threat](https://github.com/bartelsjoshuac/SAPG/blob/main/images/BINDThreat.jpg)

[Bind Threat TFT Report](https://htmlpreview.github.io/?https://github.com/bartelsjoshuac/SAPG/blob/main/HTML_Files/BINDThreatReport.htm)

## Part 2: Observations
Using findings from the DFD-based threat analysis from part 1 of this assignment, review your OSS project for design-related issues. Summarize your observations, in particular, highlight the gaps.

Include a link to your team's GitHub repository that shows your internal project task assignments and collaborations to finish this task. 

Include a reflection of your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward? 

The tools has limitations in that it does not accomadate modern firewall design, load balancers, or application firewalls, all of which are used today to mitigate threats to the system in its enterity.  While some of the flaws it can find automtically are valid, the majority are mitigated outside the scope of what the tool is considering.
