# Cloud Security Assessment Framework
`Risk Management` `Security Frameworks` `CIS` `NIST 800-53`

## Overview

During my rotation with the DoIT Public Cloud Team, I helped build a standardized set of security assessment questions for evaluating risk in cloud-based projects. The team had no consistent way to assess a project's security posture early, so gaps surfaced late or not at all. Nine questions, each mapped to CIS and NIST 800-53 controls, now give the team's engineers a repeatable starting point for cloud security reviews.

## The problem

Cloud projects on the team varied widely in architecture, data handling, and access design, and there was no standard assessment to catch security gaps before projects moved forward. The team needed a short, practical question set that engineers could use with project stakeholders to surface risk early in the project lifecycle.

## How we worked

This was a collaboration with Pam Snyder, the team's cloud security analyst. We held project meetings at least twice a week. Pam led on the security concepts, since that is her specialty, and each meeting established what needed to be done next. Because she was busier than I was, I did the writing: I drafted each question, and in the following meeting we reviewed them together and cross-matched every question to the relevant NIST and CIS controls. The questions cover four areas: data storage and protection, identity and access management, resource governance, and network access.

## The questions

Three examples show the shape of the set:

- "Have you assigned a specific individual or role to regularly review and remediate security findings from the cloud provider security tools (e.g., AWS Security Hub, Microsoft Defender for Cloud)?"
- "Do you enforce least privilege access by granting only the specific permissions required and restricting to only the necessary IP ranges, services, or resources?"
- "Have you applied the Shared Security Responsibility Model (SSRM) for your project, clearly identifying which security controls are your responsibility and which are inherited from the Cloud Service Provider (CSP)?"

Each question is written so a project owner can answer it directly, and each maps to specific controls so engineers can trace an answer back to the framework behind it.

## Result and scope

Before my rotation ended, all nine questions were drafted and matched to CIS and NIST controls, and the set was sent to the team's Cloud Engineers for review. The review was still open when I rotated off. I met with Pam afterward and she confirmed the engineers finished their reviews. The plan is to use the question set for cybersecurity assessments of cloud-based projects to evaluate security risk. I do not have a count of projects assessed with it, since that work happened after I left, so I am stating the outcome honestly: a completed, peer-reviewed framework ready for use.

## What this shows

I can take a security concept from a senior analyst and turn it into a concrete, usable work product: drafting precise questions, mapping them to framework controls, and driving a review to completion across a team. The work was collaborative throughout, with Pam setting the security direction and me producing the drafts and the control mappings we reviewed together.
