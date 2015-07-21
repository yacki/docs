How to repair a svn server
====

Developpers reported that they cannot commit/update from svn server sometimes.

I logged in to console and executed a command to check svn server:

      svn verify my_repo_name/ 

It reported an error:

      * Verified revision 21416.
      * Verified revision 21417.
      * Verified revision 21418.
      * Verified revision 21419.
      * Verified revision 21420.
      * Verified revision 21421.
      * Verified revision 21422.
      svnadmin: E160004: Filesystem is corrupt
      svnadmin: E200014: Checksum mismatch while reading representation:
         expected:  a109ad6f30aeb69cb2ba124017dfd5c3
         actual:  a2b753837dd0bc0285eb3c56f25a55b2

Version 21423 is corrupt for some unknown reason.

### First, I export two parts of the svn:
      svnadmin dump my_repo/ > ~/svn_20150721/my_repo.dump
It will stop at version 21422, because 21423 is corrupted.

### Then I export the tail of the svn:
      svnadmin dump my_repo/ -r 21424:HEAD --incremental > ~/svn_20150721/my_repo_21422_21546.dump

Now I have 2 dump files now, the first 
### After this, I created a new repo:
      svnadmin create newrepo

### Then import first dump file:
      svnadmin load --bypass-prop-validation newrepo/ < ~/svn_20150721/my_repo.dump 

Now I downloaded the svn to my Windows computer, and commit a test file to server.
So the latest version is 21423 now.

Then I import the sencond dump file:
            svnadmin load --bypass-prop-validation newrepo/ < ~/svn_20150721/my_repo_21422_21546.dump 
