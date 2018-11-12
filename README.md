# 4d-component-build-app

Object-based replacement for BUILD APPLICATION

### Version

<img src="https://user-images.githubusercontent.com/1725068/41266195-ddf767b2-6e30-11e8-9d6b-2adf6a9f57a5.png" width="32" height="32" />

---

## Syntax

```
project:=BUILD_Get_option
BUILD_SET_OPTION(project)
```

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
