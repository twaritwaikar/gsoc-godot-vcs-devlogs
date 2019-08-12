Project Name: Version Control Systems Integration

My Name: Twarit Waikar (@IronicallySerious)

Mentors: Gilles Roudiere (@Groud) and Jairo Honorio (@jahd)

Repositories: 
* PR candidate: https://github.com/IronicallySerious/godot/tree/add-vcs-integration
* Git API GDNative Plugin: https://github.com/IronicallySerious/godot-git-plugin

# Re-cap from Progress report #1
The version control systems integrations proposes to add new UI to the editor which lets the user to commit, stage, and view file differences from the last version and the current state of the file, in a presentable manner which helps improve the workflow of the user using the editor in terms of managing version control.

The integration mainly required 3 distinct verticals:

* A version control themed editor plugin to place all the UI elements required.
* An interface/API for the Godot editor to extract all VCS metadata from.
* An implementation of the VCS interface for any of the popular VCSs in use.

Out of these, only the first and the second verticals are planned to be merged into Godot master and the third vertical will be kept in a separate repository. The third vertical is a GDNative Plugin, (referred to as 'addon' further).

Keeping the implementation separated from the interface helps us to create different behaviours of the VCS interface depending on what VCS is at use in the project. I have worked on the Git implementation as a part of GSoC 2019 and for any of the other VCSs, we are depending upon future work by fellow contributors. 

# Complications faced since Progress report #1
My mentors, Groud and jahd, and I realised the the kind of architecture that we were hoping to accomplish was actually unsuitable for the kind of functionality that the existing engine API has. I have tried to summarise my entire research surrounding the topic of creating a GDNative addon that extends an API that is called to from within the editor in a devlog post present [here](https://github.com/IronicallySerious/gsoc-godot-vcs-devlogs/blob/master/2019-8-2.md). 

You can also have a look at the [predecessor of the above mentioned devlog](https://github.com/IronicallySerious/gsoc-godot-vcs-devlogs/blob/master/2019-7-08.md) to know more about what different types of complications we faced while designing the architecture for this sort of a involvement between the editor and the GDNative addons.

# Version Control Editor Plugin
This editor plugin is responsible for providing all VCS integration UI elements to the editor and hands the data extraction and error handling related to the VCS interface/API.

The entire VCS interaction is fired off by the `Set Up Version Control` dialog (same as previously reported but with an internal working change):

![](/images/003.png)

In the above screenshot, the name `GitAPI` is coming directly from the GDNative addon that impements the Git interactionIf you'd like to know how we managed to detect addons from the editor and use it to implement an API which is consumed by the editor, you can refer to the devlog links in the previous section. The solution came out to be rather simple but it required some extensive research from both my mentors and me, since this use-case of GDNative was rather an odd one. Anyway, we are happy to share the results that we have found.

Currently we can expect the Commit panel to look similar to as shown below (not representative of the final version):

![](/images/005.png)

You may notice in the panel above that the 'Refresh' button has recently been clicked and the 'New' section of the tree has been populated. Upon opening this tree, you shall see a list of all the new files that have been added to the Git repository. Since the demo project doesn't come with a pre-initialised Git repository, all the files of the project are currently recognised as newly created files.

The next major UI element that the Version Control Editor Plugin provides is the Version Control dock which will most likely be placed at the bottom dock likewise:

![](/images/006.png)

The left side will be showing the version of the file in the previous commit, and the right side will be showing the newer changes for the file.

# GDNative based Git Addon
[libgit2](https://libgit2.org) is a portable, pure C implementation of the Git core methods provided as a linkable library with a solid API, allowing to build Git functionality into your application

We are using [libgit2](https://libgit2.org), which is a C library best described by the libgit2 developers themselves at [their Github project](https://github.com/libgit2/libgit2) as follows:
> libgit2 is a portable, pure C implementation of the Git core methods provided as a linkable library with a solid API, allowing to build Git functionality into your application

Since it is a C implementation, we have successfully linked libgit2 to the GDNative C++ bindings along with our addon logic for extraction of Git metadata as well as repository data. The architecture being followed to call into libgit2 has been explained in the devlog that I mentioned in the previous section.

So far, we are able to replicate Git commands like `git init`, `git commit`, `git add`, and `git diff` with libgit2 the help of their extremely thorough [user guides](https://libgit2.org/docs/guides/101-samples/).


