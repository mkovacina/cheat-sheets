# Tracing
Write-Debug will write messages to the screen. You need to set, $DebugPreference = "continue", so they actually appear. Setting $DebugPreference and $VerbosePreference can sometimes give you clues on what other commands are doing.
Write-Host is a more normal/boring way of getting output. Out-Host is sometimes better because it preserves format and output

Write-Output is usually a better alternative... Write-Host cant be captured into log files.

More info on Write-Host and Write-Output is here http://blogs.msdn.com/monad/archive/2005/11/09/490625.aspx


[Error information]
$error holds the error information from commands. To get more details information, try
$error | select -ExpandProperty Exception | Format-List -f *
$error is explained here https://blogs.msdn.com/powershell/archive/2006/04/25/583229.aspx
A better Resolve-Error function from Jeffery can be found here http://blogs.msdn.com/powershell/archive/2006/12/07/resolve-error.aspx

$LASTEXITCODE tells you if a native command completed successfully or not. For example ping example.com gives $LASTEXITCODE of 1

$? Tells you if the last command had any error in it. This includes if you tried to execute a command that does not exist. Pping will not set $LASTEXITCODE, but it will set $? To False



# personal profile#
C:\Documents and Settings\%username%\My Documents\WindowsPowerShell\profile.ps1

[Event logs]
Get-EventLog -LogName Application -Source Outlook

[Can't run the command]
Windows PowerShell is designed not to run any script from the current folder, thereby preventing any script from hijacking an operating system command.  You need a relative or absolute path (i.e., "./command" or "c:\command").

By default, Windows PowerShell is not able to run scripts.
PS C:\test> set-executionpolicy remotesigned

The RemoteSigned execution policy allows unsigned scripts to execute from the local computer-downloaded scripts still have to be signed in order to run. A better policy is AllSigned, which only executes scripts that have been digitally signed with a certificate issued by a trusted publisher. However, I don't have a certificate handy so I'm unable to sign my scripts, making RemoteSigned a rather good choice for this situation. Now I'll try running my script again:

PS C:\test> ./disableservices
Disabling messenger
Disabling alerter
PS C:\test>

I want to point out that the RemoteSigned execution policy we're using isn't a great choice; it's merely a good one. But there is a much better solution. It would be far more secure to obtain a code-signing certificate, use the Windows PowerShell Set-AuthenticodeSignature cmdlet to sign my scripts, and set the execution policy to the much safer AllSigned policy.
There's also another policy, Unrestricted, which you definitely should avoid. This policy allows all scripts-even malicious scripts from remote locations-to run unfettered on your computer, which can put you in a very dangerous position. Consequently, I don't recommend using the Unrestricted policy for any reason.

# Piping

The pipe doesn't work exactly like in Unix.  First, they are objects being piped, not text.  Fine.  That's great.

But, let's say you do this "get-content c:\data.dat | echo". 
=> Linux, echo invoked once with the contents of c:\data.dat
=> PowerShell, echo invoked once for each line object in the file

***
dir, not just for directories anymore

# list all environment variable
PS> dir env:         

# list all defined aliases
PS> dir alias:

# list all defined functions
PS> dir function:

# list all defined variables
PS> dir variable:

# Invoking Methods Using Variables
PS> $d=Get-Date 
PS> foreach ($p in "Day","hour","minute") {"$p :  " + $d.$p } 
Day :  5 
hour :  8 
minute :  53

PS> foreach ($p in "AddDays","AddHours","AddMinutes"){"$p :  " + $d.$p.Invoke(1) } 
AddDays :  01/06/2009 08:53:53 
AddHours :  01/05/2009 09:53:53 
AddMinutes :  01/05/2009 08:54:53

# Finding the computer name
PS> $env:COMPUTERNAME # Check local computer name. 

# Piping output of foreach loops
$NumArray = (1..12)
$(foreach ($number in $numArray ) { $number * 7}) | set-variable 7x
$7x
# Option research properties by removing # on the next line
# $7x |get-Member

Dejan Milic's Method (Better)

$NumArray = (1..12)
$7x = @()
foreach ($number in $numArray ) { $7x+=$number * 7}
$7x

/\/\o\/\/'s  Method (Fantastic)

$7x = 1..12 |% {$_ * 7 }
$7x

# List Startup commands
Here is a PowerShell one liner to list startup command on your machine

PS C:\Users\Ying> Get-WmiObject -class "Win32_Startupcommand"

Command User Caption
------- ---- -------
DING!.lnk Dino\Ying DING!
ÌÚÑ¶QQ.lnk Dino\Ying ÌÚÑ¶QQ 

# Display message in the Notification Area
[void] [System.Reflection.Assembly]::LoadWithPartialName("System.Windows.Forms")

$objNotifyIcon = New-Object System.Windows.Forms.NotifyIcon 

$objNotifyIcon.Icon = "C:\Scripts\Forms\Folder.ico"
$objNotifyIcon.BalloonTipIcon = "Info" 
$objNotifyIcon.BalloonTipText = "Retrieving files from C:\Windows." 
$objNotifyIcon.BalloonTipTitle = "Retrieving Files" 

$objNotifyIcon.Visible = $True 
$objNotifyIcon.ShowBalloonTip(10000)

Get-ChildItem C:\Windows

$objNotifyIcon.BalloonTipText = "The script has finished running." 
$objNotifyIcon.BalloonTipTitle = "Files retrieved." 

$objNotifyIcon.Visible = $True 
$objNotifyIcon.ShowBalloonTip(10000)

# PS and XML #text
PS> $xml.root.element."#text"
Text
PS>

It's weird, but makes sense after a while.  ; )