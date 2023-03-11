# Windows Disk Encryption Policy

## Use Case

This is an updated version of the ProServe Policy Template for assessing Disk Encryption on Windows Devices.

We have noticed recently that the built in script identifying BitLocker Encryption is occasionally having mixed results in the Encryption status of Windows machines. 

Through some testing with the following Powershell script we found the same error occasionally. 

	$drive = "C:"
	$encryptionStatus = Get-BitLockerVolume -MountPoint $drive | Select-Object -ExpandProperty EncryptionStatus

The following Command Line script ended up giving us the correct results and this was merged into the policy template using the condition "Windows Expected Script Results" running the following command and looking that the results contains "Fully Encrypted" or "Fully Decrypted". 

  manage-bde -status
  
  Example Results:
    
	C:\WINDOWS\system32>manage-bde -status
	BitLocker Drive Encryption: Configuration Tool version 10.0.19041
	Copyright (C) 2013 Microsoft Corporation. All rights reserved.

	Disk volumes that can be protected with
	BitLocker Drive Encryption:
	Volume C: []
	[OS Volume]

	    Size:                 78.68 GB
	    BitLocker Version:    None
	    Conversion Status:    Fully Decrypted
	    Percentage Encrypted: 0.0%
	    Encryption Method:    None
	    Protection Status:    Protection Off
	    Lock Status:          Unlocked
	    Identification Field: None
	    Key Protectors:       None Found

In additon, on the Sub-Rule detecting if the Encryption Software was Installed but NOT Encrypted, we made this an OR statement and added the condition to validate just if BitLocker was installed. 


## Needed for the Policy

Copy the XML code and paste into a text editor and save as a .xml file.

Then import into your policy template. 

You will need to update the Segments to point to your Segment Name.
