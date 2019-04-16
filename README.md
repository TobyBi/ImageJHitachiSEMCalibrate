# ImageJ Hitachi SEM Calibrate
An ImageJ macro script that applies proper scale calibration to images taken with Hitachi PC-SEM software, such as the Hitachi SU-70

Hitachi SU-70 Scanning Electron Microscopes (and likely other Hitachi models) use software called PC-SEM to acquire images.  The images are saved as TIFF files with an accompanying .txt file with same name that contains information about the image.

When doing image analysis work with ImageJ/Fiji, it is helpful to have the image properly calibrated so lengths and other
dimensions on the image are in the same units as the scale bars (usually micrometers or nanometers).  While you can do 
this manually from measuring the displayed scale bar on the image and using the "set scale" function, the Hitachi .txt
file already has the calibration factor in it, so this script just opens that file and applies the proper scale calibration. The accompanying .txt file must be in the same directory as the tiff file and have the same name for the script to find it (this is the default when saving images from PC-SEM).  

As written, it renames the image to a new image ("... calibrated.tiff") but does not automatically save it.  You can then
build this function into other batch operation macros.

This was originally written in 2009, and I don't have access to any newer versions of PC-SEM so as they say "your mileage may vary"
