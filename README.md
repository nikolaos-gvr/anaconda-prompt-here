# Open Anaconda Prompt Here

A simple registry setup that adds **Open Anaconda Prompt here** to the Windows File Explorer right-click menu. It opens Anaconda's **base** environment directly in the folder you select.

The files are configured for Anaconda installed at **`C:\ProgramData\anaconda3`**. If yours is installed elsewhere, update the path before running the setup.

## Files

| File | Purpose |
| --- | --- |
| `enable-anaconda-context-menu.reg` | Adds entries for right-clicking a folder and empty space inside a folder. |
| `remove-anaconda-context-menu.reg` | Removes both entries and their command subkeys. |
| `README.md` | Instructions for setup, customization, removal, and testing. |

Both registry files contain explanatory comments. Lines beginning with `;` are comments; keep the `Windows Registry Editor Version 5.00` header as the first line.

## Before you start

- You need Windows File Explorer and an existing Anaconda installation; these files do not install Anaconda.
- The setup file expects `C:\ProgramData\anaconda3\Scripts\activate.bat` to exist.
- The menu entries apply to your Windows account, under `HKEY_CURRENT_USER\Software\Classes`.
- Use local folders. Network paths such as `\\server\share` are not supported by the directory-change command.
- Paths with spaces are quoted. Paths containing shell expansion characters such as `%` have not been tested.
- This is an unofficial project, not affiliated with Anaconda, Inc.

## Install

1. If you downloaded the ZIP, extract it first.
2. Check that Anaconda is installed at `C:\ProgramData\anaconda3`. If it is elsewhere, follow the customization instructions below before continuing.
3. Double-click `enable-anaconda-context-menu.reg` in File Explorer and accept the Windows registry import prompts.
4. Right-click a project folder, or open it and right-click empty space, then choose **Open Anaconda Prompt here**.

On Windows 11, the entry normally appears under **Show more options**. If it is not immediately visible, close the menu and reopen the File Explorer window.

The prompt should display `(base)` and the selected folder path. A project-specific environment is not selected automatically; use `conda activate your-environment` if needed.

Importing the setup again updates the same keys rather than creating duplicate entries. You can move or delete these package files after import; the menu command refers directly to your Anaconda installation.

## Use a different Anaconda installation path

Right-click the setup file and open it in a text editor. Replace **every** occurrence of this path in the registry values:

```text
C:\\ProgramData\\anaconda3
```

For example, if your installation is `C:\Users\Alice\anaconda3`, replace it with:

```text
C:\\Users\\Alice\\anaconda3
```

Keep the doubled backslashes and the existing `\"` escapes in the registry values. The examples use literal usernames: do not substitute `%USERPROFILE%` into these values. The comments use ordinary, single-backslash paths and can be updated separately.

Check that `Scripts\activate.bat` exists inside your installation. If `Menu\anaconda-navigator.ico` is missing, you may remove the two `"Icon"=` lines; they only control the menu icon.

Save the file with its `.reg` extension and import it. The removal file needs no path changes.

## Uninstall

Double-click `remove-anaconda-context-menu.reg` and accept the import prompts using the same Windows account that installed the entries.

This removes only these keys and their subkeys:

```text
HKEY_CURRENT_USER\Software\Classes\Directory\shell\AnacondaPromptHere
HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\AnacondaPromptHere
```

Anaconda, environments, and project files are unaffected. Removal does not restore older custom values that might have existed under these same key names before installation.

## Testing

I checked the launch command with conda `25.11.1` installed at `C:\ProgramData\anaconda3`. It activated Anaconda and opened the correct project directory, including spaces in the folder path.

The registry import and right-click menu still need a full manual check.

To check the setup:

1. Import the setup file and confirm both context-menu entries appear.
2. Launch from a folder's context menu and from empty space inside it.
3. Run `conda --version`, `conda info --envs`, and `cd`; confirm conda works, base is active, and the directory is correct.
4. Repeat with a folder containing spaces, and on another local drive if available.
5. Import the removal file and confirm both entries disappear.

## References

- [Microsoft: Adding, modifying, and deleting registry keys with .reg files](https://support.microsoft.com/en-us/topic/how-to-add-modify-or-delete-registry-subkeys-and-values-by-using-a-reg-file-9c7f37cf-a5e9-e1cd-c4fa-2a26218a1a23)
- [Microsoft: File Explorer and Show more options](https://support.microsoft.com/en-us/windows/experience/fileexplorer/file-explorer-in-windows)
- [Anaconda: Windows installation and opening Anaconda Prompt](https://www.anaconda.com/docs/getting-started/anaconda/install/windows-gui-install)
