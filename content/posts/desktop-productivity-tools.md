---
path: '/2012/09/desktop-productivity-tools-tips-on-using-executor-and-winsplit-revolution/'
date: '2012-09-11'
title: 'Desktop Productivity Tools: Tips on using Executor and WinSplit Revolution'
---

Two software programs are critical for improving my productivity. The first is Executor. The second is WinSplit Revolution. Both are free to download. I’ll describe both of them and tips for using them.

Executor is essentially an enhanced version of the command line. Its key feature is an easy way to create keyboard shortcuts to key applications and files. For example, I can press Ctrl+Alt+Z to launch Executor and then type the first few letters of “projectslist” to see a SharePoint site with files about upcoming projects. Alternatively, I can launch common folders or key information, such as my conference call number.

Executor can be downloaded from http://executor.dk/.

Once you have installed Executor, you should set it to launch on startup and to index your common files and folders. In my experience, this does not cause any noticable slowdown on your computer. Once Executor is running, press the default launch key, Windows+Z, to bring it up. Select the “Settings” tab at the top. On the first tab, I recommend changing the hotkey to “Ctrl+Alt+Z” because it is easier to type than Windows+Z, but feel free to change it to whatever is easier for you. On the “Indexing & cache” tab, I recommend you add your “My Documents” folder at the bottom and include subfolders (index depth of 10, for example). This will allow Executor to reindex new files and make it easier for you to launch them.

To add a new keyword, just open Executor and press Ctrl+K. Then press “Insert” to add a new keyword. You can drag shortcuts onto the new keyword screen to automatically fill in the fields.

I also recommend you create a new keyword called `echo` with the command: `c:\windows\system32\cmd.exe$GRABTOINPUT$` (without quotes) and the paramaters: `/c echo $P$` (also without quotes). Then, if you want to quickly display information that you can copy and paste elsewhere, you can create a new keyword with the command `echo` and the parameters with the information you want. For instance, I create a command called “conference” and put my conference call dial-in information in the parameters field.

———-

WinSplit Revolution enables you to use keyboard hotkeys to move items from one monitor to another and, on a single monitor, from one side to another. It is an extremely handy tool if you have multiple monitors and you need to quickly move applications between them. Dragging them back and forth with the mouse if very slow.

You can download WinSplit Revolution from http://winsplit-revolution.com/. I recommend you set it to launch when you start Windows. (It takes less than three megabytes of RAM.) To do this, launch it and right-click the icon in the system tray. Click “Launch with Windows.” I also recommend you customize the hotkey settings. Right-click the system tray icon and select “Hotkey settings.” I recommend you set “Move to Left screen” to Ctrl+Alt+1 and “Move to Right screen” to Ctrl+Alt+2. If not already set, you should set the down, left, right and up functions to Ctrl+Alt+appropriate arrow key. These settings will allow you to quickly move windows between two monitors by pressing Ctrl+Alt+1 or Ctrl+Alt+2, or use Ctrl+Alt+(left arrow key) or Ctrl+Alt+(right arrow key) to arrange items side by side on a single screen. Either of these moves are very impressive to onlookers.
