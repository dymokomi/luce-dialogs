# luce-dialogs

Native panels for choosing files and folders: the desktop's own, run modally
from the UI thread.

```luce
from luce_dialogs import dialogs

let photo = dialogs.open_file("Choose a photo") else return        # one file, or none when cancelled
let several = dialogs.open_files("Choose photos") else return      # paths, one a line
let folder = dialogs.choose_folder("Choose a folder") else return  # a folder
let target = dialogs.save_file("Save as", "untitled.png") else return
```

| function | macOS | Windows | Linux |
| --- | --- | --- | --- |
| `open_file`, `open_files` | NSOpenPanel | GetOpenFileNameW | zenity, else kdialog |
| `choose_folder` | NSOpenPanel, folders only | SHBrowseForFolderW | `zenity --directory`, else `kdialog --getexistingdirectory` |
| `save_file` | NSSavePanel | GetSaveFileNameW | zenity, else kdialog |

## Modules

| import | what it holds |
| --- | --- |
| `import dialogs` | Native file dialogs |

## Using it

Add the dependency to `package.prisma`; the modules keep their short names:

```prisma
def dependency "luce-dialogs" {
    str owner = "dymokomi"
}
```

## Depends on

- luce-window

## Platforms

macOS, Windows, and Linux through the desktop's helper (zenity or kdialog,
whichever is installed).

Native libraries it links, by platform (declared in `package.prisma`, linked only when the program reaches code that needs them):

- macos: AppKit, Foundation
- windows: comdlg32, user32, shell32, ole32

## Tests

`luc test` runs the tests in `tests/dialogs/`: how a Linux helper's answer is read, with a shell standing in for zenity and kdialog, and the refusal of a dialog off the UI thread. A panel itself needs a person to answer it, so it is not tested.

## License

MIT or Apache-2.0, at your option.
