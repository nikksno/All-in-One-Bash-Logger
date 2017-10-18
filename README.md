# All-in-One-Bash-Logger
A one-touch bash logging solution to be simply pasted inside the script itself

```bash
# All in one logging | v0.3 | 20171018 173600 | 20171018 175600 | Nk

logname=`basename "$0"`                   # The name of this script
now="$(date +"%Y-%m-%d_%H-%M-%S")"        # The current timestamp
logdir="/root/logs/$logname"              # Don't store anything else than logs in here!
logfile="$logdir/$now"                    # The new logfile
mkdir -p $logdir                          # Touch the dir
touch $logfile                            # Touch the file
rm $logdir/latest-log                     # Remove the old latest-log symlink
ln -s $logfile $logdir/latest-log         # Recreate the symlink
ls -t $logdir | tail -n 4 | xargs rm --   # Delete all logs older than the newest 42
exec >  >(tee -ia $logfile)               # Log one output to logfile
exec 2> >(tee -ia $logfile >&2)           # Log the other output to logfile
```
