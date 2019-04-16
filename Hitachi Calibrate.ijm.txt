// This ImageJ macro will read in the Hitachi .txt info file for a given image
// and parse it to determine the pixel scale, and set the scale accordingly. 
//
// images must be named same as info file, and be in the directory together:
//    My Cool image.tif
//    My Cool image.txt   <-- info file
// By default hitachi PC-SEM makes them this way. 
// 
// After running, the macro changes the name to "My Cool image calibrated.tif", so you can re-save it
// with the calibration preserved.
//
// Steven Cogswell, P.Eng.
// Microscopy and Microanalysis Facility
// University of New Brunswick
// 


//getImageInfo(); 
// Get the directory the active image came from
path = getDirectory("image"); 
imageTitle=getTitle();  // The image title has the name
checkTif = indexOf(imageTitle,".tif");   
imageName=substring(imageTitle,0,checkTif);   // Name without .tif on the end
newTitle=imageName+" calibrated.tif";   // The new image name, if successful (changed later)
hitachiFile = substring(imageTitle,0,checkTif)+".txt";  // Hitachi file ending in .txt
hitachiPath = path+File.separator+hitachiFile;   // Path to Hitachi file

exists=File.exists(hitachiPath);   // Check if hitachi file exists or not
if (exists==1) {
	// All is ok, we can continue
	//print("Hitachi info file ",hitachiFile," exists"); 
} else {
	// Couldn't find the hitachi info file.  Better stop before something bad happens. 
	showMessage("No calibration file","Couldn't find Hitachi info file "+hitachiFile); 
	exit(); 
}

// Find the calibration in the file
target = "PixelSize=";   // This is in nm in the Hitachi file, it seems
lines=split(File.openAsString(hitachiPath),"\n");   // Opens entire .txt file into a string array, ugh but what ImageJ does. 
for (i=1; i<lines.length; i++) {
	if (startsWith(lines[i],target)) {  // If this matches the target
		//print(lines[i]); 
		hitachiScale=substring(lines[i],lengthOf(target));
		if (isNaN(parseFloat(hitachiScale))) {
			showMessage("Error parsing pixel size","Error parsing the pixelsize ",hitachiScale);
			exit();
		}
		
		hitachiScaleUM=parseFloat(hitachiScale)/1000;   // Convert from nm to um in scale
		//print("scale ",hitachiScale," nm per pixel"); 
		// Set the actual scale
		run("Set Scale...", "distance=1 known="+hitachiScaleUM+" pixel=1 unit=um");
		rename(newTitle);   // Rename the file "... calibrated.tif", does not save automatically. 
	}
}
