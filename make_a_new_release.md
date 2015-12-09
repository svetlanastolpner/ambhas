#Describe how to make a new release of the library

# Make a new release #

To create the x.y.z :
First, put the entry for the x.y.z release in the CHANGELOG.txt file in the tags/x.y.z folder.
```
svn co https://ambhas.googlecode.com/svn/tags tags
svn export https://ambhas.googlecode.com/svn/trunk/ x.y.z
mv x.y.z tags/
svn add tags/x.y.z
svn ci tags/x.y.z -m "Creating the x.y.z tag"
```

Now update the version number (x.y.z) in setup.py in the tags/x.y.z folder.

Check in the changes:
```
svn ci tags/x.y.z -m "Final changes for the x.y.z release"
```

Now create the release:
```
cd tags/x.y.z
python setup.py sdist bdist_egg register upload
```