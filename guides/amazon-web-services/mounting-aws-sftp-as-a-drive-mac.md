---
description: Guide to mounting an s3 bucket via SFTP as a drive on your computer
---

# Mounting AWS SFTP as a Drive (Mac)

**PREP**

1. Check/Create SSH/RSA Keys
   1. The RSA Keys should not have passwords, create new keys (without a password) if the current keys the user has were setup with a password
2. Setup SFTP Account on AWS
   1. If you’re not an Admin ask one (David, Phill) to create your account
3. Add the users SSH/RSA key to their FTP account
4. Make sure the user you use in your computer is an admin on that computer
   1. We’ll need to run commands under  \`sudo\`

**SETUP**

1. Download FUSE & SSHFS from [https://osxfuse.github.io/](https://osxfuse.github.io/)
2.
   1. Install FUSE
   2. Install SSHFS
3. Restart the computer
4. Open the Terminal App, can be found in the Applications Folder under Utilities
5. Check sshfs is install with his command \`\`\`sshfs --help\`\`\`
6. Create two directories, \`mnt/patterns\`
   1. mkdir \~/mnt
   2. mkdir \~/mnt/patterns&#x20;
7. Locate the SSH/RSA Keys
   1. This is probably at \`\~/.ssh/\`,
8. Save/copy the users FTP account username (ask AWS admin if you don’t have it)
9. Try connecting/mounting the Drive with this command, replacing RSA Key and Username with the values from the previous two steps
   1. ```bash
      sshfs -o IdentityFile=RSAPublicKeyLocation -odebug,sshfs_debug,loglevel=debug -o defer_permissions -o noappledouble -o volname=patterns username@assets_sftp.boston.gov:/patterns.boston.gov/assets ~/mnt/patterns/
      ```
   2. If this doesn’t work try \`\`\`sftp -o IdentityFile=RSAPublicKeyLocation [username@assets\_sftp.boston.gov](mailto:username@assets\_sftp.boston.gov)\`\`\`
   3. This should work,  if not trouble shoot by looking at the logs from the previous command (#1)
10. Now you are able to mound it’s time to create a Bash script that will run when the user logs in.
11. Using a Text or Code Editor create a bash file at \`\`\`/Library/Startup.sh\`\`
12. Copy the code below into the file&#x20;

    ```bash
    #!/bin/sh

    USERNAME=sftp_username
    RSAPUBLICKEYLOCATION=~/.ssh/id_rsa
    MOUNTLOCATION=~/mnt/patterns/

    sshfs -o IdentityFile=$RSAPUBLICKEYLOCATION -o reconnect,ServerAliveInterval=15,ServerAliveCountMax=3 -o defer_permissions -o noappledouble -o volname=patterns $USERNAME@assets_sftp.boston.gov:/patterns.boston.gov/assets $MOUNTLOCATION
    ```
13. From the Terminal app, make the file executable: \`\`\`chmod +x /Library/Startup.sh\`\`\`
14. Get to this file using the \`Finder\`, then right-click on the file and select the 'Get Info' option. Use the 'Open with:' to use 'Terminal'. Can be found under 'Applications > Utilities' and check the 'Enable' drop down to 'All Applications'
15. Open up “System Preferences” and go to “Users & Groups”
16. Switch to the “Login Items” tab, unlock the ability to edit these settings by clicking the Padlock in the bottom left.
17. Use the “+” button to add a new action in “Login Items”, this will open up a file browser window.
18. Use the File Browser to locate the “Startup.sh” file we created in the “Library” and select it.
19. Use the Apple icon on the top left of the screen to “Log Out”
20. When you sign in again open up a “Finder” window and check if the drive mounted at \~/mnt/patterns



Debug Tips

```
# Manual Mount
sshfs -o IdentityFile=~/.ssh/id_rsa_cob -odebug,sshfs_debug,loglevel=debug -o defer_permissions -o noappledouble -o volname=patterns phillip_kelly@assets_sftp.boston.gov:/patterns.boston.gov/assets ~/mnt/patterns/

Unmount
- diskutil unmount ~/mnt/assets/
- sudo mount -a

CHOWN
780  sudo chown phillipkelly /Users/phillipkelly/mnt/patterns2/

CHMOD
672  sudo chmod 777 ~/mnt/assets
794  sudo chmod +x ~/Library/Startup.cmd
```















