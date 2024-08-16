---
description: VS Code, Tips, Hacks, etc.
---

# VS Code

## What extensions are we allowed to use in VS Code?

Following is the list of extensions **allowed** in VS Code. Please do not use any other extensions.

### Extension: C/C++ (by Microsoft)

The C/C++ extension adds language support for C/C++ to Visual Studio Code, including features such as IntelliSense and debugging.

### Extension: Remote - WSL and related Remote - XX extensions (by Microsoft)

This is to compile your code remotely in Linux Subsystem for Windows. _This extension is optional if you are using MinGW Linux executables on windows._

### Extension: Better Comments (by Aaron Bond)

The Better Comments extension will help you create more human-friendly comments in your code.\
With this extension, you will be able to categorize your annotations into:

* Alerts
* Queries
* TODOs
* Highlights
* Commented out code can also be styled to make it clear the code shouldn't be there
* Any other comment styles you'd like can be specified in the settings

### Extension: C-family Documentation Comments (by Aleksei Ermolov)

Generate documentation comments for Visual Studio Code.

### Optional Extensions:

#### vscode-pdf (by tomok1207)

Display pdf file in VSCode. _This will be helpful to keep your prac sheet open on one side while you code._

## Can I automate the compilation and debugging in VS Code?

During the first few weeks, please use VS Code or vim as a dumb notepad editor and **use a terminal to compile your code** and run it. Later on, you can try the advanced features of VS Code with some experience in the complete process of C code compilation and debugging with `gcc` and `gdb`

In UCP, you are expected to know `gcc, gdb, valgrind, make` utility usage on a terminal in detail. Therefore, it is important that you do not rely on VS Code advanced features to automate those tasks.

{% content-ref url="../../../ucp-expectation.md" %}
[ucp-expectation.md](../../../ucp-expectation.md)
{% endcontent-ref %}

## Can I use VS code to generate Makefile?

**DO NOT USE** generated Makefile to compile your code. You must create your own Makefile from the sketch according to the template given on the blackboard which will be considered relevant during marking.

## VS Code Productivity Tips

The following tips are based on javascript language but are mostly applicable to C/C++ development!&#x20;

{% embed url="https://www.youtube.com/watch?v=ifTF3ags0XI" %}

### Code Snippet Generator

{% embed url="https://snippet-generator.app/" %}

{% embed url="https://www.youtube.com/watch?v=l66s-Ju_QRk" %}

{% embed url="https://www.youtube.com/watch?v=JIqk9UxgKEc" %}

{% embed url="https://code.visualstudio.com/docs/editor/userdefinedsnippets" %}

### VS Code Windows Keyboard Shortcuts

{% embed url="https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf" %}
