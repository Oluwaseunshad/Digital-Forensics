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



<b>Differences between diskutil unmount and unmountDisk</b></br>

From man diskutil:</br>
 unmount | umount [force] device</br>
     Unmount a single volume.  Force will force-unmount the volume (less kind to any open files; see also umount (8)).</br>
 /br>unmountDisk | umountDisk [force] device</br>
     Unmount an entire disk (all volumes).  Force will force-unmount the volumes (less kind to any open files; see also umount (8)).  You should specify a whole disk, but all volumes of the whole disk are attempted to be unmounted even if you specify a partition.</br>
So diskutil unmount just ejects a single volume/partition, diskutil unmountDisk the whole disk (including all volumes/partitions).</br>
</br>
</br>
To enter Recovery mode, Press keys Command + R immediately after restarting the system</br>
When booted to OS X Recovery the root of the Macintosh HD is not /, it's:  /Volumes/Macintosh HD/</br>
To use dd in Terminal, use:</br>
	"/Volumes/Macintosh HD/bin/dd”</br>
 </br>
When booting into the Recovery Partition, the Macintosh HD does get mounted automatically and should be listed in /Volumes. Use ls /Volumes to get a list of volumes and ls /dev to get a list of devices that are mounted.</br>
</br>


<b>Commands to copy a Partitioned Image</b></br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ diskutil list</br>
/dev/disk0 (internal, physical):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:      GUID_partition_scheme                        *121.3 GB   disk0</br>
   1:                        EFI EFI                     209.7 MB   disk0s1</br>
   2:                 Apple_APFS Container disk1         121.1 GB   disk0s2</br>
</br>
/dev/disk1 (synthesized):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:      APFS Container Scheme -                      +121.1 GB   disk1</br>
                                 Physical Store disk0s2</br>
   1:                APFS Volume MACINTOSH HD            98.0 GB    disk1s1</br>
   2:                APFS Volume Preboot                 22.0 MB    disk1s2</br>
   3:                APFS Volume Recovery                518.8 MB   disk1s3</br>
   4:                APFS Volume VM                      8.1 GB     disk1s4</br>
</br>
/dev/disk2 (disk image):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:     Apple_partition_scheme                        +24.3 MB    disk2</br>
   1:        Apple_partition_map                         32.3 KB    disk2s1</br>
   2:                  Apple_HFS Flash Player            24.2 MB    disk2s2</br>
</br>
/dev/disk3 (external, physical):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:     FDisk_partition_scheme                        *251.7 MB   disk3</br>
   1:               Windows_NTFS SHAD                    137.3 MB   disk3s1</br>
   2:             Windows_FAT_16 NEW VOLUME              113.2 MB   disk3s2</br>
</br>
/dev/disk4 (external, physical):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:     FDisk_partition_scheme                        *500.1 GB   disk4</br>
   1:               Windows_NTFS SHAD                    500.1 GB   disk4s1</br>
   2:                      Linux                         2.1 MB     disk4s2</br>
</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ ls /Volumes</br>
MACINTOSH HD	NEW VOLUME	Preboot		SHAD		SHAD 1</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ cd /Volumes/SHAD</br>
Oluwaseuns-MacBook-Air:SHAD oluwaseunshad$ ls</br>
System Volume Information</br>
Oluwaseuns-MacBook-Air:SHAD oluwaseunshad$ cd ..</br>
Oluwaseuns-MacBook-Air:Volumes oluwaseunshad$ ls SHAD\ 1</br>
System Volume Information	backup_image.img.gz</br>
Oluwaseuns-MacBook-Air:Volumes oluwaseunshad$ cd ~</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ dd if=/dev/disk3s2 of=/Volumes/SHAD\ 1/partitionImage.dd</br>
dd: /dev/disk3s2: Permission denied</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ sudo dd if=/dev/disk3s2 of=/Volumes/SHAD\ 1/partitionImage.dd</br>
Password:</br>
dd: /dev/disk3s2: Resource busy</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ hdutil unmount /Volumes/SHAD\ 1</br>
-bash: hdutil: command not found</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ hdiutil unmount /Volumes/SHAD\ 1</br>
hdiutil: unmount: "/Volumes/SHAD 1" failed to unmount due to error 2.</br>
hdiutil: unmount failed - No such file or directory</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ ls /Volumes/</br>
MACINTOSH HD	NEW VOLUME	Preboot		SHAD</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ dd if=/dev/disk3s2 of=/Volumes/SHAD\ 1/partitionImage.dd</br>
dd: /dev/disk3s2: Permission denied</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ sudo dd if=/dev/disk3s2 of=/Volumes/SHAD\ 1/partitionImage.dd</br>
dd: /dev/disk3s2: Resource busy</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ sudo diskutil unmount /dev/disk3s2</br>
Volume NEW VOLUME on disk3s2 unmounted</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ sudo dd if=/dev/disk3s2 of=/Volumes/SHAD\ 1/partitionImage.dd</br>
dd: /Volumes/SHAD 1/partitionImage.dd: No such file or directory</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ hdutil mount /Volumes/SHAD\ 1</br>
-bash: hdutil: command not found</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ hdiutil mount /Volumes/SHAD\ 1</br>
hdiutil: mount failed - No such file or directory</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ diskutil mount /Volumes/SHAD\ 1</br>
Unable to find disk for /Volumes/SHAD 1</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ sudo dd if=/dev/disk3s2 of=/Volumes/SHAD\ 1/partitionImage.dd</br>
dd: /Volumes/SHAD 1/partitionImage.dd: Read-only file system</br>
Oluwaseuns-MacBook-Air:~ oluwaseunshad$ sudo dd if=/dev/disk3s2 of=/Volumes/SHAD\ 1/partitionImage.dd bs=512</br>
221184+0 records in</br>
221184+0 records out</br>
113246208 bytes transferred in 19.627999 secs (5769626 bytes/sec)</br>
</br>
</br>

<b>Commands to make a bootable Kali Linux USB drive</b></br>

Oluwaseuns-Air:~ oluwaseunshad$ diskutil list</br>
/dev/disk0 (internal, physical):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:      GUID_partition_scheme                        *121.3 GB   disk0</br>
   1:                        EFI EFI                     209.7 MB   disk0s1</br>
   2:                 Apple_APFS Container disk1         121.1 GB   disk0s2</br>
</br>
/dev/disk1 (synthesized):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:      APFS Container Scheme -                      +121.1 GB   disk1</br>
                                 Physical Store disk0s2</br>
   1:                APFS Volume MACINTOSH HD            98.6 GB    disk1s1</br>
   2:                APFS Volume Preboot                 22.0 MB    disk1s2</br>
   3:                APFS Volume Recovery                518.8 MB   disk1s3</br>
   4:                APFS Volume VM                      5.4 GB     disk1s4</br>
</br>
/dev/disk3 (external, physical):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:     FDisk_partition_scheme                        *500.1 GB   disk3</br>
   1:               Windows_NTFS SHAD                    500.1 GB   disk3s1</br>
   2:                      Linux                         2.1 MB     disk3s2</br>
</br>
Oluwaseuns-Air:~ oluwaseunshad$ diskutil list</br>
/dev/disk0 (internal, physical):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:      GUID_partition_scheme                        *121.3 GB   disk0</br>
   1:                        EFI EFI                     209.7 MB   disk0s1</br>
   2:                 Apple_APFS Container disk1         121.1 GB   disk0s2</br>
</br>
/dev/disk1 (synthesized):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:      APFS Container Scheme -                      +121.1 GB   disk1</br>
                                 Physical Store disk0s2</br>
   1:                APFS Volume MACINTOSH HD            98.6 GB    disk1s1</br>
   2:                APFS Volume Preboot                 22.0 MB    disk1s2</br>
   3:                APFS Volume Recovery                518.8 MB   disk1s3</br>
   4:                APFS Volume VM                      5.4 GB     disk1s4</br>
</br>
/dev/disk2 (external, physical):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:     FDisk_partition_scheme                        *15.7 GB    disk2</br>
   1:             Windows_FAT_32 NO NAME                 2.9 GB     disk2s1</br>
   2:             Windows_FAT_16 NO NAME                 720.9 KB   disk2s2</br>
</br>
/dev/disk3 (external, physical):</br>
   #:                       TYPE NAME                    SIZE       IDENTIFIER</br>
   0:     FDisk_partition_scheme                        *500.1 GB   disk3</br>
   1:               Windows_NTFS SHAD                    500.1 GB   disk3s1</br>
   2:                      Linux                         2.1 MB     disk3s2</br>
</br>
Oluwaseuns-Air:~ oluwaseunshad$ diskutil unmount /dev/disk2</br>
disk2 was already unmounted or it has a partitioning scheme so use "diskutil unmountDisk" instead</br>
Oluwaseuns-Air:~ oluwaseunshad$ diskutil unmountDisk /dev/disk2</br>
Unmount of all volumes on disk2 was successful</br>
Oluwaseuns-Air:~ oluwaseunshad$ sudo dd if=/Users/oluwaseunshad/Downloads/kali-linux-2018.2-amd64.iso of=/dev/disk2 bs=1m</br>
Password:</br>
2800+1 records in</br>
2800+1 records out</br>
2936733696 bytes transferred in 1438.559749 secs (2041440 bytes/sec)</br>
</br>

<b>Acquiring Data with dd, dcfldd, dc3dd</b></br>
<b>Acquiring Data with dd in Linux</b></br>
dd stands for “data dump” and is available on all UNIX and Linux distributions. dd can create a bit-by-bit copy of a physical drive without mounting the drive first. This RAW image ca be read by most of the forensics tools currently on the market. A few shortcomings of the dd command are: no support of decompression, no additional metadata can be saved.</br>
An example of the dd command is shown here:</br>
	dd if=/dev/sdb of=sdb_image.img bs=65536 conv=noerror,sync</br>
Explanation of the parameters:</br>
if             => input file</br>
/dev/sdb       => source /suspect drive (whole disk)</br>
of             => output file</br>
sdb_image.img  => name of the image file</br>
bs             => block size (default is 512)</br>
65536          => 64k</br>
conv           => conversion</br>
noerror        => will continue even with read errors</br>
sync           => if there is an error, null fill the rest of the block.</br>

<b>dcfldd</b></br>
dd in general is a data management tool and was not particularly built for forensics purposes. Therefore it has its shortcomings.</br>
Nicholas Harbour of the Defense Computer Forensics Laboratory (DCFL) developed a tool  that works very similar than dd but adds many features to support forensics data acquisition. dcfldd offers the following options:</br>
•	Log errors to an output file for analysis and review</br>
•	Various hashing options MD5, SHA-1, SHA-256, etc</br>
•	Indicating the acquisition progress</br>
•	Split image file into segmented volumes</br>
•	Verify acquired data with the original source</br>

An example of the dcfldd command is shown here:</br>
	dcfldd if=/dev/sdb of=sdb_image.img</br>
Explanation of the parameters:</br>
if             => input file</br>
/dev/sdb       => source /suspect drive (whole disk)</br>
of             => output file</br>
sdb_image.img  => name of the image file</br>
If you need to split the image file in smaller chunks and hash the image at the and:</br>
	dcfldd if=/dev/sdb split=2M of=sdb_image.img hash=md5</br>
A more advanced dcfldd command could look like:</br>
dcfldd if=/dev/sdb hash=md5,sha256 hashwindow=2G md5log=md5.txt sha256log=sha256.txt \ hashconv=after bs=512 conv=noerror,sync split=2G splitformat=aa of=sdb_image.img</br>
Explanation of the parameters:</br>
if             => input file</br>
/dev/sdb       => source /suspect drive (whole disk)</br>
hash           => Definition of hash algorithms</br>
hashwindows    => Will hash data chunks of 2 GB</br>
md5log         => Saves all md5 hashes in a file called md5.txt</br>
sha256log      => Saves all sha hashes in a file called sha256.txt</br>
hashconv       => Hashing AFTER or BEFORE the conversion</br>
bs             => block size (default is 512)</br>
512             => block size of 512 kilobyte</br>
conv           => conversion</br>
noerror        => will continue even with read errors</br>
sync           => if there is an error, NULL fill the rest of the block</br>
split          => Split image file in chunks of 2 GB</br>
splitformat    => the file extension format for split operation</br>
of             => output file sdb_image.img => name of the image file</br>
This command will read two Gigabytes from the source drive and write that to a file called sdb_image.img.aa. It will also calculate the MD5 hash and the sha256 hash of the two Gigabyte chunk. It will then read the next two gigs and name that sdb_image.img.ab. The md5 hashes will be stored in a file called md5.txt and the sha256 hashes will be stored in a file called sha256.txt. The block size for transferring has been set to 512 bytes, and in the event of read errors, dcfldd will write zeros.</br>

Cautions, this tool is not suitable for imaging faulty drives:</br>
dcfldd is based on an extremely old version of dd: it’s known that dcfldd will misalign the data in the image after a faulty sector is encountered on the source drive (see the NIST report), and this kind of bug (wrong offset calculation when seeking over a bad block) was fixed for dd in 2003 (see the fix in the mailing list);</br>
Similarly, dcfldd can enter an infinite loop when a faulty sector is encountered on the source drive, thus writing to the image over and over again until there is no free space left (source: forensicswiki).</br>


<b>Dc3dd</b></br>
dc3dd is a patched version of GNU dd with added features for computer forensics. It was developed at the DoD Cyber Crime Center by Jesse Kornblum. The first release, corresponding to Coreutils version 6.9.91, was published on 1 Feb 2008.</br>

Comparison to GNU dd</br>
The following features are available in dc3dd that are not found in GNU dd:</br>
•	On the fly hashing with multiple algorithms (MD5, SHA-1, SHA-256, and SHA-512) with variable sized piecewise hashing</br>
•	Able to write errors directly to a file</br>
•	Combined error log. Groups errors together (e.g. Had 1,023 'Input/ouput errors' between blocks 17-233' )</br>
•	Pattern wiping. Wipe output files with a single hex digit or a text pattern</br>
•	Verify mode</br>
•	Progress reports. See the progress of the operation while it's running</br>
•	Split output. Able to split output files into fixed size chunks</br>
The following changes to GNU dd's behavior were made:</br>
•	On a partial read, the whole block is wiped with zeros. This allows for repeatable reads/hashes of a drive with errors.</br>
Comparison to dcfldd</br>
Although there are definitely similarities between dc3dd and dcfldd, the programs are based on slightly different code bases and have different feature sets. dcfldd is a fork of GNU dd, whereas dc3dd is a patch to the current version. This means that dc3dd will be updated every time GNU dd is updated, whereas dcfldd has its own release schedule. Certain features added to GNU dd after dcfldd forked, such as direct input/output mode, are not found in dcfldd.</br>
On the other hand, dcfldd supports more hashing algorithms than dc3dd, allows the user greater control over how hashes are displayed, supports wiping output files with random patterns, and is supported on the Cygwin platform.</br>

An example of a dc3dd command is shown here:</br>
	dc3dd if=/dev/sdb of=sdb_image.img bs=4k hash=md5 log=dc3dd.log progress=on split=2G splitformat=000</br>
Explanation of the parameters:</br>
if             => input file</br>
/dev/sdb       => source /suspect drive (whole disk)</br>
of             => output file</br>
bs             => blocksize of 4 kb</br>
sdb_image.img  => name of the image file</br>
hash           => Definition of hash algorithms</br>
log            => Path of the log file</br>
progress       => on; see progress of acquisition</br>
split          => Split image file in chunks of 2 GB</br>
splitformat    => Will append a number or letter at the end of the image file name</br>

DC3DD offers the best of DD with more features, including:</br>
•	On-the-fly hashing using more algorithm choices (MD5, SHA-1, SHA-256, and SHA-512)</br>
•	A meter to monitor progress and acquisition time</br>
•	Writing of errors to a file</br>
•	Splitting of output files</br>
•	Verification of files</br>
•	Wiping of output files (pattern wiping)</br>


