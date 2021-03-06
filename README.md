# All-in-One-Bash-Logger

All you need to do is paste the following textblock under the shebang in your script! That's all!

You'll find logfiles in **~/logs/**.

```bash
# All in One Bash Logger | v0.51 | 20171018 | 20171214 | Nk

scriptname=$(basename "$0")                                             # The name of this script
now="$(date +"%Y-%m-%d_%H-%M-%S")"                                      # The current timestamp
logdir="$HOME/logs/$scriptname"                                         # Don't store anything else than logs in here!
logfile="$logdir/$now"                                                  # The new logfile
if [[ -t 1 ]]; then interactive=y; else interactive=n; fi               # Determine if this is an interactive shell
mkdir -p "$logdir"                                                      # Touch the dir
touch "$logfile"                                                        # Touch the file
if [ -f "$logdir"/latest-log ]; then rm "$logdir"/latest-log; fi        # Remove the old latest-log symlink
ln -s "$logfile" "$logdir"/latest-log                                   # Recreate the symlink
( cd "$logdir" && rm "$(ls -t | awk 'NR>43')" ) 2> /dev/null || true    # Delete all logs older than the newest 42
exec >  >(tee -ia "$logfile")                                           # Log stdout to logfile
if [ $interactive = "n" ]; then exec 2> >(tee -ia "$logfile" >&2); fi   # Log stderr to logfile if non-interactive
echo && echo "Starting $scriptname script on $now..." && echo           # Write heading to logfile
chmod -R 700 "$logdir"                                                  # Secure logs directory

```
