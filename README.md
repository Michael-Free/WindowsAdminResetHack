# Windows Admin Reset Hack

Got Root?

## Purpose 

This is a great little trick for recovering accounts/passwords or even getting admin-level privileges on a Windows-based computer. This was recently used by me when our team encountered this [exact issue](https://answers.microsoft.com/en-us/windows/forum/all/no-way-to-input-admin-userpass/be15fb3b-49f9-48b8-8de8-631a8b789fbd).  We couldn't join newly re-imaged machine to a domain, and our admin account was having weird behaviour with UAC controls. The link does have someone suggest a fix - but it was suggesting registry edits, which required admin rights in the first place, and booting into recovery to modify a registry entry wasn't quick and dirty enough.

We haven't completely narrowed down the root cause, but it may be Group Policy related on our Domain Controller.  It definitely isn't the best way to fix this issue...  Especially when you're in a remote situation, don't have access to BIOS passwords, or the drive is encrypted.  However, it saved us in a pinch!  

This little hack goes all the way back to the XP/Vista days. I'm surprised its still a thing?

## Description

This quick-and-dirty little hack allows a user to get an Administration/System-Level Command Line prompt on Windows at the login screen.  Allowing them to do whatever they want or needed to that machine if they are denied Administrative access to it. This is also great when you have forgotten your password to an account on your own Windows computer.

## Steps

1. Boot the device off of USB with some Live Linux Distro or Window Recovery Media.
     - If this is Windows 10 Recovery Media:
          - Once the media has fully booted, choose the Language, click Next.
          - Instead of proceeding with a new install, in the bottom left-hand corner of the window, click "Repair This Computer."
          - When the next screen loads up, hit the SHIFT+F10 keys to bring up a command prompt.
     - If this is a Linux Live Distro:
          - Open up a terminal window and mount the computer's hard drive where Windows is installed.
2. Make a backup of Utilman.exe
     - If this is Windows 10 Recovery Media:
          - `move C:\Windows\System32\Utilman.exe C:\Windows\System32\Utilman.exe.backup`
     - If this is a Linux Live Distro:
          - `mv /mnt/harddrive/Windows/System32/Utilman.exe /mnt/harddrive/Windows/System32/Utilman.exe.backup`
3. Copy C:\Windows\System32\cmd.exe to C:\Windows\System32\Utilman.exe
4. Reboot the computer and remove the boot media just used.
5. When the Login Screen appears - click on the Ease of Access icon in the bottom right-hand corner
6. Create a user for yourself on the command prompt
7. Log into Windows using the username just created.
8. Add the user account that doesn't have Admin access to the Administrators local group
9. Clean up after yourself! 
10. Log out of the account you just created, and log back in to the computer with the other account that lost admin access
11. Clean up after yourself some more!



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
