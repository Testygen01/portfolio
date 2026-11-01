# Service Account Governance for a Student Organization
`IAM` `Identity and Access Management` `Security` `Customer Support`

## Overview

A registered student organization needed shared communication channels and a website presence that would survive changes in its leadership. The request arrived at the help desk as a routine access question, but the correct answer depended on governance, not just tooling.

## The problem

The organization needed shared access to communication and online resources. The expedient option would have been a personal NetID belonging to a current officer, but that would have tied ownership of the organization's accounts to one individual. Student organizations turn over their leadership every year or two, so the failure mode was not hypothetical. When the account holder graduated or stepped down, the organization would have faced lockouts, awkward credential handoffs outside any approved process, and an audit trail that pointed at a person who was no longer involved. The request needed a solution designed for shared, long-term ownership rather than individual convenience.

## What I did

I evaluated the request against identity management best practices and the university's account governance policies instead of treating it as a simple provisioning ticket. The review came down to a lifecycle question: who owns this access in two years, and how does it transfer? A personal NetID had no good answer. That pointed to service accounts as the appropriate mechanism, since service accounts exist for shared organizational use rather than being tied to one person's identity, and their ownership can move with the organization's leadership. I escalated the request to the Productivity and Collaboration Services (PCS) team, which owns service account provisioning, rather than trying to implement it from the help desk.

## Result and scope

I am stating the boundary of my involvement honestly: PCS contacted the customer directly and took the request from there, so I did not see the final provisioning through and cannot confirm the accounts were created. My contribution was diagnostic and directional. I identified the correct mechanism, verified it against policy, and routed the organization to the team that owns it, avoiding the continuity and security risks a personal NetID would have created as leadership turned over. Getting the routing right at the start also saved everyone the rework of unwinding a personal-account setup later.

## What this shows

Support work is not only about fixing what is broken. It is also about recognizing when a request, handled the expedient way, would create a larger problem later. This case shows governance-aware triage: understanding the identity model behind a request, reasoning about the full lifecycle of the access being granted, choosing the mechanism that fits the organization's actual lifespan, and routing the work to the right owners instead of improvising a shortcut that the next officer would inherit as a crisis.
