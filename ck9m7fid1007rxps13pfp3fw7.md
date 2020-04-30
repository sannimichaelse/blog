## How to create Sudo Enabled User on CentOS 7

## Introduction

Sudo (superuser do) is a utility for UNIX- and Linux-based systems that provides an efficient way to give specific users permission to use specific system commands at the root (most powerful) level of the system. The sudo commands give administrative privileges to normal users. In this guide you will learn how to create a new user with sudo access on CentOS 7 as well as how to add user to the Sudoers configuration file

## Prerequisite

- A system running CentOS 7
- Access to a user account with root privileges

## Add User to Sudoers Group on CentOS

## STEP 1 - Login as administrator

SSH into your server as the root user

```
$ ssh root@ip_address
```

Enter root user login credentials when prompted

## STEP 2 - Create a new user

```
$ adduser tomiwa
```

Use the actual username for your new user in place of tomiwa

Next create a password for the new user by entering the following command

```
$ passwd tomiwa
```

You will get a prompt in which you can set and confirm a password for your new user. If successful, the system should respond with “all authentication tokens updated successfully.”

Also try to use a strong password for security purpose

## STEP 3 - Add newly created user to Sudo Group

CentOS has a group called the “wheel” group by default and users added to this group will have sudo privileges automatically. This is an easy way to grant sudo privileges to the newly created user. Just add the new user to the wheel group and the user will be able to carry out operations using root privileges

Type this command as a root user

```
$ visudo
```

visudo is a tool for safely updating the /etc/sudoers file, found in most Linux systems (Ubuntu for example). This is the file that is required for allowing regular users run commands with superuser privileges

The visudo will show the contents of the /etc/sudoers file

Look for the line below

Allows people in group wheel to run all commands

```
%wheel ALL=(ALL) ALL
```

You can see the wheel group is there by default and has permission to run all commands. So lets add our newly created user to the wheel group to be able to run commands with root privileges

```
$ usermod –aG wheel tomiwa
```

The command ‘usermod’ is used to modify or change any attributes of an already created user account via command line.

Options of usermod
The ‘usermod’ command is simple to use with lots of options to make changes to an existing user. Let us see how to use the usermod command by modifying some existing users in the Linux box with the help of following options.

-  -c = We can add a comment field for the user account.
-  -d = To modify the directory for any existing user account.
-  -e = Using this option we can make the account expiry in a specific period.
-  -g = Change the primary group for a User.
-  -G = To add a supplementary group.
-  -a = To add anyone of the group to a secondary group.
-  -l = To change the login name from tecmint to tecmint_admin.
-  -L = To lock the user account. This will lock the password so we can’t use the account.
-  -m = moving the contents of the home directory from existing home dir to new dir.
-  -p = To Use un-encrypted password for the new password. (NOT Secured).
-  -s = Create a Specified shell for new accounts.
-  -u = Used to Assign UID for the user account between 0 to 999.
-  -U = To unlock the user accounts. This will remove the password lock and allow us to use the user account.

The -aG means add a user to group wheel

It can also be

```
$ usermod –a -G wheel tomiwa
```

## STEP 4 - Switch to the Sudo User

Switch to the new (or newly-elevated) user account with the su (substitute user) command:

```
su - tomiwa
```

Enter the password if prompted. The terminal prompt should change to include the UserName in my case tomiwa.
Enter the following command to list the contents of the /root directory:

```
sudo ls -la /root
```

## Alternative: Add User to Sudoers Configuration File

What If there’s a problem with the wheel group, or administrative policy prevents you from creating or modifying groups, you can add a user directly to the sudoers configuration file to grant sudo privileges.

## STEP 1 - Open the sudoers file with visudo

```
visudo
```

This will open the /etc/sudoers file in a text editor.

## STEP 2 - Add the New User to the file

Scroll down to find the following section:

Allow root to run any commands anywhere

```
root ALL=(ALL) ALL
```

Right after this entry, add the following text:

```
tomiwa ALL=(ALL) ALL
```

Replace tomiwa with the username created in step 2
This section should look like the following:

Allow root to run any commands anywhere

```
root ALL=(ALL) ALL
tomiwa ALL=(ALL) ALL
```

Save the file and exit.

## STEP 2 - Test Sudo Privileges for the User Account

Switch user accounts with the su (substitute user) command:

```
su - tomiwa
```

Enter the password for the account, if prompted. The terminal prompt should change to include tomiwa.
List the contents of the /root directory:

```
sudo ls -la /root
```

Enter the password for this user when prompted. The terminal should display a list of all the directories in the /root directory.

## Conclusion

This guide shows you how to give sudo privileges to a user by adding them to the sudo group or editing the sudoers file and adding the user directly on CentOS 7.