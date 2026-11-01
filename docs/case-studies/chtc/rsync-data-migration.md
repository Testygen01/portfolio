# Migrating 930 GiB Past a CephFS Quota Failure
`Linux` `rsync` `CephFS` `Storage` `Troubleshooting`

## Overview

A researcher needed to move a dataset of over 930 GiB from `/home/h/<user_redacted>/DSCOVR` to `/staging/h/<user_redacted>` using rsync on Linux. The transfer failed with disk quota errors even though the destination filesystem had free space. The fix required understanding CephFS directory quotas and raising them with the cluster's quota wrapper scripts.

## The problem

The rsync output was blunt:

```
Disk quota exceeded (122)
rsync error: error in file IO (code 11)
rsync: [sender] write error: Broken pipe (32)
```

The destination had free space, so the quota being exceeded was not the filesystem filling up. It was a CephFS directory quota on the destination path, a separate administrative limit from overall capacity. Acting on the face-value reading, finding more space, would have wasted the effort; the actual limit was a quota that someone had to raise.

## Investigation

I investigated the CephFS quota wrapper scripts, `set_cephfs_dir_quota_items` and `set_cephfs_dir_quota_size`, to understand how quotas were applied on the cluster. The scripts ultimately expanded into `setfattr` commands setting `ceph.quota.max_bytes` and `ceph.quota.max_files` on the directory. That told me exactly which two knobs controlled the failure: the maximum file count and the maximum byte count under the destination path. Reading the mechanism beat guessing at it.

## Fix

I asked one of the research computing facilitators (RCFs) what the quotas should be, and they specified 500,000 files and 1.5 TiB. I set the file count quota first:

```
set_cephfs_dir_quota_items 500000 /staging/h/<user_redacted>
```

Then the size quota, passing gibibytes as the script expects (1.5 TiB is 1536 GiB):

```
set_cephfs_dir_quota_size 1536 /staging/h/<user_redacted>
```

The script converted 1536 GiB to bytes (1536 * 1024 * 1024 * 1024) and applied it via `setfattr`. With both quotas raised, I re-ran the rsync migration and monitored it through to verified completion.

## Result and scope

The full 930+ GiB dataset migrated successfully once the CephFS quotas reflected the actual need. The value of the work was in the diagnosis: reading the quota wrapper scripts to learn the mechanism instead of guessing, confirming the right values with the RCF rather than inventing them, and applying both limits before rerunning.

## What this shows

Storage troubleshooting below the surface. When rsync says quota but the filesystem says space, find the quota mechanism, here CephFS directory quotas via `setfattr`, and work it directly. It also shows operational judgment: confirm administrative values with the person authorized to set them, and validate the transfer after the fix instead of assuming the second attempt behaves differently.
