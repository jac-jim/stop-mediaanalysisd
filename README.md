# Stop `mediaanalysisd`
How to stop `mediaanalysisd`?  This mystery service has been terrorizing users for nearly a decade, wasting electricity and possibly SSD write life.  Please keep your *System Integrity Protection* enabled on your **macOS** machines, because classic process control techniques exist to handle this situation thanks to UNIX principles.  These instructions show how to setup a safe way to stop `mediaanalysisd` from wasting your system resources.

## Installation
Open the **Terminal** app via **Finder**, and follow these instructions for each user on your system:

1. Make sure the contents of the `stop-mediaanalysisd.sh` *BASH* script are in your home directory.
2. Make the script executable by running `chmod 755 ~/stop-mediaanalysisd.sh`.
3. Run `crontab -e` and add the following: 
```
* * * * * ~/stop-mediaanalysisd.sh >> ~/stop-mediaanalysisd.log 2>&1
```

4. Press the `ESC` key on your keyboard, carefully enter the string `:wq` and press `Enter` (or `Return`) on your keyboard.  The **Terminal** window should restore back to showing the commands you ran previously, and you'll see a message saying the crontab was installed.

If you get stuck on **Step 4**, then please look up a YouTube video of how to use `crontab -e`.

### Log File
A log file is created, called `stop-mediaanalysisd.log`.  Ideally this file remains empty.  If you find a problem, open an Issue in the GitHub repository here.

## Explanation
The `crontab` is a list of jobs that should run periodically, and five astericks means run the job every minute.  

Every minute, the script will run, search for all process ID's that belong to `mediaanalysisd`, and `STOP` them.  This is unlike killing them.  Instead, the processes are paused.  Running this script every minute finds the new process ID incase it has been restarted.

Additionally, the script re-prioritizes the paused process to have the lowest runtime priority (of `20`, see `man renice`) for scheduling by the operating system.

This approach starves the process from doing whatever it is that it does.  Perhaps some day when *Apple* is more transparent about what exactly `mediaanalysisd` is doing and offers a proper configuration interface, we can let it run.

