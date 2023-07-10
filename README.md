# How to set up debugging for your Outward Mod

* Turn Outward into a Debug-Enabled Dev-Build
	* First Method
		* Download unity Editor Version 2020.3.26f1
		* Go into Editor\Data\PlaybackEngines\windowsstandalonesupport\Variations\win64_development_mono and copy the UnityPlayer.dll and the MonoBleedingEdge Folder into the Outward_Defed Folder.
		* Add the following to the boot.config file in the "Outward Definitive Edition_Data" Folder (not sure if all are needed):
		```
		wait-for-managed-debugger=1
		player-connection-debug=1
		```
	* Second Method
		* Download "Extract to Outward_Defed.zip" from this repository
		* Unzip it into the "Outward_Defed" Directory and overwrite all files
* Download the pdb2mdb.exe from this repository or from [here](https://www.nuget.org/packages/Mono.Unofficial.pdb2mdb) <-- This one is more up-to-date
* Install BepInEx
* Modify your project file (MyMod.csproj)
	* Add these 2 variables into your PropertyGroup. The path has to point to the directory containing the exe.
	```
	<MdbToolPath>C:\Tools</MdbToolPath>
	<MdbToolExe>pdb2mdb.exe</MdbToolExe>
	```
	* Add a PostBuild Target at the bottom of your Project to call the tool after every build.
	```
	<Target Name="PostBuild" AfterTargets="PostBuildEvent">
		<Exec WorkingDirectory="$(MdbToolPath)" Command="$(MdbToolExe) &quot;$(TargetPath)&quot;" />
	</Target>
	```
* Rebuild your mod: yourMod.mdb should now appear next to your .dll
* Run Outward
	* Outward should now show a popup that says you can attach the debugger.
	* In Visual Studio select Debug > Attach Unity Debugger

Happy debugging :)
