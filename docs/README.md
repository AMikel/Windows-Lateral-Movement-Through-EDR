# Investigation of Lateral Movement on Windows through EDR

This guide is used for looking into the various types of lateral movement in Windows and how to track them through an EDR (Endpoint Detection and Response) platform.

This guide will present the activity as seen from both the source host as well as on the destination host. The type of activity will be split into the different types that are captured by EDR software.

Some EDR software may not capture persistence events directly or Windows Events. However, they can still be investigated if that information is found on a secondary source like a SIEM (Security Information and Event Management).

Other events like network activity, command line, and file metadata are fairly common on most EDR products and should be relevant to this guide.

Based on the SANS Hunt Evil poster.
