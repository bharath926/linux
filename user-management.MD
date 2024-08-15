[www.aventurecloud.co](http://www.aventurecloud.co)

**Document Privacy:** This document belongs to **Aventure Cloud LLC** which is here after referred as Company and is intended for use by persons with whom it is shared by the Company. It is copyrighted and prohibited to share to persons other than intended and by anymeans like print, physical or electronic or other. Any violations attract legal proceedings and penalties as per applicable US and Global laws.

# **Ubuntu**

### **Create user**

```bash
adduser admin1

```

### **Find the created user details**

```bash
id admin1

uid=1001(admin1) gid=1001(admin1) groups=1001(admin1),100(users)

```

### **Find the created user details in the file /etc/passwd**

```bash
grep admin1 /etc/passwd

admin1:x:1001:1001:,,,:/home/admin1:/bin/bash

```

### Find the user’s hashed password

```bash
sudo cat /etc/shadow | grep admin1

admin1:$y$j9T$pMOcPWyw.l2iyyF3U0auk1$/Nul7hGzidDoJari9n.C59h38FucIIEnVksB9t0YU6C:19949:0:99999:7:::

```

### Find the default hashing algorithm

```bash
grep success=1 /etc/pam.d/common-password

password        [success=1 default=ignore]      pam_unix.so obscure yescrypt

```

### Change root password

```bash
sudo passwd

New password:
Retype new password:
passwd: password updated successfully

```

### Verify that root password is written to /etc/shadow file (By default you see a *)

```bash
sudo cat /etc/shadow | grep root

root:$y$j9T$UdRDlK42CGAGg7P8DUOPv0$Whq/ch7bRsU2L6qGdmPwOAJZXuEndxjzpg7SIO6kSO9:19949:0:99999:7:::

```

### Viewing default login permissions (Password vs SSH Key)

**On a machine in Azure when the default user is created with a user/password combination**

Connect to the system using Putty.

### Become a root user

```bash
sudo su -

```

### View the default Login Permissions by executing these commands

To find if Password Authentication is allowed

```bash
sshd -T

```

OR

```bash
sshd -T | grep passwordauthentication

passwordauthentication yes

```

Which means **Password Authentication** is allowed.

To find if SSH Key Authentication is allowed

```bash
sshd -T | grep pubkeyauthentication

pubkeyauthentication yes

```

Which means **SSH Key Authentication** is allowed.

**RESULT:** Both Password & SSH Key ALLOWED

**On a machine in AWS when the default user is created with with a user/ssh key combination**

To find if Password Authentication is allowed

```bash
sshd -T | grep passwordauthentication

passwordauthentication no

```

To find if SSH Key Authentication is allowed

```bash
sshd -T | grep pubkeyauthentication

pubkeyauthentication yes

```

**RESULT:** Only SSH Key ALLOWED

### **How to Allow both Password Authentication**

**Connect to the system using Putty and become a root user**

```bash
sudo su -

```

**Edit /etc/ssh/sshd_config using nano editor**

```bash
nano /etc/ssh/sshd_config

```

**Uncomment this line and change it to yes**

```bash
PasswordAuthentication yes

```

**Change this line to yes**

```bash
KbdInteractiveAuthentication yes

```

**Restart SSH service**

```bash
sudo systemctl restart ssh

```

> **TEST:** Create a user called test and login with password

```bash
adduser test

```
