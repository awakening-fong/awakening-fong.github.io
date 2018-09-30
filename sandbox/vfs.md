---
title: 草稿:VFS
date: '2018-07-29'
tags: [file system]
categories: 文件系统
---


http://wiki.old.lustre.org/images/d/da/Understanding_Lustre_Filesystem_Internals.pdf

A process maintains a list of associated open files. Each open file has an in-memory
representation known as the file object. It stores the information necessary to interact
between an open file and process. To the userland, this is presented by a file handle,
fd. This data structure contains a field, f_op, that acts like a switchboard or pointer to
a method table that provides functions specific to each filesystem. 


Another important field defined in the file structure is f_dentry, which points to a
dentry object (struct dentry) stored in a dentry cache, also known as the dcache.
Essentially, VFS will create a dentry object the first time a file or directory is about
to be accessed. If this is a non-existent file/directory, then a negative dentry will be created. As an example, take the following pathname: /home/bob/research08; it is composed of four path components: /, home, bob, and research08. Correspondingly, the path lookup will create four dentry objects for each component. Each dentry
object associates the respective component with its inode through field d_inode.



The inode object stores information about a specific file, uniquely identified by an
inode number. ULK32 has exhaustive listings of field definitions for inode structure.
What is important to know is that (1) i_sb points to the VFS superblock; (2) i_op is
the switchboard for inode operations such as:
	create(dir, dentry, mode, nameidata)
	mkdir(dir, dentry, mode)
	lookup(dir,dentry,nameidata)
	...


	
VFS layer defines a generic superblock object (struct super_block), which stores
information about the mounted filesystem. One particular field s_fs_info points to
superblock information that belongs to a specific filesystem. 


direct io
read ahead
