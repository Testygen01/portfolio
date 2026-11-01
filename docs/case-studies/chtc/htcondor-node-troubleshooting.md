# HTCondor Node Troubleshooting

`Linux` `HTCondor` `Distributed Computing` `Troubleshooting`

## Overview

I investigated an HTCondor execution node in the CHTC environment, e2466.chtc.wisc.edu, that did not appear to be functioning as expected. The node was visible in HTCondor status information, which indicated that it was successfully advertising itself to the collector. The node was not accepting jobs.

## The central question

The investigation turned on a distinction that mattered for the whole diagnosis: a node being visible to the collector is not the same as a node actually being available to run jobs. If I had stopped at visibility, the node would have looked healthy. The question I had to answer was why a node that was advertising itself was not behaving like a healthy execution node.

That question was sharper because of the node's recent history. The node had been drained and rebuilt about two days earlier, so it should have been receiving jobs again by the time I investigated it.

## Investigation

I connected to the node with SSH and ran `condor_status`. That confirmed the starting observation: the node was present in HTCondor status information, yet it was not accepting jobs.

From there, I checked the temporary Condor configuration file, `htcss.testing.config`, where temporary Condor configurations are written. In that file, I found a line setting `PreventJobsReason`, a setting that gives a reason why a job should be prevented from running on a specific node. That line explained the symptom I was seeing. The node could advertise itself to the collector while still being prevented from accepting jobs.

## Fix

I commented out the `PreventJobsReason` line and restarted Condor. I then checked the Condor systemd service with `systemctl status condor` to confirm that the service was running. I also checked the process tree and confirmed that `condor_master`, `condor_shared_port`, and `condor_startd` were present.

## Verification

After the fix, I verified the node by watching its behavior rather than relying on visibility alone. Different slots on e2466 were claimed by queued jobs, which showed that the node was now accepting work as an execution point should.

## Result and scope

The node was restored to service as an execution point that could accept queued jobs. I do not have a number for the scale of the impact, and I am not estimating one here. In a system that queues jobs, an execution point that cannot accept jobs reduces the capacity available to that queue, so restoring a node that should already have been back in service mattered even without a job count attached to it.

## What this shows

This case shows structured troubleshooting of a distributed system: starting from a precise symptom, separating visibility from availability, checking temporary configuration before assuming a deeper failure, making a single targeted change, and verifying the result by observing jobs claim slots on the node.
