# Digital-Forensics

<b>Installing Autopsy on Mac</b>
</br>
	•	Press Command+Space and type Terminal and press enter/return key.</br>
	•	Run in Terminal app: ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" < /dev/null 2> /dev/null</br> 
	and press enter/return key.</br>
	•	Run: brew install autopsy</br>
</br>
Done! You can now use autopsy [1]</br>

Problems encountered during installation: None</br>

<b>Mounting a forensic image</b></br>
Problems encountered during image mounting:</br> 
I tried using the following commands:</br> 
	•	sudo mount -t ntfs-3g -o loop,ro,noexec,offset=32256 imageName.dd</br>
	•	mmls out.dd</br>
But it wasn’t compatible with macOS</br>

<b>Solution:</b>
</br>Run the following command</br>
	•	hdiutil attach -imagekey diskimage-class=CRawDiskImage -nomount imageName.dd</br>
You will get an output indicating the name of the disk being mounted</br>
	•	hdiutil mount DiskNameBeingMounted</br> 
e.g hdiutil mount /dev/disk2

<b>Installing exiftool on Mac</b></br>
	•	Press Command+Space and type Terminal and press enter/return key.</br>
	•	Run in Terminal app:</br>
$ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" < /dev/null 2> /dev/null</br>
and press enter/return key.</br>
	•	Run: brew install exiftool</br>

Done! You can now use exiftool [1]

Problems encountered during installation: None

<b>References</b></br>
	•	http://macappstore.org/



