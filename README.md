# How to set up debugging for your Outward Mod

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
* Install BepInEx
* Build the .dll into BepInEx\plugins\YourModFolder\yourMod.dll
* Download the pdb2mdb.exe from here: https://gist.github.com/jbevain/ba23149da8369e4a966f
* Create a .bat script in your project to run the tool on your .dll as follows:
```
@echo off
cd C:\Tools
pdb2mdb.exe "C:\Program Files (x86)\Steam\steamapps\common\Outward\Outward_Defed\BepInEx\plugins\YourModFolder\yourMod.dll"
```
* Add this to the end of your .csproj (before the project end tag)
```
	<Target Name="PostBuild" AfterTargets="PostBuildEvent">
	  <Exec Command="call create_mdb.bat" />
	</Target>
```
* Rebuild your mod: yourMod.mdb should now appear next to your .dll
* Run Outward and in Visual Studio select Debug > Attach Unity Debugger
 * Sometimes you can see a fakeout process first, wait a tiny bit.
 * If you need to debug startup code its a good idea to add a little Thread.Sleep() to give you more time to attach the debugger.
