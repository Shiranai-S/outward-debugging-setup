# How to set up debugging for your Outward Mod

* Turn Outward into a Debug-Enabled Dev-Build
	* First Method
		* Download unity Editor Version 2020.3.26f1
		* Go into Editor\Data\PlaybackEngines\windowsstandalonesupport\Variations\win64_development_mono and copy the UnityPlayer.dll and the MonoBleedingEdge Folder into the Outward_Defed Folder.
		* Add the following to the boot.config file in the "Outward Definitive Edition_Data" Folder (not sure if all are needed):
		```
		player-connection-mode=Listen
		player-connection-guid=3060108046
		player-connection-debug=1
		player-connection-wait-timeout=-1
		player-connection-ip=127.0.0.1:55555
		```
	* Second Method
		* Download "Extract to Outward_Defed.zip" from this repository
		* Unzip it into the "Outward_Defed" Directory and overwrite all files
* Download the pdb2mdb.exe from this repository or from [here](https://gist.github.com/jbevain/ba23149da8369e4a966f)
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
		<Exec WorkingDirectory="$(MdbToolPath)" Command="$(MdbToolExe) &quot;$(OutputPath)$(AssemblyName).dll&quot;" />
	</Target>
	```
* Rebuild your mod: yourMod.mdb should now appear next to your .dll
* Run Outward and in Visual Studio select Debug > Attach Unity Debugger
	* If you just started Outward then you might see an open Unity-Debugger port without a PID. You can ignore this.
	* If you need to debug startup code its a good idea to add a little Thread.Sleep() to give you more time to attach the debugger.
 		* The opening time of the Debugger-Port seems to be inconsistent enough to not always work for startup code. You can add Thread.Sleep(10000) to the wakeup-code of your mod to give you time to attach the debugger. Restart the game if the debugger does not show up after the window opens and you want to debug startup code.
