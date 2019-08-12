Project Name: Version Control Systems Integration

My Name: Twarit Waikar (@IronicallySerious)

Mentors: Gilles Roudiere (@Groud) and Jairo Honorio (@jahd)

Repositories: 
* PR candidate: https://github.com/IronicallySerious/godot/tree/add-vcs-integration
* Git API GDNative Plugin: https://github.com/IronicallySerious/godot-git-plugin

# Re-cap from Progress report #1
The version control systems integration proposes to add new UI to the editor which lets the user commit, stage, and view file differences from the last version and the current state of the file, in a presentable manner which helps improve the workflow of the user using the editor in terms of managing version control.

The integration mainly required 3 distinct verticals:

* A version control themed editor plugin to place all the UI elements required.
* An interface/API for the Godot editor to extract all VCS metadata from.
* An implementation of the VCS interface for any of the popular VCSs in use.

Out of these, only the first and the second verticals are planned to be merged into Godot master and the third vertical will be kept in a separate repository. The third vertical is a GDNative Plugin, (referred to as 'addon' further).

Keeping the implementation separated from the interface helps us to create different behaviours of the VCS interface depending on what VCS is at use in the project. I have worked on the Git implementation as a part of GSoC 2019 and for any of the other VCSs, we are depending upon future work by fellow contributors. 

# Complications faced since Progress report #1
My mentors, Groud and jahd, and I realised the kind of architecture that we were hoping to accomplish was unsuitable for the kind of functionality that the existing engine API has. I have tried to summarise my entire research surrounding the topic of creating a GDNative addon that extends an API that is called to from within the editor in a devlog post present [here](https://github.com/IronicallySerious/gsoc-godot-vcs-devlogs/blob/master/2019-8-2.md). 

You can also have a look at the [predecessor of the above-mentioned devlog](https://github.com/IronicallySerious/gsoc-godot-vcs-devlogs/blob/master/2019-7-08.md) to know more about what different types of complications we faced while designing the architecture for this sort of an involvement between the editor and the GDNative addons.

# 1. Version Control Editor Plugin
This editor plugin is responsible for providing all VCS integration UI elements to the editor and hands the data extraction and error handling related to the VCS interface/API.

The entire VCS interaction is fired off by the `Set Up Version Control` dialog (same as previously reported but with an internal working change):

![](/images/003.png)

In the above screenshot, the name `GitAPI` is coming directly from the GDNative addon that implements the Git interaction. If you'd like to know how we managed to detect addons from the editor and use it to implement an API which is consumed by the editor, you can refer to the devlog links in the previous section. The solution came out to be rather simple but it required some extensive research from both my mentors and me since this use-case of GDNative was rather an odd one. Anyway, we are happy to share the results that we have found.

When the Git addon is initialized, the addon also initializes a bare `.gitignore` file. All these behaviours are handled by the addon so the engine is not required to do any of the VCS specific tasks.

Currently, we can expect the Commit panel to look similar to as shown below (not representative of the final version):

![](/images/005.png)

You may notice in the panel above that the 'Refresh' button has recently been clicked and the 'New' section of the tree has been populated. 

![](images/006.png)

Upon opening this tree, you shall see a list of all the new files that have been added to the Git repository. Since the demo project doesn't come with a pre-initialised Git repository, all the files of the project are currently recognised as newly created files. The checkboxes shall be indicative of whether a file will be added to the stage or not. This is similar to selecting what input we need to provide the `git add` command.

The next major UI element that the Version Control Editor Plugin provides is the Version Control dock which will most likely be placed at amongst the bottom docks likewise:

![](/images/007.png)

The left side will be showing the version of the file in the previous commit, and the right side will be showing the newer changes for the file. The panel UI will remain, however, the logic which displays the difference contents is in works.

# 2. Editor VCS Interface
As explained earlier, the engine editor is theoretically not allowed to even know the name of the version control system that is in use by the user. This means the editor needs to consult an API that extracts all such data from the GDNative based VCS addons.

This interface is functionally complete. It currently defines all methods that the editor requires, which act as proxies to the methods defined in the GDNative addon. Thus, a function like `get_vcs_name()` would reply with a "Git" response, which has essentially originated from the GDNative addon.

The proxy architecture in play here has particularly helped us to create an API which does not require the implementation addon to implement the entire variety of methods defined in the API. The addon can accomplish far less and still provide enough data for the editor to correctly display the data extracted from the VCS. 

# 3. GDNative based Git Addon
[libgit2](https://libgit2.org) is a portable, pure C implementation of the Git core methods provided as a linkable library with a solid API, allowing to build Git functionality into your application

We are using [libgit2](https://libgit2.org), which is a C library best described by the libgit2 developers themselves at [their Github project](https://github.com/libgit2/libgit2) as follows:
> libgit2 is a portable, pure C implementation of the Git core methods provided as a linkable library with a solid API, allowing to build Git functionality into your application

Since it is a C implementation, we have successfully linked libgit2 to the GDNative C++ bindings along with our addon logic for extraction of Git metadata as well as repository data. The architecture being followed to call into libgit2 has been explained in the devlog that I mentioned in the previous section.

So far, we can replicate Git commands like `git init`, `git commit`, `git add`, and `git diff` with libgit2 the help of their extremely thorough [user guides](https://libgit2.org/docs/guides/101-samples/).

# Closing Notes
I am glad to be working on this project idea of mine and seeing it in its close-to-mature form as it is currently is a delight. I plan to finish the remaining bits by this weekend and deliver some instructions to use this feature in the next report.
