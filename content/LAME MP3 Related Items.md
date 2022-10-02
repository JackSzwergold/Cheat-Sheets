---
Title: LAME MP3 Related Items
Description: A cheat sheet for Kernel Networking Tweaks related items.
Author: Jack Szwergold
Date: 2015-10-08
Robots: noindex,nofollow
Template: index
---

### Some nice LAME MP3 settings.

Good settings for music:

    --lowpass 19.7 -V 3 --vbr-new -q 0 -b 96 --scale 0.99 --athaa-sensitivity 1

Good settings for low fidelity mono and spoken word files:

    --resample 8 -V 3 --vbr-new -q 0 -B 16 --lowpass 15.4 --athaa-sensitivity 1
