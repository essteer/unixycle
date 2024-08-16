# UNIXycle :recycle:

A recycle bin tool for UNIX.

## Documentation

UNIXycle provides two scripts — `recycle` & `restore` — for use in removing files to a recycle bin in the user's `$HOME` directory from where they can later be restored.

### recycle :recycle:

The `recycle` script creates a `recyclebin` directory and `.restore.info` file in the user's `$HOME` directory (if they do not already exist).

When passed file names as arguments, `recycle` moves those target files from their current location to the `recyclebin` with each the unique inode number of each file appended to its name.

Recycled files are recorded in `.restore.info` with their name in `recyclebin` and original absolute file path.

The `recycle` script accepts optional arguments as follows:

| Option | Mode        | Description                                                                     |
| :----: | ----------- | ------------------------------------------------------------------------------- |
|  `-a`  | ASCII       | Displays... ASCII art.                                                          |
|  `-i`  | interactive | Issues a prompt before recycling each file.                                     |
|  `-r`  | recursive   | Accepts directories to recycle their file contents, then deletes the directory. |
|  `-v`  | verbose     | Outputs a confirmation message for each recycled file.                          |

File names may include either the absolute or relative paths to the files, and may include wildcard characters to delete multiple files with similar names.

Example — delete a single file in interactive and verbose modes:

```bash
$ bash recycle -iv demo.txt
recycle 'demo.txt'? y/n y
recycled 'demo.txt'
```

Example — delete multiple files:

```bash
$ bash recycle -iv demo.txt example.doc
recycle 'demo.txt'? y/n y
recycled 'demo.txt'
recycle 'example.doc'? y/n n
```

Example — delete files with wildcard characters and directory contents with recursion:

```bash
$ bash recycle -i -r -v *.txt demo_directory
recycle 'demo.txt'? y/n y
recycled 'demo.txt'
recycle 'example.txt'? y/n y
recycled 'example.txt'
recycle 'demo_directory'? y/n y
recycled 'demo_directory/note1.txt'
recycled 'demo_directory'
```

### restore :cyclone:

The `restore` script accepts a single file name as an argument.

If the file exists in the `recyclebin`, it is restored to its original location — with parent directories created as needed if they no longer exist.

Note that the `recycle` script appends each file's inode number to the file name when moving them to the `recyclebin`. Check the contents of the `recyclebin` to determine the name of the recycled file to be restored.

Example — restore a previously deleted file:

```bash
$ bash restore demo.txt_64135
```

### .restore.info

This hidden file is created in the user's `$HOME` directory and records the details of files in the `recyclebin`.

The `restore` script uses this data to recover recycled files, after which their records are removed from this file.

## Tests

A test suite is provided in `tests/` for the `recycle` and `restore` scripts.

Run the tests as follows from the project root directory:

```bash
$ bash tests/test_recycle
$ bash tests/test_restore
```

## Credits

Credit for the ASCII art recycle logo goes to [Normand Veilleux](http://www.afn.org/~afn39695/veilleux.htm). This program features a slightly revised version of his original design.
