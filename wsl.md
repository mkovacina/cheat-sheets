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

## fixing DNS
1. Create a file: /etc/wsl.conf.
2. Put the following lines in the file in order to ensure the your DNS changes do not get blown away

[network]
generateResolvConf = false

3. In a cmd window, run wsl --shutdown
4. Restart WSL2
5. Create a file: /etc/resolv.conf. If it exists, replace existing one with this new file.
6. Put the following line in the file

nameserver 8.8.8.8 # Or use your DNS server instead of 8.8.8.8 which is a Google DNS server

7. Repeat step 3 and 4. You will see git working fine now.



## References
1. https://www.turek.dev/post/fix-wsl-file-permissions/
2. https://docs.microsoft.com/en-us/windows/wsl/file-permissions
3.  https://gist.github.com/coltenkrauter/608cfe02319ce60facd76373249b8ca6 (fixing DNS)

