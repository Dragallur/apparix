# apparix
Command line directory bookmarks with jumping to bookmarks, subdirectory tab completion, distant listing etc

Apparix is a tiny set of commands implementing directory bookmarking in **bash** and **zsh**.
You just need the file [.bourne-apparix](https://raw.githubusercontent.com/micans/apparix/main/.bourne-apparix).
What apparix provides:

- Bookmark the current directory by issuing `bm foo`. This takes effect instantly
  across all your sessions (it is stored in `$HOME/.apparixrc`).

- Jump to `foo` by issuing\
  `to foo`

- Even better, jump to the subdirectory `barzoodle` of `foo` using\
  `to foo barzoodle`

- Even betterer, use tab completion with subdirectory jumping:\
  `to foo b<TAB>`

- Even even betterer, this works at arbitrary levels:\
  `to foo barzoodle/ti<TAB>`

- No less excellent, there are several *distant* listing/editing commands.
  In all cases, tab completions work on subdirectories and files (below
	is the output of the apparix ahoy helper function):

```
Apparix functions.
             Below all SUBDIR and FILE can be tab-completed.

  bm   MARK               Bookmark current directory as mark
  to   MARK [SUBDIR]      Jump to mark or a subdirectory of mark
--
  als  MARK [SUBDIR] [ls-options]  List mark dir or subdir
  ald  MARK [SUBDIR]      List subdirs of mark dir or subdir
                          ignores hidden directories
  aldr MARK [SUBDIR]      Like ald, recursively
  amd  MARK [SUBDIR] [mkdir options] Make dir in mark
  a    MARK [SUBDIR/]FILE Echo the true location of file, useful
                 e.g. in: cp file $(a mark dir/file.txt)
--
  aget MARK [SUBDIR/]FILE Copy file to current directory
  aput MARK [SUBDIR] -- FiLE+   Copy files to mark (-- required)
--
  ae MARK [SUBDIR/]FILE [editor options] Edit file in mark
  av MARK [SUBDIR/]FILE [editor options] View file in mark
--
  amibm                   See if current directory is a bookmark
  bmgrep PATTERN          List all marks where target matches PATTERN
--
  agather MARK            List all targets for bookmark mark
  whence MARK             Menu selection for mark with multiple targets
--
  todo MARK [SUBDIR]      Edit TODO file in mark dir
  rme MARK [SUBDIR]       Edit README file
  portal                  current directory subdirs become mark names
  portal-expand           Re-expand all portals
  aghast MARK [SUBDIR/]FILE [dummy options] testing the apparix muxer
--
  Where options passing is indicated above:
   - The sequence has to start with a '-' or '+' character.
   - Multiple options with arguments can be passed.
   - FWIW Arguments with spaces in them seemed to work under limited
     testing, e.g. ae pl main.nf '+set paste'
```

Tab completion with apparix works best, IMHO, with cyclic tab completion. This
is activated by the line `TAB: menu-complete` in the file `$HOME/.inputrc` (and you may
need as well put `INPUTRC=$HOME/.inputrc` in `$HOME/.bashrc`), or the
line `bind '"\t":menu-complete'` in `$HOME/.bashrc`. 

There are two asymmetries between `aget` and `aput`. The former can only
retrieve a single file, but tab completion on the (distant) file to be copied
works. The latter can copy over multiple files, but tab completion is bound
to the target and hence does not work on the files to be copied. For other cases
just work with `$(a mark dir)`. This can be combined with globbing, as in

```
cp $(a mrk)/*.txt .
```

Many thanks to Sitaram Chamarty for the original idea of sub-directory
completion and the first bash implementation thereof, and to Izaak van Dongen
for the zsh completion code and complete overhaul and improvement of the bash
completion code.  Thanks to Martin Zuther
there is a cool [**fish** implementation](https://github.com/mzuther/appari-fish),
named appari-fish.


## Apparix/apparish developments, past and ongoing

In the beginning (2005-ish) the system was called Apparix. It was
implemented in C and shipped with bash wrapper functions and completion code.
The C code was, in hindsight, a slightly heavy hammer (although it must consume
many fewer CPU cycles). A simple bash shell reimplementation was undertaken
many years later, around 2018 and published in the `micans/bash-utils`
repository.  Izaak thought of the name apparish and added zsh code and
additionally contributed a thorough rewrite of the bash completion layer (more
about that below).

Martin added appari-fish to the family. The valley was peaceful for a
while.  Early 2021 I realised that I still think of the valley as apparix, so
I've tweaked documentation and renamed files to revert back to apparix, followed
by moving apparix to its own repository.

The glorious shell code itself is still apparish-rich. Stay tuned for further
naming shenanigans. More relevant, the bash helper functions are still
undergoing little tweaks and improvements every now and then. When this happens
the functionality is described and added to the list of example commands at the top.

A bleedinge edge fork of apparix lives in [this repo](https://github.com/goedel-gang/bash-utils/),
supporting among other things newlines in the directory name that a bookmark
points to.  For various reasons (my dinosaur habits among them) that code did
not make it into this repository. The main reason you might want to use the
dinosaur code here is that I'm a pretty heavy apparix user and add and change
things to improve the distant directory experience. When something becomes too
gnarly I tend to ask Izaak anyway. Happy to'ing!

