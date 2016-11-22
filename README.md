**wapamux-tools** – Script toolchain for mass naming and remuxing of anime and
TV episodes


### Summary ###

wapamux-tools provides a set of handy shell scripts for making the mass naming
and tagging of anime and TV episodes a lot easier.

* **wapaname**: A semi-automated frontend for tvnamer.
* **wapamux**: Converts or remuxes (based on an mkvmerge option file) video
  files into a subdirectory.
* **wapasplit**: Splits a set of video files into subdirectories by metadata
  CRC.

Output is always in Matroska (.mkv) format.

Note that the scripts are deliberately intended for per-directory
semi-automation only, without a true batch mode. The motivation behind this is
the sensible handling of anime seasons put together from different encodes.


### Prerequisites ###

wapamux-tools should be able to run fine on most POSIX-compatible shells, like
bash, zsh and dash, as long as the dependencies are met. These are:

* **tvnamer**[[1]]
* **mkvmerge** (part of the mkvtoolnix[[2]] package)
* **rsync**[[3]]—preinstalled on just about anything non-embedded

wapamux-tools has only been tested on Linux so far; BSD/OSX/Android testers
are highly welcome.


### Installation ###

First install the dependencies, if required. Then copy the script files to
somewhere in your path (like ```~/bin```) and ```chmod +x``` if necessary.
Symlinking them will work too.


### Usage ###

For more detailed information, run the the scripts with the ```-h``` or
```--help``` switch.

    wapaname [series_name] [tvdb_series_id]

Auto-renames all TV episodes in the current directory using tvnamer. File
names have to already be in a Kodi-compatible ```sNNeNN```, ```NNxNN``` or
equivalent format; just the season and episode number is already sufficient.
Should autodetection by means of the directory name fail, a series name can be
enforced. Should that fail too, a TVDB ID can additionally be given.

    wapamux [-s | --scan-headers] [fileext]

Remuxes all files ending in ```.fileext``` found in the current directory into
a single subdirectory of a hardcoded name. If an mkvmerge option file is
present in the directory, it will be used for metadata tagging. ```fileext```
defaults to ```mkv```. ```-s``` only does a scan for metadata consistency,
just as it would before a remux with an option file.

    wapasplit [-s | --scanonly] [fileext]

Splits all video files ending in ```.fileext``` found in the current directory
into subdirectories based on their metadata checksum. This is handy with
source files from different encodes. Files can either be rsync-copied (more
secure, plus resumeable) or moved. ```fileext``` defaults to ```mkv```.
```-s``` only performs the metadata CRCs and displays which directories would
be created.


### Example workflow ###

In a common scenario ```wapaname``` would be called after naming the video
files in a way that tvnamer can parse (one series/season per directory, which
is named after the series, sNNeNN or NNxNN file numbering). The result would
be numbered video files with episode names.

Next a metadata consistency check can be done using ```wapasplit -s``` or
```wapamux -s```. If the metadata turns out inconsistent across files, one can
either manually perform multiple wapamux passes or let wapasplit take care of
creating a directory subtree and copying/moving the files there.

Finally one would edit the header metadata as needed for each subset (usually
by means of ```mkvtoolnix-gui```), export the changes as option files, then
remux using wapamux. There is nothing to worry about filenames stored in the
option file(s), by the way—wapamux will substitute them with the currently
processed file during remuxing.


### FAQ ###

*“wapamux”?*

It is an allusion to “wapanese”, a somewhat derogatory term for a japanophile
person with limited knowledge and a warped notion of Japanese culture and
language. Just in line with that, I was utterly unable to come up with a more
creative pun.

*When remuxing, can I also move tracks around or remove some of them in
addition to setting header data?*

wapamux uses mkvmerge as its backend so this is perfectly fine. The option
file (series.opt) that keeps track of the changes is not touched except for
filename sanitation required for batch mode.

*wapaname errors out with “tvnamer: error: No valid files were supplied”*

Either you tried running the script on a movie, or tvnamer is unable to match
the episode numbers in the filenames. This can sometimes happen with unicode
characters around or next to episode numbers, for example if a
(typographically correct) dash is used in place of a minus sign. Filename
matching issues should be reported directly to tvnamer's issue tracker[[4]].

*wapaname defaults to English episode names when I force a series name or ID*

tvnamer behaves like this when no language is set in its configuration. You
can create a default configuration file using

    mkdir -p ~/.config/tvnamer && tvnamer -s ~/.config/tvnamer/tvnamerrc

wapaname will always use that file as a tvnamer configuration if it exists.
Minimal example file:

    {
        "language": "de", 
        "search_all_languages": true, 
        "order": "dvd" 
    }

Here, German is defined as the default language and episode order is set to
DVD order instead of the default “first aired” order.

*wapaname automatically selects the English dub of a series even when the
language code is set in the configuration file*

Try passing the series name and ID directly to wapaname, as in:

    wapaname "Some anime series" 123456

This should make tvnamer auto-select the correct dub in most cases.


[1]: https://github.com/dbr/tvnamer

[2]: https://mkvtoolnix.download

[3]: https://rsync.samba.org/

[4]: https://github.com/dbr/tvnamer/issues
