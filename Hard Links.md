## Hard Links

A _hard link_ is the file system representation of a file by which more than one path references a single file in the same volume. To create a hard link, use the [**CreateHardLink**](https://learn.microsoft.com/en-us/windows/desktop/api/WinBase/nf-winbase-createhardlinka) function. Any changes to that file are instantly visible to applications that access it through the hard links that reference it. However, the directory entry size and attribute information is updated only for the link through which the change was made. Note that the attributes on the file are reflected in every hard link to that file, and changes to that file's attributes propagate to all the hard links. For example if you reset the READONLY attribute on a hard link to delete that particular hard link, and there are multiple hard links to the actual file, then you will need to reset the READONLY bit on the file from one of the remaining hard links to bring the file and all remaining hard links back to the READONLY state.

For example, in a system where C: and D: are local drives and Z: is a network drive mapped to \\fred\share, the following references are permitted as a hard link:

-   C:\dira\ethel.txt linked to C:\dirb\dirc\lucy.txt
-   D:\dir1\tinker.txt to D:\dir2\dirx\bell.txt
-   C:\diry\bob.bak linked to C:\dir2\mina.txt

The following are not:

-   C:\dira linked to C:\dirb
-   C:\dira\ethel.txt linked to D:\dirb\lucy.txt
-   C:\dira\ethel.txt linked to Z:\dirb\lucy.txt

To delete a hard link, use the [**DeleteFile**](https://learn.microsoft.com/en-us/windows/desktop/api/FileAPI/nf-fileapi-deletefilea) function. You can delete hard links in any order regardless of the order in which they are created.

## [](https://learn.microsoft.com/en-us/windows/win32/fileio/hard-links-and-junctions?redirectedfrom=MSDN#junctions)Junctions

A _junction_ (also called a _soft link_) differs from a hard link in that the storage objects it references are separate directories, and a junction can link directories located on different local volumes on the same computer. Otherwise, junctions operate identically to hard links. Junctions are implemented through [reparse points](https://learn.microsoft.com/en-us/windows/win32/fileio/reparse-points).

Assuming the same conditions in the Hard Links section, the following references are permitted as junctions:

-   C:\dira linked to C:\dirb\dirc
-   C:\dirx linked to D:\diry

The following are not:

-   C:\dira\one.txt linked to C:\dirb\two.txt
-   C:\dir1 linked to Z:\dir2