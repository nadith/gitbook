---
description: Remote login to the Linux server
---

# VMWare Horizon

## Why are my files missing in the VMware horizon server?&#x20;

> I can't see my files in VMware horizon server anymore. The files which were shown during the my previous login to the server, are not there when I login again  today!

VMware Horizon is a temporary environment and it expires your data once the login session is expired. During a connection drop, you will not lose files as long as you can restore the connection back to the server quickly (hopefully your previous session is still alive). But once you log out (close VMware Horizon Client or Web Browser Client), you will lose all your files once the session is expired.

Before you close the client, copy your work somewhere permanent such as:

1. Shared lab drive (which you can also access via the Lab PCs)
2. Cloud Storage (One Drive, Google Drive)
3. Shared folder in your Local PC (folder sharing is only available via VMware Horizon Client application but not through the web client; once you login to VMware Horizon Linux server, click on options -> shared folders on VMware Horizon Client)
