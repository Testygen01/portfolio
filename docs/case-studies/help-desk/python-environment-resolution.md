# Resolving a Python Environment Issue for Coursework
`Python` `Conda` `Troubleshooting` `Customer Support`

## Overview

A student could not complete coursework because the required Python version would not run on their laptop. The course needed Python 3.10.12, but the student's Anaconda installation had set up Python 3.13. Two commands in the terminal fixed it, plus one more for the dependency error that surfaced underneath.

## The problem

The student had installed Python through Anaconda, which pulled the recent 3.13 release. Their coursework specifically required 3.10.12, and the newer version would not run what the course needed. The machine was fine and the installation was healthy; it was simply the wrong version. The fix had to swap the Python version inside the existing Anaconda setup without disturbing anything else on the system.

## Investigation

The reported symptom was "Python 3.10.12 does not work," which could have meant many things. Checking the actual environment showed the real situation: there was no 3.10.12 on the machine at all, only the 3.13 that Anaconda had installed. That reframed the problem from something broken to something missing. The fix was a version change, not a repair, which meant the risk was low as long as the change stayed inside the Conda environment.

## Fix

From the terminal I ran `conda install python=3.10.12`, which downgraded the environment's Python to the required version. That surfaced the next layer: importing packages failed with `ModuleNotFoundError: No module named menuinst`. Following a documented resolution for this exact error, I ran `conda install menuinst`, which resolved the missing module. After both installs, the environment executed the coursework successfully.

## Result and scope

The student was unblocked and able to continue coursework the same day. The issue was resolved at the help desk without escalation, which would have cost the student days of waiting for a problem that turned out to need two package installs. The value of the work was in the diagnosis: verifying what was actually installed before acting, rather than treating "does not work" as a single problem with a single fix.

## What this shows

This case shows structured troubleshooting of a developer environment, combined with restraint. I checked the ground truth first, fixed the specific mismatch, and expected the next layer: version changes in Conda frequently surface dependency errors like the missing menuinst module, and resolving them in sequence instead of starting over is what keeps a ten-minute fix from becoming a reinstall.
