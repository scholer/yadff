# yadff
Yet another duplicate file finder - yadff. Find (and optionally delete) duplicate files. Written in modern, PEP8-compliant Python, and designed to work nicely with the standard gnu/unix tool chain.


Features and design goals:
* Input directories and files can be given either as command line arguments or through stdin.
* Works well with the standard gnu/unix tool chain: ``find . -type f -size +10M -exec yadff -d -i {} \+`` or ``find . -type f -size +10M |  yadff --skip 1 | xargs rm -v {} \;`` will work equally well. 
  * Note: yadff includes its own rudimentary filters that can be used to include/exclude files, if the unix toolchain is not available – e.g. on the Windows command line.
* Compares files in multiple rounds, typically: (1) file size, (2) file change timestamp, (3) MD5/SHA of first N number of bytes, (4) Full MD5/SHA. Comparisons can be added, deleted and ordered as desired.
* "Invert result" feature - instead of showing file duplications, show files that are unique.
* "Directory mirroring comparison" - Only consider a files to be duplicated if present in all the given folders.
* Order matters: The output order must match input order (lexiographically sorted for subdirectories and files).

This program/library is not designed for:
* yadff is not designed for finding files that are similar but not identical. yadff will only find files that are exactly the same.
* yadff is not a file synchronization tool. Use ``rsync`` or similar for that.

Command line args:
```
  -s, --skip N    Skip the first N duplicates.
  --last N        Print only the last N duplicates. Is applied AFTER --first, but BEFORE --skip.
  --first N       Print only the first N duplicates. Is applied BEFORE --skip and before --AFTER.
  --comparisons   [function_name1, ...]
                  Define which functions are applied to determine identical files.
                  
  -m, --mirroring Switch to "directory mirroring comparison" mode. In "directory mirroring comparison" mode,
                  a file can only be a duplicate if it is present in ALL the directories given as input.
                  Consider ``yadff -m directoryA directoryB``.
                  Say directoryA/file1a and directoryA/file1b are identical. 
                  However, since directoryB does not contain this file, file1a and file1b are not considered to be duplicates.
                  Directory mirroring comparison mode is often used in conjunction with --invert,
                  with the result that files that ARE NOT DUPLICATED are printed as output.
                  For instance, let's say you have two USB drives which appears to contain almost the same files, 
                  but the file hierarchy is different, so you cannot simply do an `rsync` mirroring.
                  Instead, you can use `yadff -m -i usbA usbB` to see which files are missing from one or the other drive.

--oneway <base> Switch to "one-way" comparison mode. 
                  Files are only considered duplicates if they are present in <base> directory (or any subdirectory).

```


Prior art:
-----------

Open Source:
* https://github.com/adrianlopezroche/fdupes - C program
* https://github.com/jbruchon/jdupes - fork of fdupes
* https://github.com/kassoulet/UltraFastDuplicateFilesFinder - Python, 
* https://github.com/michaelkrisper/duplicate-file-finder
* https://github.com/keul/PyDirDuplicateFinder
* https://github.com/NunoMCSilva/DuplicateFinder


Other Apps:
* Disk Drill, Duplicate Finder - https://www.cleverfiles.com/find-duplicates-mac.html

Other guides and refs:
* http://lifehacker.com/the-best-duplicate-file-finder-for-mac-1696993702 - Mentions Gemini, dupeGuru, Tidy Up 4, Chipmunk, The Duplicate Finder, CCleaner.

