# Recovering a Chromebook from Cryptohome Failure
`ChromeOS` `Crostini` `Endpoint Support` `Troubleshooting`

## Overview

A student's Chromebook could not launch its Linux terminal, and the repair path led through a ChromeOS update into a cryptohome mount failure that locked the student out of the device entirely. A Powerwash reset resolved it. The case is a clean example of escalation discipline: non-destructive steps first, documented procedures next, destructive reset only after I had exhausted the alternatives.

## The problem

The student needed terminal access, so I started the penguin container, the default Crostini Linux container on ChromeOS. It failed with `Error starting crostini for terminal: 62 (NEED_UPDATE)`. The error code pointed at the operating system itself: the installed ChromeOS version was too old for the terminal to start. That made the next step obvious, but the step after it was not.

## Investigation

Following a documented troubleshooting path, I updated ChromeOS on the device, which is the reasonable response to a NEED_UPDATE error. The update completed and I rebooted. The student then could not sign back in at all: the device reported `can't mount crypto home`. Cryptohome is ChromeOS's encrypted per-user storage, so a mount failure there means the user's entire profile, including the Crostini container, is unreachable. I worked through the troubleshooting steps in Google's support documentation for this exact error, and none of them resolved it.

## Fix

With the documented non-destructive steps exhausted, I reset the Chromebook with a Powerwash, which returns it to its factory state. That resolved the mount failure and the device came back healthy. A Powerwash erases local data, so this was not a step to take lightly; it was the last resort after the update path and the support documentation both failed. I want to report the outcome precisely: I restored the device to service, but I do not have a record of which local files, if any, the student lost in the reset.

## Result and scope

The student got a working device back the same day instead of a multi-day repair cycle. The case stayed at the help desk the whole way through: terminal error, OS update, sign-in failure, documented troubleshooting, and finally the reset. Each step followed from the evidence of the previous one rather than from guessing.

## What this shows

This case demanded disciplined endpoint troubleshooting under a real constraint. The NEED_UPDATE error had one reasonable response, and that response created a worse problem, which is exactly when discipline matters most: I worked the documented steps, confirmed each failure before moving on, and took the destructive step only after nothing else remained. It also demonstrates working knowledge of the ChromeOS stack, including Crostini containers, cryptohome, and the recovery procedures around them.
