#Using the svn to mange the subversion

# Initial upload your filed into the repository #

svn import /path/to/local/folder http://ambhas.googlecode.com/svn/ -m "your message"

# Make a working copy of the repository #
svn checkout http://ambhas.googlecode.com/svn/trunk/ ambhas

# Add a new file/folder to repository #
svn add folder/file\_name

# Publish your changes to others #
svn commit -m "your message"

# See the status of your project #
svn status

This will print the list of the files/folders with their status. Following is the description of the codes given by Subversion:
  * No modifications
  * A Added
  * C Conflicted
  * D Deleted
  * G Merged
  * I Ignored
  * M Modified
  * R Replaced
  * X Unversioned, used as external
  * ? Unversioned
  * ! Missing
  * - Obstructed by another item

# Information about a working copy #
svn info path/to/repository

# SVN behind Proxy #
Go to the ~/.subversion/servers and update the relevant entries.