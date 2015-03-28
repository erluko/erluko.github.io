---
title: Pasting Service For VirtualBox
---

Pasting In VirtualBox
---------------------

Have you ever found yourself trying to paste a value into a VirtualBox
instance that's not running an OS with clipboard integration?

If you know the keyboard scancodes of the characters you'd like to
input, you can use the `controlvm` command of `VBoxManage` as follows:

`VBoxManage controlvm $VM_ID keyboardputscancode $DOWN_CODE $UP_CODE`

Setting `DOWN_CODE` to 10 and `UP_CODE` to 90 inserts the letter 'q'

That works, but even with some automation it's a bit of a pain.


Here's one way to do it under OS X without messing about with KeyCode
generation and a running terminal.

Create a Paste Service
======================

In Automator, create a new Service.

The service should receive no input.

The first action should be "Get Clipboard Contents"

The second action should be "Run AppleScript" with the following code:
```
on run {input, parameters}
	set tinput to input as text
	repeat with thisCharacter in the characters of tinput
		set thisCharacter to thisCharacter as text
		tell application "System Events" to keystroke thisCharacter
	end repeat
	return input
end run
```

Save the workflow as 'TypeClipboardContents'


Test The Service
================

1. Copy some text.
2. Open an application that allows text insertion.
3. Run the service from the Application's services submenu.


Keyboard Shortcut for VirtualBox
================================

Assuming the above steps worked for you, you can avoid having to use
the mouse by adding a keyboard shortcut for your service. I wanted to
use `⌘-V` as the shortcut, but only in VirtualBox.

1. Open System Preferences
2. Select the Keyboard pane. On some OS X versions it may be called
"Keyboard and Mouse"
3. Select the Shortcuts tab.
4. Select "App Shortcuts" from the panel on the left
5. Click the Plus(+) button
6. Select the VirtualBoxVM app. Mine was at:
`/Applications/VirtualBox.app/Contents/Resources/VirtualBoxVM.app`
7. Set Menu Title to 'TypeClipboardContents'
8. Set Keyboard Shortcut to `⌘-V`


Test The Shortcut
=================

1. Copy some text to the clipboard
2. Switch to the running VM of your choice
3. Type `⌘-V`

If it worked, great. If not, retrace your steps and try again!
