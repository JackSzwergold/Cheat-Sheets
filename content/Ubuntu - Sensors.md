---
Title: Ubuntu - Sensors
Description: Ubuntu - Sensors
Author: Jack Szwergold
Date: 2015-09-22
Robots: noindex,nofollow
Template: index
---

Install `lm-sensors`:

    sudo aptitude install lm-sensors

Configure with `sensors-detect`:

    sudo sensors-detect

Then view the output with the `sensors` command like this:

    sensors
