# windows subsystem for linux

## fix bad mount permissions
add to the `/etc/wsl.conf` file (create if needed)
	```
	[automount]
	enabled = true
	options = "metadata,umask=22,fmask=11"
	```

## fix bad default file permissions
add to ```/etc/bash.bashrc```
	```
	# Note: Bash on Windows does not currently apply umask properly.
	if [[ "$(umask)" = "0000" ]]; then
	  umask 0022
	  fi
	  ```
## accessing linux files from windows
browse to `\\wsl$`

## References
1. https://www.turek.dev/post/fix-wsl-file-permissions/
2. https://docs.microsoft.com/en-us/windows/wsl/file-permissions

