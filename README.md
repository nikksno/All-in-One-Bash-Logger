# All-in-One-Bash-Logger

All you need to do is paste the following textblock under the shebang in your script! That's all!

You'll find logfiles in /root/ [only works for root, change line 3 if running as a different user]

```bash
# All in One Bash Logger | v0.42 | 20171018 173600 | 20171018 124800 | Nk

logname=`basename "$0"`                      # The name of this script
now="$(date +"%Y-%m-%d_%H-%M-%S")"           # The current timestamp
logdir="/root/logs/$logname"                 # Don't store anything else than logs in here!
logfile="$logdir/$now"                       # The new logfile
mkdir -p $logdir                             # Touch the dir
touch $logfile                               # Touch the file
rm $logdir/latest-log                        # Remove the old latest-log symlink
ln -s $logfile $logdir/latest-log            # Recreate the symlink
( cd $logdir && rm `ls -t | awk 'NR>43'` )   # Delete all logs older than the newest 42
exec >  >(tee -ia $logfile)                  # Log one output to logfile
exec 2> >(tee -ia $logfile >&2)              # Log the other output to logfile
```
