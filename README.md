# 4d-component-build-app

Object-based replacement for BUILD APPLICATION

### Version

<img src="https://user-images.githubusercontent.com/1725068/41266195-ddf767b2-6e30-11e8-9d6b-2adf6a9f57a5.png" width="32" height="32" />

## Examples

```
$BuildApp:=BUILD_Get_option 

  //bundle identifier is automatically set to 4d.com.{name}.app

$BuildApp.BuildApplicationName:=Path to object(Structure file).name
$BuildApp.BuildCompiled:=False
$BuildApp.IncludeAssociatedFolders:=False
$BuildApp.BuildComponent:=False
$BuildApp.BuildApplicationLight:=False
$BuildApp.BuildApplicationSerialized:=True

$BuildApp.BuildMacDestFolder:=System folder(Desktop)+"DEMO"+Folder separator
$BuildApp.BuildWinDestFolder:=System folder(Desktop)+"DEMO"+Folder separator

$BuildApp.SourcesFiles.RuntimeVL.RuntimeVLIncludeIt:=True
$BuildApp.SourcesFiles.RuntimeVL.RuntimeVLMacFolder:=System folder(Applications or program files)+"4D"+Folder separator+"4D v17 R2"+Folder separator+"4D Volume Desktop.app"+Folder separator
$BuildApp.SourcesFiles.RuntimeVL.RuntimeVLWinFolder:=\
Replace string(System folder(Applications or program files);" (x86)";"";*)+\
"4D"+Folder separator+"4D v17 R2"+Folder separator+"4D Volume Desktop"+Folder separator
$BuildApp.SourcesFiles.RuntimeVL.IsOEM:=True

$BuildApp.SourcesFiles.CS.ServerIncludeIt:=False
$BuildApp.SourcesFiles.CS.ClientMacIncludeIt:=False
$BuildApp.SourcesFiles.CS.ClientWinIncludeIt:=False
$BuildApp.SourcesFiles.CS.ServerMacFolder:=Null
$BuildApp.SourcesFiles.CS.ServerWinFolder:=Null
$BuildApp.SourcesFiles.CS.ClientWinFolderToWin:=Null
$BuildApp.SourcesFiles.CS.ClientWinFolderToMac:=Null
$BuildApp.SourcesFiles.CS.ClientMacFolderToWin:=Null
$BuildApp.SourcesFiles.CS.ClientMacFolderToMac:=Null
$BuildApp.SourcesFiles.CS.ServerIconWinPath:=Null
$BuildApp.SourcesFiles.CS.ServerIconMacPath:=Null
$BuildApp.SourcesFiles.CS.ClientMacIconForMacPath:=Null
$BuildApp.SourcesFiles.CS.ClientWinIconForMacPath:=Null
$BuildApp.SourcesFiles.CS.ClientMacIconForWinPath:=Null
$BuildApp.SourcesFiles.CS.ClientWinIconForWinPath:=Null
$BuildApp.SourcesFiles.CS.IsOEM:=False

$BuildApp.CS.BuildServerApplication:=False
$BuildApp.CS.BuildCSUpgradeable:=False
$BuildApp.CS.BuildV13ClientUpgrades:=False
$BuildApp.CS.ServerSelectionAllowed:=False
$BuildApp.CS.LastDataPathLookup:="ByAppName"
$BuildApp.CS.CurrentVers:=1
$BuildApp.CS.HardLink:=Null
$BuildApp.CS.IPAddress:=Null
$BuildApp.CS.PortNumber:=Null
$BuildApp.CS.RangeVersMin:=1  //can not be zero
$BuildApp.CS.RangeVersMax:=MAXINT

$BuildApp.ArrayExcludedPluginName.ItemsCount:=1
$BuildApp.ArrayExcludedPluginName.Item[0]:="4D Internet Commands"

$BuildApp.ArrayExcludedPluginID.ItemsCount:=1
$BuildApp.ArrayExcludedPluginID.Item[0]:=15010

$BuildApp.ArrayExcludedComponentName.ItemsCount:=5
$BuildApp.ArrayExcludedComponentName.Item[0]:="4D Progress"
$BuildApp.ArrayExcludedComponentName.Item[1]:="4D SVG"
$BuildApp.ArrayExcludedComponentName.Item[2]:="4D ViewPro"
$BuildApp.ArrayExcludedComponentName.Item[3]:="4D Widgets"
$BuildApp.ArrayExcludedComponentName.Item[4]:="4D WritePro Interface"

$BuildApp.Licenses.ArrayLicenseMac.ItemsCount:=2
$BuildApp.Licenses.ArrayLicenseMac.Item[0]:=Folder separator+"Licenses"+Folder separator+"MacOS"+Folder separator+"R-4DOE170XXXXXXXXXXXXXXXX.license4D"
$BuildApp.Licenses.ArrayLicenseMac.Item[1]:=Folder separator+"Licenses"+Folder separator+"MacOS"+Folder separator+"R-4UOE170XXXXXXXXXXXXXXXX.license4D"

$BuildApp.Licenses.ArrayLicenseWin.ItemsCount:=2
$BuildApp.Licenses.ArrayLicenseWin.Item[0]:=Folder separator+"Licenses"+Folder separator+"Windows"+Folder separator+"R-4DOE170XXXXXXXXXXXXXXXX.license4D"
$BuildApp.Licenses.ArrayLicenseWin.Item[1]:=Folder separator+"Licenses"+Folder separator+"Windows"+Folder separator+"R-4UOE170XXXXXXXXXXXXXXXX.license4D"

$BuildApp.SignApplication.MacCertificate:="Developer ID Application: keisuke miyako (Y69CWUC25B)"
$BuildApp.SignApplication.MacSignature:=False

$BuildApp.Versioning.RuntimeVL.RuntimeVLVersion:="1.0"
$BuildApp.Versioning.RuntimeVL.RuntimeVLCopyright:="DEMO"
$BuildApp.Versioning.RuntimeVL.RuntimeVLCreator:="DEMO"
$BuildApp.Versioning.RuntimeVL.RuntimeVLComment:="DEMO"
$BuildApp.Versioning.RuntimeVL.RuntimeVLCompanyName:="DEMO"
$BuildApp.Versioning.RuntimeVL.RuntimeVLFileDescription:="DEMO"
$BuildApp.Versioning.RuntimeVL.RuntimeVLInternalName:="DEMO"
$BuildApp.Versioning.RuntimeVL.RuntimeVLLegalTrademark:="DEMO"
$BuildApp.Versioning.RuntimeVL.RuntimeVLPrivateBuild:="1.0"
$BuildApp.Versioning.RuntimeVL.RuntimeVLSpecialBuild:="1.0"

BUILD_SET_OPTION ($BuildApp)

$Log:=BUILD_APPLICATION 

If ($Log.OK=1)
	
	If ($BuildApp.SignApplication.MacSignature=False) | Is Windows
		
		$imagePath:=Get 4D folder(Current resources folder)+"templates"+Folder separator+"application.png"
		BUILD_SET_SPLASH ($BuildApp;$imagePath)  //the default image is visibile for a short time...
		
	End if 
	
	If (Is macOS)
		$path:=$BuildApp.BuildMacDestFolder
	Else 
		$path:=$BuildApp.BuildWinDestFolder
	End if 
	$path:=$path+"Final Application"+Folder separator
	
	SHOW ON DISK($path;*)
	
End if 
```

---

## Syntax

```
project:=BUILD_Get_option
BUILD_SET_OPTION(project)
```

Parameter|Type|Description
------------|------------|----
project|TEXT|``JSON``

converts the default build application project ``xml`` file to ``json`` object.

```
BUILD_SET_SPLASH(project;splash)
```

Parameter|Type|Description
------------|------------|----
project|TEXT|``JSON``
splash|PICTURE|picture to display in the splash screen (because, even if defined in toobar/menu, the default "4D" logo is displayed for a brief moment. this command will replace the default splash picture file inside the application)

```
log:=BUILD_APPLICATION
```

Parameter|Type|Description
------------|------------|----
log|TEXT|``JSON`` properties are ``OK`` (numeric), ``log`` (collection) and ``path``) each item in ``log`` has ``messageType``, ``target``, ``codeDesc``, ``CodeId``, and ``message`` from the build application log file.

```
C_OBJECT($0;$result)

$result:=New object("OK";0;"log";New collection;"path";Null)

$path:=Path to object(Path to object(Get 4D file(Backup configuration file;*)).parentFolder).parentFolder+"BuildApp"+Folder separator+"BuildApp.xml"

If (Test path name($path)=Is a document)
	
	BUILD APPLICATION($path)
	
	$result.OK:=OK
	
	$logPath:=Get 4D file(Build application log file;*)
	
	If (Test path name($logPath)=Is a document)
		
		$dom:=DOM Parse XML source($logPath)
		
		If (OK=1)
			
			$BuildApplicationLog:=DOM Find XML element($dom;"BuildApplicationLog")
			
			ARRAY TEXT($Logs;0)
			
			$log:=DOM Find XML element($BuildApplicationLog;"BuildApplicationLog/log";$Logs)
			
			C_TEXT($MessageType;$Target;$CodeDesc;$Message)
			C_LONGINT($CodeId)
			
			For ($i;1;Size of array($Logs))
				
				$log:=$Logs{$i}
				
				DOM GET XML ELEMENT VALUE(DOM Find XML element($log;"log/MessageType");$MessageType)
				DOM GET XML ELEMENT VALUE(DOM Find XML element($log;"log/Target");$Target)
				DOM GET XML ELEMENT VALUE(DOM Find XML element($log;"log/CodeDesc");$CodeDesc)
				DOM GET XML ELEMENT VALUE(DOM Find XML element($log;"log/CodeId");$CodeId)
				DOM GET XML ELEMENT VALUE(DOM Find XML element($log;"log/Message");$Message)
				
				$result.log[$i-1]:=New object(\
				"messageType";$MessageType;\
				"target";$Target;\
				"codeDesc";$CodeDesc;\
				"codeId";$CodeId;\
				"message";$Message)
				
			End for 
			
			DOM CLOSE XML($dom)
			
		End if 
		
	End if 
	
End if 

$0:=$result
```
