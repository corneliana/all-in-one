**Manage and apply changes**
- **Distributed Architecture**: Unlike centralized VCS, Git is distributed. Each contributor has a full copy of the project history, enabling work without a central server with fault tolerance.
    
- **Snapshot-Based Storage**: Git stores data as snapshots of the project. Each commit captures a snapshot at a point in time. This contrasts with systems that track file differences.
    
- **Data Integrity**: Git uses SHA-1 hash to checksum files and commits, ensuring the integrity of the version history against corruption.
    
- **Branching and Merging**: Git's lightweight branching allows for easy and fast context switching, parallel workstreams, and isolated experiments. Merging integrates diverged branches.
    
- **Content Addressable Storage System**: Git’s objects (commits, trees, blobs) are stored in a way that's addressable by their content hash, facilitating quick access, integrity checks, and deduplication.

**Git** is the core technology, a tool for version control.
**GitHub** and **GitLab** are hosting platforms that offer Git repository management with additional features. GitHub focuses more on the social aspect and open-source collaboration, while GitLab positions itself as an integrated solution for the entire software development lifecycle.
