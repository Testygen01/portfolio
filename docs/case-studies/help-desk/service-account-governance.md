# Service Account Governance for a Student Organization
`IAM` `Identity and Access Management` `Security` `Customer Support`

## Overview

A registered student organization needed shared communication channels and a website presence that would survive changes in its leadership. The request arrived at the help desk as a routine access question, but the correct answer depended on governance, not just tooling.

## The problem

The organization needed shared access to communication and online resources. The expedient option would have been a personal NetID belonging to a current officer, but that would have tied ownership of the organization's accounts to one individual. Student organizations turn over their leadership every year or two, so the failure mode was not hypothetical. When the account holder graduated or stepped down, the organization would have faced lockouts, awkward credential handoffs outside any approved process, and an audit trail that pointed at a person who was no longer involved. The request needed a solution designed for shared, long-term ownership rather than individual convenience.

## What I did

I evaluated the request against identity management best practices and the university's account governance policies instead of treating it as a simple provisioning ticket. The review came down to a lifecycle question: who owns this access in two years, and how does it transfer? A personal NetID had no good answer. That pointed to service accounts as the appropriate mechanism, since service accounts exist for shared organizational use rather than being tied to one person's identity, and their ownership can move with the organization's leadership. I escalated the request to the Productivity and Collaboration Services (PCS) team, which owns service account provisioning, rather than trying to implement it from the help desk.

## Result and scope

To be clear about the boundary of my involvement: PCS contacted the customer directly and took the request from there, so I did not see the final provisioning through and cannot confirm that PCS created the accounts. I identified the correct mechanism, verified it against policy, and routed the organization to the team that owns it, which avoided the continuity and security risks a personal NetID would have created as leadership turned over. Because I routed it correctly at the start, nobody had to unwind a personal-account setup later.

## What this shows

Support work is not only about fixing what is broken. It also means recognizing when the expedient handling of a request would create a larger problem later. This case shows governance-aware triage. I worked from the identity model behind the request, reasoned about the full lifecycle of the access being granted, chose the mechanism that fit the organization's actual lifespan, and routed the work to the right owners instead of improvising a shortcut that the next officer would inherit as a crisis.
