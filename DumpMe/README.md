# DumpMe - Cyberdefender.org

## Summary
* [Q1. What is the SHA1 hash of Triage-Memory.mem (memory dump)?](#q1-what-is-the-sha1-hash-of-triage-memorymem-memory-dump)
* [Q2. What volatility profile is the most appropriate for this machine? (ex: Win10x86_14393)](#q2-what-volatility-profile-is-the-most-appropriate-for-this-machine-ex-win10x86_14393)
* [Q3. What was the process ID of notepad.exe?](#q3-what-was-the-process-id-of-notepadexe)
* [Q4. Name the child process of wscript.exe.](#q4-name-the-child-process-of-wscriptexe)
* [Q5. What was the IP address of the machine at the time the RAM dump was created?](#q5-what-was-the-ip-address-of-the-machine-at-the-time-the-ram-dump-was-created)
* [Q6. Based on the answer regarding the infected PID, can you determine the IP of the attacker?](#q6-based-on-the-answer-regarding-the-infected-pid-can-you-determine-the-ip-of-the-attacker)
* [Q7. How many processes are associated with VCRUNTIME140.dll?](#q7-how-many-processes-are-associated-with-vcruntime140dll)
* [Q8. After dumping the infected process, what is its md5 hash?](#q8-after-dumping-the-infected-process-what-is-its-md5-hash)
* [Q9. What is the LM hash of Bob's account?](#q9-what-is-the-lm-hash-of-bobs-account)
* [Q10. What memory protection constants does the VAD node at 0xfffffa800577ba10 have?](#q10-what-memory-protection-constants-does-the-vad-node-at-0xfffffa800577ba10-have)
* [Q11. What memory protection did the VAD starting at 0x00000000033c0000 and ending at 0x00000000033dffff have?](#q11-what-memory-protection-did-the-vad-starting-at-0x00000000033c0000-and-ending-at-0x00000000033dffff-have)
* [Q12. There was a VBS script that ran on the machine. What is the name of the script? (submit without file extension)](#q12-there-was-a-vbs-script-that-ran-on-the-machine-what-is-the-name-of-the-script-submit-without-file-extension)
* [Q13. An application was run at 2019-03-07 23:06:58 UTC. What is the name of the program? (Include extension)](#q13-an-application-was-run-at-2019-03-07-230658-utc-what-is-the-name-of-the-program-include-extension)
* [Q14. What was written in notepad.exe at the time when the memory dump was captured?](#q14-what-was-written-in-notepadexe-at-the-time-when-the-memory-dump-was-captured)
* [Q15. What is the short name of the file at file record 59045?](#q15-what-is-the-short-name-of-the-file-at-file-record-59045)
* [Q16. This box was exploited and is running meterpreter. What was the infected PID?](#q16-this-box-was-exploited-and-is-running-meterpreter-what-was-the-infected-pid)

### Q1. What is the SHA1 hash of Triage-Memory.mem (memory dump)?
Use `sha1sum` for calculating the sha1 vavlue.
```
$ sha1sum Triage-Memory.mem 
c95e8cc8c946f95a109ea8e47a6800de10a27abd  Triage-Memory.mem
```

**Answer:** c95e8cc8c946f95a109ea8e47a6800de10a27abd

### Q2. What volatility profile is the most appropriate for this machine? (ex: Win10x86_14393)
Use `imageinfo` plugin.
```
$ vol.py -f Triage-Memory.mem imageinfo
Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_24000, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_24000, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/ubuntu/Downloads/cyberdefenders/Triage-Memory.mem)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf800029f80a0L
          Number of Processors : 2
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff800029f9d00L
                KPCR for CPU 1 : 0xfffff880009ee000L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2019-03-22 05:46:00 UTC+0000
     Image local date and time : 2019-03-22 01:46:00 -0400
```

**Answer:** Win7SP1x64

### Q3. What was the process ID of notepad.exe?
Use `pslist` plugin then grep string `notepad.exe`
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 pslist | grep notepad.exe
Volatility Foundation Volatility Framework 2.6.1
0xfffffa80054f9060 notepad.exe            3032   1432      1       60      1      0 2019-03-22 05:32:22 UTC+0000
```

**Answer:** 3032

### Q4. Name the child process of wscript.exe.
Use `pstree` plugin then grep string `wscript.exe` and display a line after it.
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 pstree | grep "wscript.exe" -A 1
Volatility Foundation Volatility Framework 2.6.1
.. 0xfffffa8005a80060:wscript.exe                    5116   3952      8    312 2019-03-22 05:35:32 UTC+0000
... 0xfffffa8005a1d9e0:UWkpjFjDzM.exe                3496   5116      5    109 2019-03-22 05:35:33 UTC+0000
```
**Answer:** UWkpjFjDzM.exe

### Q5. What was the IP address of the machine at the time the RAM dump was created?
Use `netscan` plugin to display all connection of the machine.
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 netscan
---SNIP---
0x13f7b4ec0        UDPv4    0.0.0.0:55707                  *:*                                   232      svchost.exe    2019-03-22 05:45:44 UTC+0000
0x13f7e8670        UDPv4    127.0.0.1:59411                *:*                                   3576     iexplore.exe   2019-03-22 05:34:49 UTC+0000
0x13fc6f1b0        UDPv4    0.0.0.0:55102                  *:*                                   232      svchost.exe    2019-03-22 05:45:36 UTC+0000
0x13fc78dc0        UDPv4    127.0.0.1:53361                *:*                                   1272     EXCEL.EXE      2019-03-22 05:34:03 UTC+0000
0x13f7ae010        TCPv4    10.0.0.101:49263               52.96.44.162:443     ESTABLISHED      3688     OUTLOOK.EXE    
0x13fa93cf0        TCPv4    -:49173                        72.51.60.132:443     CLOSED           1272     EXCEL.EXE      
0x13fa95cf0        TCPv4    -:49170                        72.51.60.132:443     CLOSED           1272     EXCEL.EXE      
0x13fa969f0        TCPv4    -:0                            56.219.119.5:0       CLOSED           1272     EXCEL.EXE      
0x13fbd07e0        TCPv4    -:49372                        212.227.15.9:25      CLOSED           504                     
0x13fc857e0        TCPv4    -:49167                        72.51.60.132:443     CLOSED           1272     EXCEL.EXE
```
At `2019-03-22 05:34:03 UTC+0000`, we see the IP of the local machine.<br>
**Answer:** 10.0.0.101

### Q6. Based on the answer regarding the infected PID, can you determine the IP of the attacker?
We have already known that the infected PID is 3495 from Q4. So re-execute the command from Q5 then grep with this string.<br>
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 netscan | grep 3496
Volatility Foundation Volatility Framework 2.6.1
0x13e397190        TCPv4    10.0.0.101:49217               10.0.0.106:4444      ESTABLISHED      3496     UWkpjFjDzM.exe
```
**Answer:** 10.0.0.106

### Q7. How many processes are associated with VCRUNTIME140.dll?
Use `dlllist` to display all dll then grep `VCRUNTIME140.dll`.
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 dlllist | grep VCRUNTIME140.dll
Volatility Foundation Volatility Framework 2.6.1
0x000007fefa5c0000            0x16000             0xffff 2019-03-22 05:32:05 UTC+0000   C:\Program Files\Common Files\Microsoft Shared\ClickToRun\VCRUNTIME140.dll
0x00000000745f0000            0x15000             0xffff 2019-03-22 05:33:49 UTC+0000   C:\Program Files (x86)\Microsoft Office\root\Office16\VCRUNTIME140.dll
0x00000000745f0000            0x15000             0xffff 2019-03-22 05:34:37 UTC+0000   C:\Program Files (x86)\Microsoft Office\root\Office16\VCRUNTIME140.dll
0x00000000745f0000            0x15000                0x3 2019-03-22 05:34:49 UTC+0000   C:\Program Files (x86)\Microsoft Office\root\Office16\VCRUNTIME140.dll
0x00000000745f0000            0x15000             0xffff 2019-03-22 05:35:09 UTC+0000   C:\Program Files (x86)\Microsoft Office\root\Office16\VCRUNTIME140.dll
```
**Answer:** 5

### Q8. After dumping the infected process, what is its md5 hash?
Use `procdump` and specify the PIP of infected process then store all associated process in `dumps` directory.
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 procdump -D dumps/ 3496
$ md5sum dumps/executable.3496.exe 
690ea20bc3bdfb328e23005d9a80c290  dumps/executable.3496.exe
```
**Answer:** 690ea20bc3bdfb328e23005d9a80c290

### Q9. What is the LM hash of Bob's account?
Use `hashdump` for dumping all hash of the machine.
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 hashdump
Volatility Foundation Volatility Framework 2.6.1
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Bob:1000:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
```

### Q10. What memory protection constants does the VAD node at 0xfffffa800577ba10 have?
Use `vadinfo` to displays extended information about a process’s VAD nodes then grep with `0xfffffa800577ba10`.
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 vadinfo | grep '0xfffffa800577ba10' -A 3
Volatility Foundation Volatility Framework 2.6.1
VAD node @ 0xfffffa800577ba10 Start 0x0000000000030000 End 0x0000000000033fff Tag Vad 
Flags: NoChange: 1, Protection: 1
Protection: PAGE_READONLY
Vad Type: VadNone
^C
```
**Answer:** PAGE_READONLY

### Q11. What memory protection did the VAD starting at 0x00000000033c0000 and ending at 0x00000000033dffff have?
Based on the format of the result from `vadinfo` on Q10, we will grep `0x00000000033c0000 End 0x00000000033dffff`. 
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 vadinfo | grep '0x00000000033c0000 End 0x00000000033dffff' -A 3
Volatility Foundation Volatility Framework 2.6.1
VAD node @ 0xfffffa80052652b0 Start 0x00000000033c0000 End 0x00000000033dffff Tag VadS
Flags: CommitCharge: 32, PrivateMemory: 1, Protection: 24
Protection: PAGE_NOACCESS
Vad Type: VadNone
```
**Answer:** PAGE_NOACCESS

### Q12. There was a VBS script that ran on the machine. What is the name of the script? (submit without file extension)
Use `cmdline` to display everthing in cmd then grep with `vbs`.
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 cmdline | grep vbs
Volatility Foundation Volatility Framework 2.6.1
Command line : "C:\Windows\System32\wscript.exe" //B //NOLOGO %TEMP%\vhjReUDEuumrX.vbs
```
**Answer:** vhjReUDEuumrX

### Q13. An application was run at 2019-03-07 23:06:58 UTC. What is the name of the program? (Include extension)
Use `shimcache` to obtain information about all executed binaries the have been execute in the system then grep with the time given in the question.
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 shimcache | grep "2019-03-07 23:06:58"
Volatility Foundation Volatility Framework 2.6.1
2019-03-07 23:06:58 UTC+0000   \??\C:\Program Files (x86)\Microsoft\Skype for Desktop\Skype.exe
```

### Q14. What was written in notepad.exe at the time when the memory dump was captured?
Follow this [link](https://andreafortuna.org/2018/03/02/volatility-tips-extract-text-typed-in-a-notepad-window-from-a-windows-memory-dump/). Find PID of notepad.exe then use `memdump` plugin to dump it for analysing.
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 pslist | grep notepad
Volatility Foundation Volatility Framework 2.6.1
0xfffffa80054f9060 notepad.exe            3032   1432      1       60      1      0 2019-03-22 05:32:22 UTC+0000

$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 memdump -D . -p 3032
Volatility Foundation Volatility Framework 2.6.1
************************************************************************
Writing notepad.exe [  3032] to 3032.dmp

$ strings -e l 3032.dmp | grep flag
flag<REDBULL_IS_LIFE>
\Registry\Machine\Software\Microsoft\Windows nt\currentversion\appcompatflags\AIT
Allow flag to be passed with CreateFile call that indicates to perform downgrade if applicable.
thread_flags
---SNIP---
```
**Answer:** flag<REDBULL_IS_LIFE>

### Q15. What is the short name of the file at file record 59045?
Use `mftparser` to find the of the file at file record 59045. The NTFS file system contains a file called the [master file table](https://docs.microsoft.com/en-us/windows/win32/fileio/master-file-table), or MFT. There is at least one entry in the MFT for every file on an NTFS file system volume, including the MFT itself .This plugin scans for and parses potential MFT entries.
```
$ vol.py -f Triage-Memory.mem --profile=Win7SP1x64 mftparser | grep "Record Number: 59045" -A 20
Volatility Foundation Volatility Framework 2.6.1
Record Number: 59045
Link count: 2


$STANDARD_INFORMATION
Creation                       Modified                       MFT Altered                    Access Date                    Type
------------------------------ ------------------------------ ------------------------------ ------------------------------ ----
2019-03-17 06:50:07 UTC+0000 2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:42 UTC+0000   Archive

$FILE_NAME
Creation                       Modified                       MFT Altered                    Access Date                    Name/Path
------------------------------ ------------------------------ ------------------------------ ------------------------------ ---------
2019-03-17 06:50:07 UTC+0000 2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:42 UTC+0000   Users\Bob\DOCUME~1\EMPLOY~1\EMPLOY~1.XLS

$FILE_NAME
Creation                       Modified                       MFT Altered                    Access Date                    Name/Path
------------------------------ ------------------------------ ------------------------------ ------------------------------ ---------
2019-03-17 06:50:07 UTC+0000 2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:42 UTC+0000   Users\Bob\DOCUME~1\EMPLOY~1\EmployeeInformation.xlsx

$OBJECT_ID
Object ID: 00fe50d2-4841-e911-8751-000c2958bc5f
```
**Answer:** EMPLOY~1.XLS

### Q16. This box was exploited and is running meterpreter. What was the infected PID?
Use `pstree` to display tree view of processes.
```
0xfffffa8005a80060:wscript.exe                    5116   3952      8    312 2019-03-22 05:35:32 UTC+0000
... 0xfffffa8005a1d9e0:UWkpjFjDzM.exe                3496   5116      5    109 2019-03-22 05:35:33 UTC+0000
.... 0xfffffa8005bb0060:cmd.exe                      4660   3496      1     33 2019-03-22 05:35:36 UTC+0000
```
We see a strange thing that `cmd.exe` process is spawn by `UWkpjFjDzM.exe` not by `explorer.exe` as normal, and this `cmd.exe` is meterpreter.<br>
**Answer:** 3496
