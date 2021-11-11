# Windows Admin Reset Hack

Got Root?

## Purpose 

This is a great little trick for recovering accounts/passwords or even getting admin-level privileges on a Windows-based computer. This was recently used by me when our team encountered this exact issue: https://answers.microsoft.com/en-us/windows/forum/all/no-way-to-input-admin-userpass/be15fb3b-49f9-48b8-8de8-631a8b789fbd.  We couldn't join newly re-imaged machine to a domain, and our admin account was having weird behaviour with UAC controls. 

We haven't completely narrowed down the root cause, but it may be Group Policy related on our AD controller.  It definitely isn't the best way to fix this issue...  Especially when you're in a remote situation, don't have access to BIOS passwords, or the drive is encrypted.  However, it saved us in a pinch!  This little hack goes all the way back to the XP/Vista days. I'm surprised its still a thing?

## Description



## Steps
I dont have an answer to why this is happening - but I was able to do hacky a work around for this issue.

1 - Boot the computer off of a USB device.
     a - If this is Windows Media - Choose the Language, click Next.
     b - On the next screen in the bottom left, click "Repair This Computer"
     c - When the next screen loads hit SHIFT+F10 to bring up a command prompt
2 - Move C:\Windows\System32\Utilman.exe to a safe location because we're going to over write it. (C:\)
3 - Copy C:\Windows\System32\cmd.exe to C:\Windows\System32\Utilman.exe.
4 - Reboot the computer. 
5 - When the login menu appears, click the ease of access icon on the bottom right.  A root shell will appear.
6 - Create a user for yourself on the command prompt:
    a - net user <username> <password> /add
    b - net localgroup administrators <username> /add
    c - close command prompt
7 - Log in to windows using the username and password just created
8 - Add the useraccounts needed to the administrator group using commands from step 6
9 - Move C:\Utilman.exe back to C:\Windows\System32\Utilman.exe. Approve overwriting, as we're setting this back to normal.
10 - Log out of this account, and sign back in with the proper local admin account.
11 - Remove your newly created account to clean up after yourself:  
      - net localgroup administrators <username> /delete
      - net user <username> /delete
