Project Name: Version Control Systems Integration

My Name: Twarit Waikar (@IronicallySerious)

Mentors: Gilles Roudiere (@Groud) and Jairo Honorio (@jahd)

Repositories: 
* PR candidate: https://github.com/IronicallySerious/godot/tree/add-vcs-integration
* Git API GDNative Plugin: https://github.com/IronicallySerious/godot-git-plugin

# Description

The Version Control Systems Integration project introduces VCS (short for 'Version Control System') management from within the Godot Editor. Currently, we have a Git management implemented for Godot and ready to use. However, support for other VCSs is supported and addons for these projects can be easily implemented using Godot Scripts. Thus, Git management has been implemented in C++ with GDNative using [libgit2](https://libgit2.org).

# Installation

1. Download the Git addon, which consists of the `bin/` folder, `git_api.gdns` and `git_api.gdnlib` files present [here](https://github.com/IronicallySerious/godot-git-plugin/tree/master/demo). Put the contents in your Godot project folder.
2. Open the Godot project using the Godot Project manager. P.S. Make sure you use the Godot branch that has the VCS Integration to test this feature.

That is it! This is the courtesy of how we have used GDNative and the existence of GDNative singleton libraries that are instantiated at runtime.

# Instructions

When the project loads up, you are greeted with the Godot editor that you know and love. However, you now have the option to set up a VCS addon from the editor.

1. Open `Project` > `Version Control`. Select `Set Up Version Control`.

![](/images/008.png)

2. A popup will ask you which VCS addon would you like to use. In our case we are using Git, so select `GitAPI` from the drop-down menu. Then click `Initialize` and close the popup.

![](/images/009.png)

_If you are not able to see a `GitAPI` option then this means that you don't have the addon binaries present in your project folder._

3. Open the `Commit` tab that you may see alongside the `Inspector` and `Node` tabs. At first, you will see that every file in the project folder is counted as a new addition to the repository since we have started from a VCS-less project. If you already have Git running on your project then the addon should be able to just use your existing repository. 

![](/images/010.png)

* The `.gitignore` and `.gitattributes` are Git specific configuration files and creating them in the project folder is handled by the Git addon, without consult from the Godot editor. You can edit them afterwards for a customized Git experience. If you are using an existing Git repository then the Git Addon will use your existing Git configuration files by default.

* To manually enforce a change in the project files, we have also provided a `Refresh` button that detects changes manually. If your file change is not showing up in the staging area, then consider checking `.gitignore` to see if your file has been ignored by Git in the first place. 

4. Stage the files that you would like to commit in the next version by checking the required files and clicking `Stage Selected`. If you are working in a new Git repository then `Stage All` will stage all the files present in the staging area. 

5. After staging, the staging area will acknowledge the file changes with a green tick.

![](/images/011.png)

Alternatively, if you would like to not stage `default_env.tres` (for example) then you should be able to just uncheck `default_env.tres` and press `Stage Selected`. Thus, you may get a view similar to the one below.

![](/images/012.png)

This way, `default_env.tres` will not get committed in the next version.

6. Add a nice commit message and click `Commit Changes`.

![](/images/013.png)

You may also notice that the number of staged files is also reported. This is for your utility where you may like to confirm if you are committing the correct project files.

7. After clicking `Commit Changes` you can start working on the next version of your project!

8. When you are done, you can choose to shut down the VCS API with the selected addon by selecting `Project` > `Version Control` > `Shut Down Version Control`. This will take away the VCS Integration related GUI and return you to the normal state of the editor without the VCS integration.

# Useful features

## Stage Status

At every Git index change, you are notified of the number of files in the stage, the number of files committed and any errors that you may like to fix before committing the next version in history.

## Diff Viewer

You would have also noticed the `Version Control` dock at the bottom of the Godot Editor.

![](/images/014.png)

While staging files to the next version, you can view the file changes in the diff viewer. Just click on the file name in the Staging Area and you will see the diff appear in the bottom Version Control dock. Git doesn't show a diff for new files, thus to test this you will have to commit a file first before making changes to it. 

For example, let's say we committed `new_script.gd` once, and then made some changes to it. After saving the changes, we shall see the following view.

![](/images/015.png)

Clicking the file name in the Staging Area will show us the following diff.

![](/images/016.png)

The diff area also has a manual refresh button on the top right of the Version Control panel to refresh the diff if you feel your changes have not been reflected in the diff shown.

# Closing Notes

The VCS Integration has a basic set of tools available at your disposal to maintain the versions of your game from within the editor. The integration is in an improvable state where it demands more attention to incorporate features such as solving merge conflicts, adding diff based colored gutters in the script editor, and some more conventional VCS uses. However, the current feature set should benefit the editor to improve upon the limitation where the Godot Editor was completely unaware of any VCS in use. 

# Experience of working with the Godot Community

I am delighted to work with the Godot community and I plan to keep contributing to Godot Engine in the future. I would like to thank my mentors Gilles Roudiere (@Groud) and Jairo Honorio (@jahd) for the constant support they gave me while developing the VCS Integration. Also, I am thankful to all the people who helped me learn GDNative at #godotengine-devel and #godotengine-gdnative and special thanks to Thomas Herzog (@karroffel) for his guidance that helped us in building the powerful VCS API with elegant simplicity. A final thanks to the Godot Engine developer community that considered me as one of their GSoC students for 2019 and allowed me to implement my project idea of a VCS integration for Godot throughout the GSoC coding period.
