# Toonboom-Harmony-Multi-Instance-Launcher
By default on macOS Toonboom Harmony doesn't launch multiple instances when opening multiple scene files from finder.

For instance, if you need to open two projects from the finder, when you double click on the second one, it will close the first one and open the second one in the same window. 

You would need to manually launch Harmony, and then open the scene from the "Open File" dialog. Nobody has time for that!

So lets fix this:

1. Launch Automator
2. Create a New "Application" document
3. Add a "Run AppleScript" action
4. Replace the default code with this AppleScript:
``` applescript
on run input
	if input is not {} then
		repeat with f in input
			try
				set filePath to POSIX path of (f as alias)
				do shell script "open -n -b \"com.toonboom.harmony.full.Harmony22Premium\" --args " & quoted form of filePath
			on error
				-- Handle files that might be passed as strings
				try
					do shell script "open -n -b \"com.toonboom.harmony.full.Harmony22Premium\" --args " & quoted form of (f as string)
				end try
			end try
		end repeat
	end if
end run
```
5. You can modify the `"Harmony22Premium"` string to fit the version you have installed.
6. Save this in your Applications folder. I saved this as `"Toonboom Harmony Multi-Instance.app"`
7. Find an `.xscene` file in the finder that you would like to open. Right click it and choose `Get Info` or with the file selected use `⌘ + I`
8. In the `Open With:` section, from the dropdown list, choose `Other`
9. Navigate to your Applications folder and choose the application you created earlier
10. Click `Change All...` and then `Continue`

Now you can launch multiple `.xscene` files from the finder and have those open on multiple instances of Toonboom Harmony
