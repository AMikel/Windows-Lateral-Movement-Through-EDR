# Investigation of Lateral Movement on Windows through EDR

This guide is used for looking into the various types of lateral movement in Windows and how to track them through an EDR (Endpoint Detection and Response) platform.

This guide will present the activity as seen from both the source host as well as on the destination host. The type of activity will be split into the different types that are captured by EDR software.

Some EDR software may not capture persistence events directly or Windows Events. However, they can still be investigated if that information is found on a secondary source like a SIEM (Security Information and Event Management).

Other events like network activity, command line, and file metadata are fairly common on most EDR products and should be relevant to this guide.

Based on the SANS Hunt Evil poster.


- [Mapping of Network Share](docs/Mapping%20of%20Network%20Share.md)
- [PowerShell](docs/PowerShell.md)
- [PsExec](docs/PsExec.md)
- [Remote Desktop Protocol](docs/Remote%20Desktop%20Protocol.md)
- [Scheduled Tasks](docs/Scheduled%20Tasks.md)
- [Services](docs/Services.md)
- [WMI](docs/WMI.md)



### Reference Links

#### Official Documentation and Guides
- [SANS Hunt Evil Poster](https://www.sans.org/posters/hunt-evil/)
- [Microsoft Sysinternals](https://docs.microsoft.com/en-us/sysinternals/)
- [PowerShell Scripting Guide](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.1)

#### Useful Tools and Resources
- [Sysinternals Suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
- [MITRE ATT&CK® Framework](https://attack.mitre.org/)
- [Endpoint Detection and Response (EDR) Solutions](https://www.gartner.com/reviews/market/endpoint-detection-and-response-solutions)

#### Community and Learning
- [Reddit - r/netsec: A community for technical news and discussion of information security and closely related topics](https://www.reddit.com/r/netsec/)
- [SANS Institute Reading Room - Whitepapers on Incident Detection and Response](https://www.sans.org/white-papers/))

<!--
Keywords: Lateral Movement Detection, Windows Security Monitoring, Endpoint Detection and Response (EDR), EDR Security Techniques, Investigating Windows Attacks, SIEM Security Analysis, Windows Event Logs Analysis, Command Line Monitoring, Network Traffic Analysis in EDR, PowerShell Security Practices, Remote Desktop Protocol Security, Scheduled Tasks for Persistence, Windows Services Manipulation, WMI Attacks and Defense
-->
