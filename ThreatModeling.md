# Threat Modeling and Observations

![Project Board](https://github.com/users/bartelsjoshuac/projects/2/views/1)

## Part 1: Threat Modeling

<!--- Josh Bartels --->
### Threats

## DFD 1

![Bind Threat](https://github.com/bartelsjoshuac/SAPG/blob/main/images/BINDThreat.jpg)

[Bind Threat TFT Report](https://htmlpreview.github.io/?https://github.com/bartelsjoshuac/SAPG/blob/main/HTML_Files/BINDThreatReport.htm)

![Del Threat](https://github.com/bartelsjoshuac/SAPG/blob/main/images/LdapDelDfd.jpg)



## Part 2: Observations

The tools has limitations in that it does not accomadate modern firewall design, load balancers, or application firewalls, all of which are used today to mitigate threats to the system in its enterity.  While some of the flaws it can find automtically are valid, the majority are mitigated outside the scope of what the tool is considering.
