---
Title: Ghostscript
Description: A cheat sheet for Ghostscript related items.
Author: Jack Szwergold
Date: 2022-01-29
Robots: noindex,nofollow
Template: index
---

### Convert a directory of EPS files into a PDF.

Be sure GhostScript is installed, maneuver into the directory that contains the EPS files and run this command. The output will be saved to a PDF named `output.pdf`.


    gs \
      -sDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER \
      -dFIXEDMEDIA -dEPSFitPage \
      -dPDFSETTINGS=/printer \
      -dCompatibilityLevel=1.3 \
      -dEmbedAllFonts=true -dSubsetFonts=true \
      -dColorImageDownsampleType=/Bicubic \
      -dColorImageResolution=144 \
      -dGrayImageDownsampleType=/Bicubic \
      -dGrayImageResolution=144 \
      -dMonoImageDownsampleType=/Bicubic \
      -dMonoImageResolution=144 \
      -sOutputFile=output.pdf \
    *.eps
