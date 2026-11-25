Naiwa Floating Assistant is a standalone Windows companion for the Codex desktop app. It opens the current conversation in a separate, always-on-top window that can collapse into a draggable toolbar. The camera button captures the screen and adds the image to the chat draft.

**Compatibility.** This `v0.1.0` preview was tested on Windows 11 x64 with Codex desktop `26.1120.6357.0`. The assistant recognizes Simplified Chinese and English menu labels in Codex. The assistant’s own tooltips and right-click menus currently use Simplified Chinese. Other Codex versions and display languages need separate testing.

## Quick start

1. Download `Naiwa-Floating-Assistant-v0.1.0-win-x64.zip` from GitHub Releases and extract the entire archive.
2. Double-click `bin\NaiwaFloatingAssistant.exe` in the extracted folder.
3. Open an existing conversation in Codex desktop and bring the main window to the foreground. Look for the frog holding a drawer, just to the left of the window controls.
4. Click it to open the floating chat. The new window starts collapsed into an always-on-top toolbar, and the main window stays available.

| Control | Action |
| --- | --- |
| Six-dot grip | Drag the toolbar. It keeps its position when you expand or collapse the chat. |
| Frog holding a chat card | Expand or collapse the chat. The tooltips are `展开聊天` (expand) and `收起聊天` (collapse). |
| Frog holding a camera | Capture the entire monitor containing the toolbar, open the floating chat, and try to paste the screenshot into the draft. |

The assistant runs independently in your Windows sign-in session. When Codex closes, the launcher button hides. It reappears when you reopen Codex and bring the main window to the foreground.

## Build and install

Use PowerShell in your signed-in Windows desktop session, with standard user permissions. Run these commands from the extracted folder or repository root.

Run the deployment script to install and launch the release build.

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\deploy.ps1
```

To rebuild from source, run the build command, then run the deployment command above.

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\build.ps1
```

`build.ps1` uses the Windows .NET Framework 4.x x64 compiler and embeds the three PNG assets in `bin\NaiwaFloatingAssistant.exe`. `deploy.ps1` builds the executable if needed, copies it to `%LOCALAPPDATA%\NaiwaFloatingAssistant`, verifies the copy, and launches it through File Explorer. The command-line execution policy applies to that PowerShell process only.

The `-Startup` option enables automatic launch at Windows sign-in.

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\deploy.ps1 -Startup
```

Use `-NoStart` to install and verify the files only, or `-InstallDirectory` to choose a different installation folder. To update, exit the running assistant from its right-click menu and run `deploy.ps1` again. Run one copy per Windows sign-in session.

### Deploy with Codex

You can give the following prompt to Codex on the Windows machine where you want to install the assistant.

> Follow `README.md` to install Naiwa Floating Assistant in my current Windows desktop session. Confirm that this is Windows x64 and that Codex desktop is available. If `bin\NaiwaFloatingAssistant.exe` is present, run `deploy.ps1` directly. If you have only the source, run `build.ps1` first. Use the default installation location and add Windows sign-in startup only if I explicitly request it. Verify the running process and the round launcher button in the foreground Codex window, then follow the verification steps below. Use a test conversation and non-sensitive screen content so existing drafts are preserved. Report what you verified and what remains untested.

### Verify the installation

1. Run `Get-Process NaiwaFloatingAssistant`. Bring an existing Codex conversation to the foreground and look for the round drawer button near the top right.
2. Click the launcher and verify that a new always-on-top toolbar appears while the main window remains usable. Drag the toolbar, expand and collapse the chat, and check that the toolbar stays where you placed it.
3. In a test conversation, click the camera. Check that a screenshot appears in the floating chat draft and the text input has focus. Remove the test image from the draft when you finish.
4. If the image is only in the clipboard, click the chat input and press `Ctrl+V`. If the launcher is missing, confirm that the assistant is running in your local interactive Windows session, then bring an existing Codex conversation to the foreground.

## Screenshots and temporary files

The assistant captures the image in memory, copies it to the Windows clipboard, and tries to paste it into the draft with `Ctrl+V`. You send the message yourself in Codex. Codex may create files named `codex-clipboard-*.png` under `%TEMP%` when handling a pasted image. Their lifetime depends on Windows and Codex temporary-file cleanup.

## Exit and uninstall

Right-click the round drawer button and choose `退出浮窗助手` (Exit Floating Assistant). If you opted into sign-in startup, open `shell:startup` in File Explorer and remove `Naiwa Floating Assistant.lnk`. You can then remove the assistant from the installation path shown by `deploy.ps1`. For the portable release, the executable is in the extracted `bin` folder.

## Source and releases

- `src/` contains the windowing, screen capture, and button drawing code.
- `assets/` contains the original artwork for the floating window, chat toggle, and camera buttons.
- `build.ps1` builds the executable and `deploy.ps1` installs it for the current Windows user.
- `pack.ps1` builds the executable and creates a release ZIP with both language guides. Use `-UseExistingBinary` to refresh the ZIP around an existing `bin\NaiwaFloatingAssistant.exe`.
- Git ignores the generated `bin/` and `release/` directories. Attach the ZIP from `release/` when creating a GitHub Release.
