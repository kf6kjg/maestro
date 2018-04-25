Maestro expected a certain structure on both the central "Grid Share" shared folder as well as in the regions.  This document is a record of what folder structures are expected.

Maestro's RegionHost polls each service, grid and regions, to see if the process ID is active.  If it is not but is supposed to be, then it queues up a task to restart that process.
NOTE: To me this should be an event-driven model not a polling: problems with temporal collision in internal restart.  Not a problem for the Windows-based process, but under Linux where I've got it maintained as a real service it would be - along with a lot of other issues with Maestro.

# Configuration

Maestro expects the configuration file to be named `maestro.config` and to be located in one of the following folders:
* `~/Inworldz Maestro 1.0/`
* `%USER%/AppData/Roaming/Inworldz Maestro 1.0/`

NOTE: A part of the code, `server.py`, allows for a commandline parameter `propfile` to set the path, but doesn't look well supported across the system...  I could be wrong.

* Section `maestro`
    * `gridshare_path` - Path to where the share will be mounted by Maestro.
    * `gridshare_user` - User on the remote server under which to mount the share.
    * `gridshare_pass` - Password for the user on the remote server under which to mount the share.
    * `template_basedir`
    * `basedir` - 

# "Grid Share"

The "Grid Share" is a shared folder that stores all available revisions of the service files.

It will be automatically mounted by Maestro when needed.  NOTE: This system is one of the features that locks Maestro into Windows-only land.

## Configuration options

All options are in the `maestro` section of the config file.

* `gridshare_path` - Path to where the share will be mounted by Maestro.
* `gridshare_user` - User on the remote server under which to mount the share.
* `gridshare_pass` - Password for the user on the remote server under which to mount the share.
* `template_basedir` - Default of `[AppData]\maestro\data\templates`.  Path to where the share will be mounted.


## Filesystem paths

In the share folder the following structure is expected:

* `_New_Region`
    * `Template` - Guessing that this contains all the config stuff.
        * `region`
            * `region-default.xml`
        * `configs`
            * `*.tmpl`
* `Grid` - A folder containing the following:
    * Revision folders of format `R*[0-9]` - These contain all the binaries from each compile.
* `simulation.config` - Region configuration options. Contains a single section: `simulation`

Note the reliance on all services and config files for such being located in the bin/revision folders.  This is a blocker for container-based or even separated service based deployments.

# Regions and services folders

The regions and services folder is where all the regions, and (if applicable) services, on a given host are located.

## Configuration options

All options are in the `maestro` section of the config file.

* `max_region_slots` - How many regions should be allowed on this host?
* `basedir` - Path to the folder where the directories for services and regions are to be located.
* `region_leaf_base` - First part of folder name for each region.
* `` - 
* `` - 

## Filesystem paths

Any part of a path or name in brackets is a variable replacement.

* `[basedir]` - Folder containing all the regions and services.
    * `[region_leaf_base]` - The first (0th) region slot folder.
    * `[region_leaf_base] - [slot#]` - Region slots 1 through N.
        * `bin` - Where all the binaries are.
            * `Regions` - Place for region XML files.  There will only be one XML file.
    * `GridServices` - The folder for all grid services.
        * `bin` - Where all the binaries are.

