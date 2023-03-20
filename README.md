# Permissions

## Learning goals

- Learn how permissions work in *Linux* systems
- Learn how to use the `chown` command to modify ownership
- Learn how to use the `chmod` command to modify permissions

## Introduction

In the last chapter, we went over how users and groups are created and the *why* behind their usage. In this chapter, we will be looking over how we can view and manipulate permissions for files and directories, which work in conjunction with users and groups.

## Permissions

Permissions in *Linux* are exactly what they sound like: they control access to files and directories. Every file and directory has their own set of permissions that determine who can read, write, and execute them. These can fall under both user and group permissions.

You can view the permisions of the files in a directory using the `ls -l` command:

```bash
$ ls -l
-rw-r--r-- 1 john students  3077 Feb 12 03:22 README.md
-rwxr-xr-x 1 john students   168 Feb  8 22:38 script.sh
```

The important bit is that first section per-file that reads like gibberish until you understand what it means:

```bash
-rwxr-xr-x
```

The first character, `-`, indicates that it is a regular file; if the first character were a `d`, it would mean it is a directory. 

The rest is split up into three sets of three characters: `rwx`, `r-x`, and `r-x`. These represent what the permissions are for the owner user, group, and all other users respectively, when it comes to this file. 

The first character is whether the user can *read* the file (can access the file and read the contents), the second character is whether the user can *write* to the file (edit and save the file, overwriting it), and the third character is whether it is *executable* or not (can be ran as a program/script). 

If it has a `-` on any given field, it just means that field is empty.

Let's take a look at the commands we have available to modify permissions and ownership!

## `chown`

You can use the `chown` command to **ch**ange the **own**ership or a file or directory. You can change the owning user as well as the owning group.

If you want to change the owning user of a file, you can do it like so:

```bash
$ sudo chown <user> <file/directory>
$ sudo chown john test_file.txt
```

If you want to change the owning user of a directory, you pass in the `-R` (recursive) flag:

```bash
$ sudo chown -R john test_folder
```

If you want to change the owning user *and/or* group, then you use the `:group` syntax:

```bash
$ sudo chown john:students myfile.txt
$ sudo chown :students myfile.txt
```

## `chmod`

The `chmod` command is a command you can use to change the permissions of files and directories. This command modifies the read/write/execute permissions we went over in the previous section.

You can use the `chmod` command in the following manner:

```bash
$ sudo chmod <permissions> <file/directory>
$ sudo chmod 755 test_file.txt
```

The way permissions are defined when using the `chmod` command are numeric, and it can be a bit tricky to understand at first.

When using numbers to define permissions, we split up the sequence into three like we do when reading permissions: the first digit for owner, the second digit for group, and the third digit for everyone else.

The way you calculate each digit is actually pretty straightforward: simply add up the permissions for each user.

Here's what the values are worth for each permission type:

| Permission 	| Value 	|
|------------	|-------	|
| read       	| 4     	|
| write      	| 2     	|
| execute    	| 1     	|

So if you want to set all permissions, you would add the values up to `7`. If you want only *read* permissions, the value would be `4`. if you want *read* and *execute*, the value would be `5`.

If you want everyone to be able to *read*, *write*, and *execute* a file, you would run the following command:

```bash
$ sudo chmod 777 /path/to/script.sh
```

If you only want the owner to be able to read and write to the script, but want everyone else to be able to execute it, you would run the following command:

```bash
$ sudo chmod 755 /path/to/script.sh
```

## Conclusion

That's it! Having a good handle on groups and permissions is essential to not only managing *Linux* systems, but also for troubleshooting access errors and other types of issues. It's worth mentioning that you don't actually need to run `chown`/`chmod` with `sudo`; if the user you are currently logged-in as is the current owner, it'll most likely work. But in most cases you won't be, so you will see the commands ran with `sudo` the majority of the time.
