# VirusTotal Fundamentals
Welcome to our Security Opertions Center (SOC). Your role in the SOC today will be reviewing files in VirusTotal. VirusTotal can tell us a lot about these files and what you find can help the other SOC analysts understand what actions they should take: they may determine they should block file, block network communications, reset passwords, or isolate and remediate a host based on your findings.

## Objectives:
By the end of this course, you should be able to:
- Recognize benign files based off of specific details in the report
- Be familiar with finding important details in a report
- Pivot from indicators to learn more about a threat
- Be enabled to expand your knowledge of a threat
- Understand the significance of Code-Signing Certificates
- Understand which details about a file can be trusted and which ones should not be
- Know how to contribute to the community using the platform

## Getting started:
In this module, you will only use VirusTotal and a few outside sources to complete your work. Are you new to VirusTotal? No worries. The module is designed to help you get familiar with VirusTotal. 

This course is designed on the **Free** level of access to VirusTotal. There is a ton to learn through the free level of access and this module will help you do this. Howver, we do recommend creating a VirusTotal account, and you will need to do so for this course. With a free account, VirusTotal shows some additional information not available to those who aren't logged in. This is valuable information and we will take advantage of it in the course.

## I need help!
The best place to get help is the KC7 Discord! The Discord community can be accessed with this link: https://kc7cyber.com/community


## Glossary
**Code-Signing Certificate:** Also known as an "Authenticode Certificate". This is a digital signature included with a file to provide evidence that it came from a verified source. The signature contains a cryptographic hash of the file which must match the computed hash of the file: if the hashes do not match, it is evidence of tampering, and the signature will not be valid. Certificates can have the following statuses "Valid", "Does not Validate" (hashes don't match), or "Revoked".

**Masquerading**: This is an attempt to hide something from security tools by modifying its name or location to make it look like something else.

**MITRE ATT&CK**: This is a catalogue of cyber attack techniques and tactics. The tactics and techniques are organized into categories of the attack life-cycle.


## Resources
Some questions in the course reference MITRE ATT&CK. More about MITRE ATT&CK and the answers to the questions can be found from their website:
https://attack.mitre.org/

The course talks about the abuse of code-signing certificates. Resources to learn more about this problem are here:
https://squiblydoo.blog/2023/05/12/certified-bad/
https://squiblydoo.blog/2024/05/13/impostor-certs/
https://www.reversinglabs.com/blog/digital-certificates-impersonated-executives-as-certificate-identity-fronts

The course talks about inflated malware and references this tool:
https://github.com/Squiblydoo/debloat
