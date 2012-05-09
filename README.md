IncogRepo
============================================
*A Personal Revision Control System for Linux*

IncogRepo Is For
----------------
* for people who don't want to use a big revision control system. (Git, Mercurial, SVN)
* for people who have a server with ssh access.
* for people who code in locked down linux environments.
* for people who code solo.

Notice
------
  This system is *not* meant for team development! It is very easy for multiple users to "overwrite" each other's data, even though it is in storage.

Requirements
------------
  php, tar, coreutils, ssh

Setup
-----
1. Download `incogrepo` and put it in the same directory as where your project folders are.
2. Open `incogrepo` with your favorite editor, and edit the `User Configuration` section.
3. Start committing!

Author
------
Josef Patoprsty (Seppi)

Technical Requirements
----------------------
* PHP 5.3.5, but may work with older versions.
* GNU tar 1.25, but may work with other versions.
* GNU coreutils 8.5 (cp, ls, mkdir), but may work with older or other versions.
* OpenSSH client 5.8 (scp), but may work with older or other versions. (May have to modify advanced config to use other clients)

Operands
--------
* `checkin|ci <project>`  
    Archive the project and check it in to the repository.
* `checkout|co <project> [timestamp]`  
    Check out the project, and unarchive the project to the working directory.
    Adding the timestamp flag will unarchive a repository with a specific timestamp. Use status to find previous project's timestamps.
* `status|st <project>`  
    See the status of the current repository.
* `serverinit|si`  
    Initializes the local and remote repository.
    
Tutorial
--------
For those who learn best by example; here is a quick annotated interactive session.

Configure IncogRepo.
<pre>
seppi@home:~/IncogRepo$ nano incogrepo
</pre>
Move over to the remote server and make storage space for the repository.
<pre>
seppi@home:~/IncogRepo$ ssh seppi@remote
seppi@remote's password: 
Welcome to Ubuntu 11.04 (GNU/Linux 2.6.38-11-generic x86_64)
seppi@remote:~$ mkdir IncogRepoStorage
seppi@remote:~$ exit
logout
Connection to remote closed.
</pre>
The remote repository is initialized.
<pre>
seppi@home:~/IncogRepo$ ./incogrepo serverinit
Initialize both local and remote repository? [y|n]:y
Initializing local repository...
Initializing remote repository...
seppi@localhost's password: 
Check for new commits? [y|n]:n
Skipping update.
</pre>
Our project, which is named `test` is checked in.
<pre>
seppi@home:~/IncogRepo$ ls
incogrepo  README  test
seppi@home:~/IncogRepo$ ./incogrepo checkin test/
Downloading newest commits...
seppi@localhost's password: 
Repository has 0 commits...
Repository needs to download 0 commits...
Archiving Project...
test/
test/wat.php
test/README
test/index.html
Checking in archive...
seppi@localhost's password: 
Check in complete...
</pre>
The local repository is deleted to emulate a different computer with the same configuration.
<pre>
seppi@home:~/IncogRepo$ rm ~/.incogrepo/ -rf
</pre>
The project `test` is checked out.
<pre>
seppi@home:~/IncogRepo$ ./incogrepo checkout test
Check for new commits? [y|n]:y
Downloading newest commits...
seppi@localhost's password: 
Repository has 1 commits...
/home/seppi/.incogrepo/test_1317087269.tgz does not exist...
Repository needs to download 1 commits...
New commits found. Download commits? [y|n]:y
seppi@localhost's password: 
Repository updated.
Unarchiving Project...
test/
test/wat.php
test/README
test/index.html
Check out complete.
</pre>
Project `test` has been successfully checked out.
<pre>
seppi@home:~/IncogRepo$ ls *
incogrepo  README
test:
index.html  README  wat.php
</pre>
Status shows us that the `test` project did commit.
<pre>
seppi@home:~/IncogRepo$ ./incogrepo status
Check for new commits? [y|n]:n
Skipping update.
test : Mon, 26 Sep 11 21:34:29 -0400 [1317087269]
</pre>
