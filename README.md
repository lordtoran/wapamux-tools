**wapamux-tools** – Script toolchain for mass naming and remuxing of anime and
TV episodes


### Summary ###

wapamux-tools provides a set of handy shell scripts for making the mass naming
and tagging of anime and TV episodes a lot easier.

* wapaname: A semi-automated frontend for tvnamer.
* wapamux: Converts or remuxes (based on an mkvmerge option file) video files
  into a subdirectory.
* wapasplit: Splits a set of video files into subdirectories by metadata CRC.

Output is always in Matroska (.mkv) format.

Note that the scripts are deliberately intended for per-directory
semi-automation only, without a true batch mode. The motivation behind this is
the sensible handling of anime seasons put together from different encodes.


### Prerequisites ###

wapamux-tools should be able to run fine on most POSIX-compatible shells, like
bash, zsh and dash, as long as the dependencies are met. These are:

* tvnamer[1]
* mkvmerge (part of the mkvtoolnix[2] package)
* rsync[3] (preinstalled on just about anything non-embedded)

wapamux-tools has only been tested on Linux so far; BSD/OSX/Android testers
are highly welcome.


### How to install ###

First install the dependencies, if required. Then copy the script files to
somewhere in your path (like ~/bin). Symlinking them will work, too.


### Usage ###

For more detailed information, run the the scripts with the -h or --help
switch.

    wapaname [series_name] [tvdb_series_id]

Auto-renames all TV episodes in the current directory using tvnamer. File
names have to already be in a Kodi-compatible sNNeNN, NNxNN or equivalent
format; just the season and episode number is already sufficient. Should
autodetection by means of the directory name fail, a series name can be
enforced. Should that fail too, a TVDB id can additionally be given.

    wapamux [-s | --scan-headers] [fileext]

Remuxes all files ending in .fileext found in the current directory into a
single subdirectory of a hardcoded name. If an mkvmerge option file is
present in the directory, it will be used for metadata tagging. fileext
defaults to 'mkv'. -s only does a scan for metadata consistency, just as it
would before a remux with an option file.

    wapasplit [-s | --scanonly] [fileext]

Splits all video files ending in .fileext found in the current directory into
subdirectories based on their metadata checksum. This is handy with source
files from different encodes. Files can either be rsync-copied (more secure)
or moved. fileext defaults to 'mkv'. -s only performs the metadata CRCs and
displays which directories would be created.


### FAQ ###

"wapamux"?

It is an allusion to "wapanese", a somewhat derogatory term for a japanophile
person with limited knowledge and a warped notion of Japanese culture and
language. Just in line with that, I was utterly unable to come up with a more
creative pun.


### References ###

[1] https://mkvtoolnix.download
[2] https://github.com/dbr/tvnamer
[3] https://rsync.samba.org/
