---
layout: default
title: Linux Permission
---

# Linux Permission

## Linux Files and File Permission

Linux files are setup so access to them is controlled. There are three types of access:

- read
- write
- execute

Each file belongs to a specific user and group. Access to the files is controlled by user, group, 
and what is called other. The term, other, is used to refer to someone who is not the user (owner) 
of the file, nor is the person a member of the group the file belongs to. When talking about setting 
permissions for "other" users to use, it is commonly referred to as setting the world execute, read, 
or write bit since anyone in the world will be able to perform the operation if the permission is 
set in the other category.

### File names and permission characters

File names can be up to 256 characters long with `-`, `_`, and `.` characters along with letters and 
numbers.

When a long file listing is done, there are 10 characters that are shown on the left that indicate 
type and permissions of the file. File permissions are shown according to the following syntax 
example: drwerwerwe

There are a total of 10 characters in this example, as in all Linux files. The first character indicates 
the type of file, and the next three indicate read, write, and execute permission for each of the three 
user types, user, group and other. Since there are three types of permission for three users, there 
are a total of nine permission bits. The table below shows the syntax:

| 1    | 2    | 3           | 4       | 5     | 6           | 7       | 8     | 9           | 10      |
|------|------|-------------|---------|-------|-------------|---------|-------|-------------|---------|
| File | User | Permissions          || Group | Permissions          || Other | Permissions          ||
| Type | Read | Write       | Execute | Read  | Write       | Execute | Read  | Write       | Execute |
| d    | r    | w           | e       | r     | w           | e       | r     | w           | e       |

  * Character 1 is the type of file: - is ordinary, d is directory, l is link.
  * Characters 2-4 show owner permissions. Character 2 indicates read permission, character 3 indicates write permission, and character 4 indicates execute permission.
  * Characters 5-7 show group permissions. Character 5=read, 6=write, 7=execute
  * Characters 8-10 show permissions for all other users. Character 8=read, 9=write, 10=execute

There are 5 possible characters in the permission fields. They are:

  * r = read - This is only found in the read field.
  * w = write - This is only found in the write field.
  * x = execute - This is only found in the execute field.
  * s = setuid - This is only found in the execute field.
  * If there is a "-" in a particular location, there is no permission. This may be found in any field whether read, write, or execute field.

### Examples 

Type `ls -l` and a listing like the following is displayed:

```bash
total 10						
drwxrwxrwx	4	george	team1	122	 Dec 12 18:02	Projects
-rw-rw-rw-	1	george	team1	1873	Aug 23 08:34	 test
-rw-rw-rw-	1	george	team1	1234	 Sep 12 11:13	datafile
```

Which means the following:

| Permission field | Links | Owner  | Group | Bytes | modification            |
|------------------|-------|--------|-------|-------|-------------------------|
| drwxrwxrwx       | 4     | george | team1 | 122   | Dec 12 18:02   Projects |

The fields are as follows:

  - Type field: The first character in the field indicates a file type of one of the following:
    * d = directory
    * l = symbolic link
    * s = socket
    * p = named pipe
    * - = regular file
    * c= character (unbuffered) device file special
    * b=block (buffered) device file special
  - Permissions are explained above.
  - Links:	The number of directory entries that refer to the file. In our example, there are four.
  - The file's owner in our example is George.
  - The group the file belongs to. In our example, the group is team1.
  - The size of the file in bytes
  - The last modification date. If the file is recent, the date and time is shown. If the file is not in the current year, the year is shown rather than time.
  - The name of the file.

**Set User Identification Attribute**

The file permissions bits include an execute permission bit for file owner, group and other. When the execute bit for the owner is set to "s" the set user ID bit is set. This causes any persons or processes that run the file to have access to system resources as though they are the owner of the file. When the execute bit for the group is set to "s", the set group ID bit is set and the user running the program is given access based on access permission for the group the file belongs to. The following command:

```bash
chmod +s myfile
```

sets the user ID bit on the file "myfile". The command:

```bash
chmod g+s myfile
```

sets the group ID bit on the file "myfile".

The listing below shows a listing of two files that have the group or user ID bit set.

```bash
-rws--x--x   1 root    root    14024 Sep  9 1999 chfn
-rwxr-sr-x   1 root   mail    12072 Aug 16 1999 lockfile
```

The files chfn and lockfile are located in the directory "/usr/bin". The "s" takes the place of the 
normal location of the execute bit in the file listings above. This special permission mode has no 
meaning unless the file has execute permission set for either the group or other as well. This means 
that in the case of the lockfile, if the other users (world execute) bit is not set with permission 
to execute, then the user ID bit set would be meaningless since only that same group could run the 
program anyhow. In both files, everyone can execute the binary. The first program, when run is executed 
as though the program is the root user. The second program is run as though the group "mail" is the 
user's group.

For system security reasons it is not a good idea to set many program's set user or group ID bits any 
more than necessary, since this can allow an unauthorized user privileges in sensitive system areas. 
If the program has a flaw that allows the user to break out of the intended use of the program, then 
the system can be compromised.

**Directory Permissions**

There are two special bits in the permissions field of directories. They are:

  * s - Set group ID
  * t - Save text attribute (sticky bit) - The user may delete or modify only those files in the directory that they own or have write permission for.

### Save text attribute

The /tmp directory is typically world-writable and looks like this in a listing:

```bash
drwxrwxrwt   13 root     root         4096 Apr 15 08:05 tmp
```

Everyone can read, write, and access the directory. The "t" indicates that only the user (and root, of course) 
that created a file in this directory can delete that file. 

To set the sticky bit in a directory, do the following:

```bash
chmod +t data
```

This option should be used carefully. A possible alternative to this is

  - Create a directory in the user's home directory to which he or she can write temporary files.
  - Set the TMPDIR environment variable using each user's login script.
  - Programs using the tempnam(3) function will look for the TMPDIR variable and use it, instead of writing to the /tmp directory.

### Directory Set Group ID

If the setgid bit on a directory entry is set, files in that directory will have the group ownership as the directory, 
instead of than the group of the user that created the file. 

This attribute is helpful when several users need access to certain files. If the users work in a directory 
with the setgid attribute set then any files created in the directory by any of the users will have the 
permission of the group. For example, the administrator can create a group called spcprj and add the users 
Kathy and Mark to the group spcprj. The directory spcprjdir can be created with the set GID bit set and Kathy 
and Mark although in different primary groups can work in the directory and have full access to all files in 
that directory, but still not be able to access files in each other's primary group.

The following command will set the GID bit on a directory:

```bash
chmod g+s spcprjdir
```

The directory listing of the directory "spcprjdir":

```bash
drwxrwsr-x 2 kathy spcprj 1674 Sep 17 1999 spcprjdir
```

The "s'' in place of the execute bit in the group permissions causes all files written to the 
directory "spcprjdir" to belong to the group "spcprj" .

**Examples**

| **Below are examples of making changes to permissions**           |  |
|--------------------|----------------------------------------------|--|
| chmod u+x myfile   | Gives the user execute permission on myfile. |
| chmod +x myfile    | Gives everyone execute permission on myfile. |
| chmod ugo+x myfile | Same as the above command, but specifically specifies user, group and other. |
| chmod 400 myfile   | Gives the user read permission, and removes all other permission. These permissions are specified in octal, the first char is for the user, second for the group and the third is for other. The high bit (4) is for read access, the middle bit (2) os for write access, and the low bit (1) is for execute access. |
| chmod 764 myfile   | Gives user full access, group read and write access, and other read access. |
| chmod 751 myfile   | Gives user full access, group read and execute permission, and other, execute permission. |
| chmod +s myfile    | Set the setuid bit. |
| chmod go=rx myfile | Remove read and execute permissions for the group and other. |

| **Below are examples of making changes to owner and group**            | |
|------------------|-------------------------------------------------------|
| chown mark test1 | Changes the owner of the file test1 to the user Mark. |
| chgrp mark test1 | Changes the file test1 to belong to the group "mark". |

**Note:**
Linux files were displayed with a default tab value of 8 in older Linux versions. That means that file names longer than 8 may not be displayed fully if you are using an old Linux distribution. There is an option associated with the ls command that solves this problem. It is "-T". Ex: "ls al -T 30" to make the tab length 30.

### Umask Settings

The umask command is used to set and determine the default file creation permissions on the 
system. It is the octal complement of the desired file mode for the specific file type. Default 
permissions are:

  * 777 - Executable files
  * 666 - Text files

These defaults are set allowing all users to execute an executable file and not to execute a 
text file. The defaults allow all users can read and write the file.

The permission for the creation of new executable files is calculated by subtracting the umask 
value from the default permission value for the file type being created. An example for a text 
file is shown below with a umask value of 022:

```bash
666 Default Permission for text file
-022 Minus the umask value
-----
644 Allowed Permissions
```

Therefore the umask value is an expression of the permissions the user, group and world will not 
have as a default with regard to reading, writing, or executing the file. The umask value here 
means the group the file belongs to and users other than the owner will not be able to write to 
the file. In this case, when a new text file is created it will have a file permission value of 
644, which means the owner can read and write the file, but members of the group the file belongs 
to, and all others can only read the file. A long directory listing of a file with these permissions 
set is shown below.

```bash
-rw-r--r--   1 root     workgrp          14233 Apr  24 10:32 textfile.txt
```

A example command to set the umask is:

```bash
umask 022
```

The most common umask setting is 022. The /etc/profile script is where the umask command is 
usually set for all users. 

Red Hat Linux has a user and group ID creation scheme where there is a group for each user and 
only that user belongs to that group. If you use this scheme consistently you only need to use 
002 for your umask value with normal users.
