# PatchStrap
Developed by Silverwolfg11.

## What is it for?
PatchStrap is a patching system designed for modifying git projects without uploading the original project to a new repo.

## Why would I use it?
I mainly use this system for PRs. Sometimes in an original project, there is stuff that I don't like, but that I don't want to PR back
for whatever reason. Thus I can use these patches to pick and choose which changes I want to PR.

## How do I set this up?
Make a new project folder. You can delete the default src folder. Then copy the patch file to your project directory.
In the patch file, you have to edit a few variables. The main two you are going to need to edit is `repoLink` and `repoFolder`.
It's essential that you edit these variables otherwise the patch script won't work.

For the repo link, paste the git repo you want to close within the double quotes of the variable so it will look something like
`repoLink="https://gitproject"`. 

Next, for the repo folder, this is simply the folder where you want the repo to be cloned into. Set the folder name into the double quotes
for `repoFolder`, so it ends up looking like `repoFolder="GitProjFolder"`.

## Using the Patch Script
First, you want to clone the repo. To do this just execute `./patch download` and it will download the folder.

Make your changes within your designated git project folder. Remember these are commit patches, so make patches for every related
change. Once you're ready to make a patch for your changes, just run `./patch gen [Patchname]`. Make patch name whatever you want,
usually a description. The patch name does accept spaces. Repeat the process for as many patches you want to make.

**Once you have finished making patches, make sure to build them.** You can build them by running `./patch rb`. It's highly important
that you build your patches.

You can now upload the Patches folder and the patch script to your own git repo.

That's it, that's the gist of using patches! 

## Editing Patches
Make sure all your patches have been re-built. Editing patches is a little tricky. First, run `./patch edit`. You will see a
list of your patches with "pick" in-front of them. Replace "pick" with "edit" and save the file (if you're unfamiliar with a bash terminal
 \- escape, then click into the terminal and then type `:wq`). Only edit one patch at a time.

Saving your edits is a little complex. There is no good easy way to do this, so you're going to have to do it manually.
Go into your project folder (`cd projfolder`), then add your changes to git (`git add .`).

The next command is very important. Run `git commit --amend` (**it's very important that you don't forget the --amend**). In this
stage you can change the commit message. Save the file (`:wq`). Now, for the final step, you are going to have to finish rebasing,
which you can do by running `git rebase --continue`. And voila you have edited a patch! **Make sure to rebuild your patches by
going back into your main proj directory (where your patches folder is) and running `./patch rb`**. 

## Getting upstream changes
Sometimes the git repo you have cloned has been worked on, and there are changes. Getting the changes and applying the patches is
a really easy process. All you have to do is run `./patch pull` which will reset the project to the last upstream commit, and then
it will pull the latest commits from the project. Then run `/patch apply` to re-apply the patches to the project.

## Resolving Patch Conflicts
However, after you get upstream changes, your patches may fail to apply due to conflicts. Now, the patch script makes it
easy to resolve these changes, but the initial set-up is a little complex.
 
The patch script uses git's mergetool to resolve
conflicts, so you should have one set up. If you don't have a mergetool set up, the patch script will default to using the
regular git merge (which is pretty ugly).

If you have a merge tool set up, make sure to specify it so the patch script knows to use it. You can specify the merge
tool by setting the `mergetool` variable inside of the patch script to the name of your merge tool. I set up a merge tool
that uses intellij's merging window (because I like the look and ease of it), but it's personal preference.

If you don't have a merge tool set up, I would recommend setting one up. A lot of people recommend the free merging tool p4merge. Just google how
to set up p4merge.

So I explained how the patch script resolves conflicts, but how do you resolve it? Pretty simple, when you encounter a patch that failed
to apply, just run `./patch resolve` and it will open up the merging tool you assigned to help you merge the patch into it. Once you finished
the merge it will automatically continue to apply further patches.

**It's important after you have resolved conflicts that you rebuild your patches with `./patch rb`**.

## Patch Authoring
Patch script allows you to change the author of your patches. This is mainly useful if you don't want to use whatever git email
you have set as the author of your patches. Simply change the `patchAuthor` variable to your desired name followed by your email
enclosed between < and >. For example, `patchAuthor="Silverwolfg11 <silverwolfg11@gmail.com>"`. It's important to note that
if you have a patch author set, all patches will be re-authored when they are re-built. 

## Updating the patch script
Sometimes, I will update the patch script to include new features. To update the patch script, just run `./patch update` and it will
automatically download a patch bootstrap, download a new patch script, delete that patch bootstrap, and leave you with the updated 
patch script. Most of the time, my updates will not require re-configuring the script, and if they do, I will update the readme to
indicate that.

## How do I use this to PR?
Well, let's say you have some patches that you rather not apply. There are two ways to go about this. You can either apply only the ones
you want, or you can unapply the ones you don't want.

For the first method, run `./patch reset` which will reset the project to the stored upstream commit. Then to apply certain patches,
run `./patch t [patch number]`. Patch number can be like 2, or 002. Representative of the patch number you want to apply.

For the second method, run `./patch fa` to apply your changes with any changes from upstream. Then to unapply certain patches, just
do `./patch ua [patch number]`.

Then you can PR, your project back to the main repo!
