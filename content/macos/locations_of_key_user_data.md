---
Title: macOS - Locations of Key User Data
Description: A cheat sheet for locations of key user data related items.
Author: Jack Szwergold
Date: 2015-10-18
Robots: noindex,nofollow
Template: index
---

All of this stuff is useful for a user migration to a clean OS install.

### Mail files and preferences.

The actual mail files are located here:

    ~/Library/Mail/

And the mail application preferences are located here:

    ~/Library/Preferences/com.apple.mail.plist

### Safari bookmarks.

Your Safari bookmarks are here:

    ~/Library/Safari/Bookmarks.plist

### Addresses.

Export your address book like this:

    File -> Export -> Address Book Archive

Reimport them by double-clicking the file and having that previous export overwrite what you already have in place.

Or you can copy the full address book directory from here:

    ~/Library/Application Support/AddressBook

### iTunes playlists.

Export your iTunes playlists like this:

    File -> Library -> Export Playlist

The reimport the  playlists like this:

    File -> Library -> Import Playlist

Or you can copy the full iTunes music  directory from here:

    ~/Music/iTunes
