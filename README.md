SlicerLegacyModules
===================

Overview
--------

A [3D Slicer](http://slicer.org/) extension bundling Slicer modules that used to be distributed
with the Slicer application. Candidates are modules that are either not widly used or now too
specific to be distributed with Slicer.

Available modules
-----------------

* Filtering
  * BlobDetection: Blob Detection based on ITK MultiScaleHessianBasedMeasureImageFilter and HessianToObjectnessMeasureImageFilter.
  * ConnectedComponent: ConnectedComponentImageFilter labels the objects in a binary image. Each distinct object is assigned a unique label. 


How to extract module history from Slicer and include it in this extension ?
----------------------------------------------------------------------------

### Step 1: Install git-rocket-filter

See https://github.com/jcfr/dockgit-rocket-filter

```
curl https://raw.githubusercontent.com/jcfr/dockgit-rocket-filter/master/git-rocket-filter.sh \
  -o ~/bin/git-rocket-filter && \
chmod +x ~/bin/git-rocket-filter
```


### Step 2: Checkout Slicer source

_To avoid loosing pre-existing topics, **do not use** your the Slicer checkout
on which you work on a daily basis_

```
cd /tmp

git checkout git://github.com/Slicer/Slicer SlicerForLegacyModuleExtraction

cd SlicerForLegacyModuleExtraction
```


### Step 3: Extract condidate modules history

Repeat this step for every module history to extract.

```
MODULE=NameOfModule
PATH_TO_KEEP=Modules/CLI/$MODULE   # If different from a CLI, update the path to keep

git checkout master && \
#
# Extract history of $MODULE into branch "module-$MODULE"
#
git-rocket-filter --branch module-$MODULE \
  --keep $PATH_TO_KEEP \
  --detach && \
#
# Checkout "module-$MODULE" branch created in previous step
#
git checkout module-$MODULE && \
#
# Add "NameOfModule: " prefix to all commits of the module and create "module-$MODULE-prefixed" branch.
#
git-rocket-filter --branch module-$MODULE-prefixed \
                  --commit-filter 'commit.Message = "'$MODULE': " + commit.Message;'
```


### Step 4: Integrate history with the SlicerLegacyModules extension

First, add references to `SlicerForLegacyModuleExtraction` local repository:

```
git clone git@github.com:Slicer/SlicerLegacyModules.git
cd SlicerLegacyModules

git remote add local-SlicerForLegacyModuleExtraction file:///tmp/SlicerForLegacyModuleExtraction
git fetch local-SlicerForLegacyModuleExtraction
```

Then, for each module, repeat the following:

```
MODULE=NameOfModule

# Cherry-pick module history
FIRST_COMMIT=$(git rev-list --reverse  local-SlicerForLegacyModuleExtraction/module-$MODULE-prefixed | head -n1)
LAST_COMMIT=$(git rev-list local-SlicerForLegacyModuleExtraction/module-$MODULE-prefixed | head -n1)

git cherry-pick $FIRST_COMMIT
git cherry-pick $FIRST_COMMIT..$LAST_COMMIT

```

Finally, update this extension `CMakeLists.txt` and `README.md` to reference the new modules.
