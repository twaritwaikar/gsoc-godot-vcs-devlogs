Project Name: Godot VCS Wide Integration

My Name: Twarit Waikar (@IronicallySerious)

Mentors: Gilles Roudiere (@Groud) and Jairo Honorio (@jahd)

Repositories: 
* Godot's Framework for a VCS integration: 
    * Dev: https://github.com/IronicallySerious/godot/tree/vcs-api-expand
    * Final (PR candidate): https://github.com/IronicallySerious/godot/tree/add-vcs-integration
* Git Interaction API Plugin: https://github.com/IronicallySerious/godot-git-plugin

# Introduction

Since Godot is gradually entering the competitive market, dominated by only a handful of fully featured engines, it is only intuitive that it should support Godot game developers with a strong version control integration from within the editor. This project aims at creating a GUI interface to the user's version control system deployed on their project and create a versioning system agnostic API that caters to all version control systems (VCSs) at once. 

This integration can further be used to create more in-depth interactions to a VCS like viewing file diffs right in the editor, committing code with a simple click, resetting to a previous state almost instantly without leaving the Godot editor.

# How does it look like?

We have taken inspiration from the diff UI currently present in Atom, Visual Studio Code and various other popular text editors and IDEs. We also have adopted some UX tips from Unreal Engine 4 for the initialisation of a VCS in a new project, however since Unreal Engine uses an external editor for C++ code by default, we found ourselves looking at something similar to what we already have in Visual Studio/Visual Studio Code.

![](/images/001.png)

In the above screenshot, you can notice:
* A Version Control dock at the bottom.
* A Version Commit dock on the right-hand side (A list of staged files and modified files is also planned)
* A Version Control Actions popup menu under Project Menu at the top left. This should behave like a quick VCS action toolbar.
* A Version Control dedicated settings tab in the Project Settings menu. This is planned to also let the VCS addon (a.k.a. implementation of the VCS API) add a bunch of VCS's nature specific settings to this tab.

![](/images/002.png)

Choosing Set Up Version Control brings up this menu. This is to detect all the different VCS API implementations available with the engine. The initialise button does some basic registrations and it will also be providing the VCS implementation with the ability to provide their own initialisation steps depending on their type.

# Breaking down the framework

The integration project has several vertical slices present in it:

1. A version control themed editor plugin
2. An interface for the Godot editor to extract all VCS metadata from
3. An implementation of the VCS interface for any of the popular VCSs in use.

Our target for minimally complete support is focused on Git currently. By the end, we should be able to display file diffs in the editor, commit changes, stage/unstage files and extract other types of important metadata for use in the editor.

# How do we plan to manage distributed and centralised VCSs?

We plan to split the API into two when the time comes to implement a centralised VCS. This is a development style decision amongst many others as suggested by my mentors, Groud and jahd, so that we make as much progress as we can without worrying about a problem that will only come once in the future. 

# Which VCSs are planned for support?

Although we will be focusing only on Git for the duration of this year's GSoC, we also recognise that Perforce and SVN are also some widely used VCS candidates for future support.

We should theoretically support all publically available VCSs so that integrating them into Godot is much easier by just implementing an API correctly and not worrying about how the data should be displayed in the editor.

# Coming up next

For the remaining of the coding period, I will be focusing on getting data from Git displayed in the editor, in different forms of commits, diffs, staging areas, and initialisation of Git dotfiles, to name a few. Since one of the long term goals is to keep the API completely VCS nature agnostic, I will also be paying attention to the design I will be using in the future to create the VCS API. In the end, we will be putting the entire integration in a GDNative plugin so as to provide a plug-and-play-like experience with the Git interaction implementation.

You can also have a look at my personal tracking tool I have [here](https://github.com/IronicallySerious/gsoc-godot-vcs-devlogs) to have a deeper look into what all decisions are going into realising this integration.
