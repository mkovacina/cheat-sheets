# windbg cheat sheet

# setting up .NET ability
`.loadby sos clr`

# remote debugging in a VM
1. Push "C:\Program Files\Microsoft Visual Studio 9.0\Common7\IDE\Remote Debugger" to the VM
2. On the VM
	a. Open "Start>All Programs>Administrative Tools>Computer Management"
	b. Create a new user with your OnBase username and password
		i. !!! The username and password MUST match that of your network login
	c. Make this user a local administrator on the VM
3. Launch msvsmon.exe using "Run As…" and use the credentials for your account (from section 2)
4. In VS on your machine, "Attach to Process…"
5. For the qualifier, use "username@vm"
	a. msvsmon.exe will display this for you if you are unsure of what to use
Select your process and go to town.

# dumpheap
`!dumpheap -stat -type <filter> /D`

# break on all clr exceptions
`sxe clr`

# basic SOS commands
command | description 
--------|------------
!threads				|	view managed threads
!clrstack				|	view the managed call stack
!dumpstack				|	view combined unmanaged & managed call stack
!clrstack -p			|	view function call arguments
!clrstack –l			|	view stack (local) variables
!name2ee module class	|	view addresses associated with a class or method
!dumpmt –md address		|	view the method table & methods for a class
!dumpmd address			|	view detailed information about a method
!do address				|	view information about an object
!dumpheap –stat			|	view memory consumption by type
!dumpheap –min size		|	view memory consumption by object when at least size
!pe!dumpheap –type type	|	view memory consumption for all objects of type type
!gcroot address			|	view which object are holding a reference to address
!syncblk				|	view information about managed locks

# how to get a stacktrace
Finally, you need to capture the debug information to include in a bug comment or support request.  Enter these three commands, one at a time, to get the stacktrace, crash analysis, and log of loaded modules.  (Again, press Enter after each command)
kp
!analyze -v -f
lm

# print the content of a system.string object
$$ Dumps the managed strings to a file
$$ Platform x86
$$ Usage $$>a<"c:\temp\dumpstringtofolder.txt" 6544f9ac 5000 c:\temp\stringtest
$$ First argument is the string method table pointer
$$ Second argument is the Min size of the string that needs to be used filter
$$ the strings
$$ Third is the path of the file
.foreach ($string {!dumpheap -short -mt ${$arg1}  -min ${$arg2}})
{ 

  $$ MT        Field      Offset               Type  VT     Attr    Value Name
  $$ 65452978  40000ed        4         System.Int32  1 instance    71117 m_stringLength
  $$ 65451dc8  40000ee        8          System.Char  1 instance       3c m_firstChar
  $$ 6544f9ac  40000ef        8        System.String  0   shared   static Empty

  $$ start of string is stored in the 8th offset, which can be inferred from above
  $$ Size of the string which is stored in the 4th offset
  r@$t0=  poi(${$string}+4)*2
  .writemem ${$arg3}${$string}.txt ${$string}+8 ${$string}+8+@$t0
}

Pasted from <http://stackoverflow.com/questions/5349945/windbg-and-sos-how-do-i-print-dump-a-large-string> 

