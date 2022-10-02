---
Title: Rsync
Description: A cheat sheet for Rsync related items.
Author: Jack Szwergold
Date: 2015-09-17
Robots: noindex,nofollow
Template: index
---

### Basic Rync command to clone directories.

    rsync -avz --dry-run /source_directory/ /destination_directory/

To run the same command on a macOS machine—and retain extended attributes:

	rsync -avzE --dry-run /source_directory/ /destination_directory/

### Command to retain partially transfered files if the connection is lost.

While this works, sometimes the partially transferred files are not cleanly resumed. Should check why that happens and if anything can be done:

    rsync -rPtz /source_directory/ /destination_directory/

- **-r**: Recurse into directories.
- **-z**: Compress file data during the transfer.
- **-t**: Preserve times.
- **-P**: Keep partially transferred files and show transfer progress.

### Compare two directories with Rsync.

A nice way to effectively “diff” two directories using Rsync:

    rsync -rvnc --delete /first_directory_to_check/ /first_directory_to_check/

- **-r**: Recurse into directories,
- **-v**: Increase verbosity.
- **-n**: Dry-run mode; show what would have been transferred.
- **-c**: Checksum the files, not mod-time & size.
