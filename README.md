# Stop `mediaanalysisd`
Please keep your *System Integrity Protection* enabled on your **macOS** machines, because classic process control techniques exist to handle this situation thanks to UNIX principles.

## Installation
Open the **Terminal** app via **Finder**, and follow these instructions:

1. Make sure the contents of the `stop-mediaanalysisd.sh` *BASH* script are in your home directory.
2. Make the script executable by running `chmod 755 ~/stop-mediaanalysisd.sh`.
3. Run `crontab -e` and add the following: 
```
* * * * * /Users/<your-user-name>/stop-mediaanalysisd.sh >> /Users/<your-user-name>/stop-mediaanalysisd.log 2>&1
```
4. Press the `ESC` key on your keyboard, carefully enter the string `:wq` and press `Enter` (or `Return`) on your keyboard.  The **Terminal** window should restore back to showing the commands you ran previously, and you'll see a message saying the crontab was installed.

If you get stuck on **Step 4**, then please look up a YouTube video of how to use `crontab -e`.

## Explanation
Every minute, the script will run, search for all process ID's that belong to `mediaanalysisd`, and `STOP` them.  This is unlike killing them.  Instead, the process is paused.  Running this script every minute makes sure the process stays paused and finds the new process ID incase it has been restarted.

Additionally, the script re-prioritizes the paused process to have the lowest runtime priority for scheduling by the operating system.

This approach starves the process from doing whatever it is that it does.  Perhaps some day when *Apple* is more transparent about what exactly `mediaanalysisd` is doing and offers a proper configuration interface, we can let it run.
