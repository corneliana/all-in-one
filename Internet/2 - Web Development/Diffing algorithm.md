Identify differences between two sets of information and then updates only what has changed.

[[DOM Reconciliation]]
[[VCS Version Control System]]

### Database Replication and Synchronization
- **Purpose**: Keeps data consistent across different DB or DB instances.
- **How**: By comparing the state of data at different points or in different DB and applying only the changes needed to synchronize them.

### File Synchronization Tools
- **Examples**: rsync, Dropbox, Google Drive.
- **Purpose**: Efficiently synchronize files between devices or locations, often over a network.
- **How**: By comparing files or directories to detect changes, then transferring only the differing parts, rather than whole files, to minimize data transfer and speed up synchronization.

### Incremental Backup Systems
- **Purpose**: Efficiently create backups by storing only what has changed since the last backup, rather than backing up all data every time.
- **How**: Uses diffing algorithms to identify which files or data have changed since the last backup and saves only those changes.

### Software Patches and Updates
- **Purpose**: Update or fix software without requiring a full reinstallation.
- **How**: By applying a patch or update that contains only the changes from the previous version of the software, reducing download size and installation time.

### Data Compression
- **Purpose**: Reduce the size of data for storage or transmission.
- **How**: Some compression algorithms identify repetitive patterns or sequences in data (similar to finding differences) and represent them more compactly.

### Operational Transformation (OT) and Conflict-free Replicated Data Types (CRDTs)
- **Purpose**: Used in collaborative applications (like Google Docs) to allow multiple users to edit a document simultaneously without losing changes.
- **How**: By applying algorithms to manage and merge changes from different users in real-time, ensuring consistency across all users' views.