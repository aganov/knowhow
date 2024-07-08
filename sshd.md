Update your firewall rules to allow the new port. If you're using UFW:

```
ufw allow <new_port_number>/tcp
ufw reload
```

1. Run the following command to edit the SSH socket unit file:

`EDITOR=vim systemctl edit ssh.socket`

2. This will open an editor with an empty file. Add the following lines:

```
[Socket]
ListenStream=
ListenStream=<new_port_number>
```

> The line ListenStream= is required that port 22 is no longer used. Without this line, the SSH server would then be accessible via port 22 (default) and <new_port_number>.

3. Save the file and exit the editor.

4. Reload the systemd manager configuration

```
systemctl daemon-reload
```

5. Restart the SSH socket

```
systemctl restart ssh.socket
```

6. Verify that the new port is being used:

```
ss -tlnp | grep ssh
```

