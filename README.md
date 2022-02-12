# RcloneZFSBackup
Backup ZFS snapshots to cloud storage using RClone rcat


This requires auto-zfs-snapshot to run at least daily and hourly backups and a working rclone remote. No snapshots are created, only holds placed and released

To create the initial backup, the yearly script sends the latest snapshot (including frequent or hourly snapshots) to a path in an rclone remote. This should be run manually to create the initial databackup and run as a cronjob on January 2nd every year

A cronjob for daily and monthly snapshots should be created passing the required parameters. The last snapshot with a rclone-backup hold will be used to send the incremental dataset to rclone. if no last snapshot exists with a hold, the most recent snapshot will be used.

The holds are only used to ensure the same reference snapshot is available for the next time this script runs. This allows you to manage how long a snapshot persists with other tools. The intention for data stored in an rclone remote is to keep 1 yearly initial snapshot, 11 monthly snapshots, and 90 daily snapshots in a rolling fashion

**Restoring**

restoring requires every snapshot file from the rclone remote to be downloaded and piped through zfs receive from the oldest yearly to the most recent daily snapshot. a script to restore data is being considered once backups are consistent. piping the output or rclone ls remote:path/to/snapshots through xargs to execute "rclone cat $1 | zfs receive recoverpool/recoverdata" is a manual method for this restoral 

Any pull requests appreciated.
