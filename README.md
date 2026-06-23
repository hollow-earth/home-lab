# home-lab

## Storage
The NAS (UGREEN DXP4800 Plus, 4-bay) hosts one storage pool, which contains one vdev (raidz1), which is comprised of 4 physical disks. I chose raidz1 in order to maximize storage space while still retaining a parity disk: this allows me to have a total of 42TB of usable space. There is a secondary pool on another device which I periodically offload the important data for cold storage in order to have a more reliable storage solution. In case of data loss due to a force majeure or an malicious program, I will still have at least one offline copy. Naturally, both pools are fully encrypted (to be more technical, all the datasets are encrypted, not the pool itself).

```text
Storage
├── storage (ZFS pool)
│   ├── raidz1-0 (vdev)
│   |   ├── disk1: Seagate 24 TB (Shucked)
│   |   ├── disk2: WD 18 TB (Shucked)
│   |   ├── disk3: Seagate 14 TB (Recertified)
│   |   └── disk4: Seagate 14 TB (Recertified)
│   └── slog
│       └── SSD
│
└── storage_bak (Backup pool)
    └── single disk (vdev)
        └── disk 1: WD 5TB (External)
```

The storage pool has many datasets, all of which have different encryption keys:
```text
storage_bak/
├── archive/
├── audio/
├── backups/
│   ├── data/
│   ├── devices/
│   └── lxc/
├── docker/
├── documents/
│   ├── ledger/
│   ├── obsidian/
│   ├── resume/
│   └── taxes/
├── game_library/
├── games/
├── images/
├── literature/
├── security/
├── share/
├── software/
├── to_sort/
└── video/
    ├── ingest/
    ├── library/
    └── youtube/
```
## Datasets

- **archive**: Long-term archival storage for files that are rarely accessed but should be retained. Uses a high compression ratio.
- **audio**: Music library.
- **backups**
  - **data**: General-purpose backup dataset. Primarily stores BorgBackup repositories for workstation backups.
  - **devices**: Backups of external devices such as phones and the Steam Deck.
  - **lxc**: Backup storage for all LXC containers.
- **docker**: Legacy Docker data from a previous deployment that is being retained until it can be reviewed and migrated.
- **documents**: General documents including correspondence, legal documents, and personal records.
  - **ledger**: Financial records, receipts, and transaction history.
  - **obsidian**: Obsidian vault shared to my workstation via NFS. Frequent ZFS snapshots provide version history and protection against accidental changes.
  - **resume**: Résumé, cover letters, and job search documentation.
  - **taxes**: Provincial and federal tax records.
- **game_library**: Network-accessible game installation library shared over NFS. Allows Steam and Heroic Launcher to reuse existing installations instead of downloading games again.
- **games**: Game-related data including save files, ROMs, executables, mods, and reference material.
- **images**: Photo library managed primarily through PhotoPrism.
- **literature**: Books, comics, technical manuals, and textbooks.
- **security**: Encrypted backups and other sensitive data.
- **share**: General-purpose shared storage for manually transferring files between devices.
-  **software**: Software development projects, scripts, utilities, and archived applications.
- **video**
  - **ingest**: Staging area for newly acquired media before processing. Files are remuxed, transcoded with FFmpeg when necessary, and prepared for the media library.
  - **library**: Primary media library containing movies, television shows, and other videos.
  - **youtube**: Automatically synchronized YouTube content downloaded with `ytdl-sub` and indexed by Jellyfin for offline viewing.

I scheduled monthly scrubs in order to ensure data integrity.

## Services

## Networking

## Environment
