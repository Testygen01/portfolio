# Jira Cloud Automation Rules
`Jira Cloud` `Automation` `Regex`

## Overview

During my rotation with the DoIT Public Cloud Team, I built seven Jira Cloud automation rules that triaged cloud alert tickets and kept stale tickets moving. One rule processed roughly 500 AWS health alert tickets. It populated their fields and then closed them. Others moved untouched tickets into an autoresolution flow. Regex did the heavy lifting of pulling cloud account IDs out of ticket text, since Azure, AWS, and GCP each use a different ID format.

## The problem

There were two problems. First, an alert queue automation created before my time collected every AWS health alert ticket into one queue. Over time the ticket count kept growing, clogging the UI and burying anything important. That is classic alert fatigue: when everything is an alert, nothing is. Second, tickets that went stale sat untouched with no nudge to the owner, so the backlog grew instead of resolving.

## How I built it

I used Jira Cloud's built-in automation engine, no plugins. The core technique was regex. The rules needed the cloud account ID from each ticket's body or description to populate fields, but the three providers format IDs differently: Azure uses a GUID for the subscription ID, AWS uses a 12-digit numeric account ID, and GCP uses a project ID of 6 to 30 characters. One regex per format let a single rule handle tickets from all three clouds.

## The automations

Three examples from the seven rules:

1. **AWS health alert triage.** The rule picked up the backlog of about 500 AWS health alert tickets, populated fields including project, cloud provider, and cloud account ID (extracted with regex), then transitioned each ticket to Done. The rule retained critical alerts instead of sweeping them up in the bulk close.
2. **Stale ticket nudge.** Tickets with no modification for one week moved to "Pending - Autoresolution," and the rule emailed the ticket owner and requestor asking for action.
3. **Auto-close.** The rule closed tickets automatically after another week in "Pending - Autoresolution".

## Result and scope

The alert queue went from roughly 500 open alert tickets to a clean queue with only critical alerts retained, which cut the noise the team had to wade through every day. Stale tickets now get a warning email after a week of inactivity and close after two, instead of lingering indefinitely. I am stating the queue size honestly: about 500 tickets processed by the triage rule, per the backlog I saw.

## What this shows

This work started from an operational pain point, alert fatigue and ticket rot, and ended with automation handling both. The regex work shows I think about real-world data formats rather than assuming clean input, and the autoresolution flow respects the humans in the loop by warning first and acting second.
