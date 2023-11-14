<!--- Josh Bartels --->
# Threat Modeling and Observations

![Project Board](https://github.com/users/bartelsjoshuac/projects/2/views/1)

## Part 1: Threat Modeling


### Threats

Our threat examination and mitigation documentation are included below in the HTML reports generated from TFT.

## DFD 1

![Bind Threat](https://github.com/bartelsjoshuac/SAPG/blob/main/images/BINDThreat.jpg)

[Bind Threat TMT Report](https://htmlpreview.github.io/?https://github.com/bartelsjoshuac/SAPG/blob/main/HTML_Files/BINDThreatReport.htm)

----------------------------
<!--- Sam --->
## DFD 2

![MDFY/SRCH](https://github.com/bartelsjoshuac/SAPG/blob/main/images/LdapDelDfd.PNG)
![MDFY/SRCH Threat](**https://htmlpreview.github.io/?**https://github.com/bartelsjoshuac/SAPG/blob/main/HTML_Files/SRCH_MDFY_Threat_Report.htm)

## Part 2: Observations
<!--- Josh Bartels and Adam Stemmler --->
WIP
The tools has limitations in that it does not accomodate many aspects of modern software development and network infrastructure, namely modern firewall design, load balancers, or application firewalls, all of which are used today to mitigate threats to the system in its enterity.  While some of the flaws it can find automtically are valid, the majority are mitigated outside the scope of what the tool is considering.  The tool is unable to digest the LDAP protocol and discover more threats associated with LDAP.  Further, after simplifying the drawing to not include the in memory/in process btree database and cache as well as the log files, not much is left for the TFT tool to analyze. Because of this, our first diagram is left feeling somewhat empty. We created a second diagram that shows more pieces of the OpenLDAP process, as well as including other parts that we intentionally left out of the first diagram that are important to show the full flow of data and analyze security threats. While both diagrams can be applied to each of our scenarios, they do encompass different aspects of the scenarios. Our first diagram applies to BIND specifically as BIND does not require as much interaction with data and the database as the other scenarios, but the authentication and login process illustrated by the first DFD can be applied to every scenario. The second DFD applies best to every scenario involving SRCH or MDFY(adding, deleting, otherwise modifying data in the database). 

In terms of threats identified by TFT, they all fall into categories we had identified in our earlier work as potential problems OpenLDAP must address. OpenLDAP does adress many of them, however, many others are outside the scope of OpenLDAP and thus OpenLDAP cannot mitigate these threats itself. Thankfully, there is a suite of both hardware and software tools often used in conjunction with OpenLDAP that mitigate the rest of the threats we previously identified and those identified by TFT. Our first DFD produced 10 threats for use to mitigate. The STRIDE threat model from the lectures helped us better conceptualize and address previously identified issues, many of which were not as specific as the those identified by TFT. The TFT threats also gave us more concrete examples of the kinds of threats OpenLDAP should be able to resist.  

10 threats and mitigations for first diagram
we already addressed a lot of the mitigations in past assignments
alot of analysis on gaps already done, look at past assignments
STRIDE!!!

<!--- Adam Stemmler --->
### Reflection
Some of our team had issues using TFT, so that inhibited progress slightly. We spent a lot of time on our DFDs. As discussed, our first diagram does not include many aspects of a DFD discussed in the lectures, but through discussions with the professor and back and forth between the team, we decided our first diagram did not need those other aspects and encompassed everything it needed for all our exisitng scenarios. We created the second diagram to reflect other pieces that may be slightly outside the scope of OpenLDAP itself but make sense to include to visualize the full picture of OpenLDAP data flow. In terms of teamwork, we again need to facilitate team cohesion, but we were able to get all the diagrams and accompanying analysis and documentation completed. 

