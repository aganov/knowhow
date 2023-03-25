```sh
sar -u 1 # Check CPU steal
lsof -i -a -p PID # list connections per pid
passenger-status | grep PID | awk '{ print $3}' | xargs -i sudo lsof -i -a -p {} | egrep 'TCP|UDP' # list the open sockets for all passenger's processes
```
