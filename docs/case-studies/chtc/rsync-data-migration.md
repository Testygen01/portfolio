# Migrating 930 GiB Past a CephFS Quota Failure
`Linux` `rsync` `CephFS` `Storage` `Troubleshooting`

## Overview

I was asked to move a dataset of over 930 GiB from `/home/h/<user_redacted>/DSCOVR` to `/staging/h/<user_redacted>`. I chose rsync over a plain `mv` because rsync can resume after a failure instead of starting over. That choice paid off: the transfer failed partway through, and the fix required understanding CephFS directory quotas.

## The problem

Shortly after I initiated the move, rsync failed with:

```
Disk quota exceeded (122)
rsync error: error in file IO (code 11)
rsync: [sender] write error: Broken pipe (32)
```

The destination filesystem had free space, so the quota being exceeded was not the disk filling up. It was a CephFS directory quota on the destination path, a separate administrative limit from overall capacity. Acting on the face-value reading, finding more space, would have wasted the effort; the actual limit was a quota that someone had to raise.

## Investigation

I investigated the CephFS quota wrapper scripts, `set_cephfs_dir_quota_items` and `set_cephfs_dir_quota_size`, to understand how quotas were applied on the cluster. The scripts ultimately expanded into `setfattr` commands setting `ceph.quota.max_bytes` and `ceph.quota.max_files` on the directory. That told me exactly which two knobs controlled the failure: the maximum file count and the maximum byte count under the destination path.

## Fix

I asked one of the research computing facilitators (RCFs) what the quotas should be, and they specified 500,000 files and 1.5 TiB. I set the file count quota first:

```
set_cephfs_dir_quota_items 500000 /staging/h/<user_redacted>
```

Then the size quota, passing gibibytes as the script expects (1.5 TiB is 1536 GiB):

```
set_cephfs_dir_quota_size 1536 /staging/h/<user_redacted>
```

The script converted 1536 GiB to bytes (1536 * 1024 * 1024 * 1024) and applied it via `setfattr`. With both quotas raised, I re-ran the rsync migration. Because rsync resumes rather than restarts, the transfer picked up where it had failed instead of re-sending 930 GiB from scratch. I monitored it through to verified completion.

## Result and scope

The full dataset migrated successfully once the CephFS quotas reflected the actual need. The value of the work was in two decisions: choosing rsync up front so a failure would be resumable, and reading the quota wrapper scripts to learn the mechanism instead of guessing.

## What this shows

This case demonstrates storage troubleshooting that goes below the surface, combined with deliberate tool choice. When rsync reports a quota error but the filesystem has free space, the right move is to identify the actual quota mechanism, in this case CephFS directory quotas applied through `setfattr`, and work with it directly. The failed first attempt cost minutes instead of hours for one reason: rsync resumes where it stopped, while `mv` would have started over.
