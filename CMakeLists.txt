cmake_minimum_required(VERSION 3.5)

project(SlicerLegacyModules)

#-----------------------------------------------------------------------------
# Extension meta-information
set(EXTENSION_HOMEPAGE "http://slicer.org/slicerWiki/index.php/Documentation/Nightly/Extensions/SlicerLegacyModules")
set(EXTENSION_CATEGORY "Legacy")
set(EXTENSION_CONTRIBUTORS "Slicer Community")
set(EXTENSION_DESCRIPTION "Collection of legacy Slicer modules")
set(EXTENSION_ICONURL "http://www.example.com/Slicer/Extensions/SlicerLegacyModules.png")
set(EXTENSION_SCREENSHOTURLS "http://www.example.com/Slicer/Extensions/SlicerLegacyModules/Screenshots/1.png")
set(EXTENSION_DEPENDS "NA") # Specified as a space separated string, a list or 'NA' if any

#-----------------------------------------------------------------------------
# Extension dependencies
find_package(Slicer REQUIRED)
include(${Slicer_USE_FILE})

#-----------------------------------------------------------------------------
# Extension modules
add_subdirectory(Modules/CLI/BlobDetection)
add_subdirectory(Modules/CLI/ConnectedComponent)
## NEXT_MODULE

#-----------------------------------------------------------------------------
include(${Slicer_EXTENSION_GENERATE_CONFIG})
include(${Slicer_EXTENSION_CPACK})
