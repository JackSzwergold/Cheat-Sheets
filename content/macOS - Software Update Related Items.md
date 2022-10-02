---
Title: macOS - Software Update Related Items
Description: A cheat sheet for software update related items.
Author: Jack Szwergold
Date: 2015-10-06
Robots: noindex,nofollow
Template: index
---

Setting the software updates server from the command line:

	sudo defaults write /Library/Preferences/com.apple.SoftwareUpdate CatalogURL "http://example.local:8088/index.sucatalog"

Shows the software updates settings:

	defaults read /Library/Preferences/com.apple.SoftwareUpdate

Resets the catalog URL to point to Apple:

	sudo defaults delete /Library/Preferences/com.apple.SoftwareUpdate CatalogURL
