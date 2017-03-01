
#Git_Dropbox


## Create a private Git repository using Dropbox

Set up bare Git repositories in our Dropbox folders and use them as our Git remotes.

## Setup

So let's go through the process of setting up what I'm talking about. First, in your Dropbox folder, create a place for all your repositories to live (mine is a top-level folder named "Git"):

Now, in a Terminal, cd into that folder and create a bare repository:

    $ cd ~/Dropbox/Git
    $ git init --bare mytestrepo.git
      Initialized empty Git repository in /Users/abeym/Dropbox/Git/mytestrepo.git/
      
You should see the new repository (it's just a folder) show up now:

Inside, you'll see the guts of the repo:

That's really all you need to do to get the bare repository set up. Let's try using it now. First let's make a sample project with a couple of commits:

    $ cd ~
    $ mkdir testrepo
    $ cd testrepo
    $ git init
      Initialized empty Git repository in /Users/abeym/testrepo/.git/
    $ echo "Some Text" > testfile.txt
    $ git add testfile.txt
    $ git commit -m "Add a test file"
    $ echo "More text" >> testfile.txt
    $ git commit testfile.txt -m "Add more text"
    $ cat testfile.txt
      Some Text
      More text
    $ git log
      commit ...
          Add more text

      commit ...
          Add a test file


Now we can hook up our new bare repository as an upstream for this one. And push to it:

    $ git remote add origin ~/Dropbox/Git/mytestrepo.git
    $ git push -u origin master
      Counting objects: 6, done.
      Delta compression using up to 4 threads.
      Compressing objects: 100% (2/2), done.
      Writing objects: 100% (6/6), 458 bytes, done.
      Total 6 (delta 0), reused 0 (delta 0)
      Unpacking objects: 100% (6/6), done.
      To ....
       * [new branch]      master -> master
      Branch master set up to track remote branch master from origin.


You should see your Dropbox icon spin for a second while it syncs the files for you, though you'll still notice that inside your Dropbox version of the repository, you still don't see any testfile.txt, because again, only the guts of the repository appear here.

Let's try cloning from the bare repository to make sure our files and content are actually stored there. On the same computer (or another one that Dropbox syncs to), clone the repository:

    $ cd ~
    $ git clone ~/Dropbox/Git/mytestrepo.git testrepo2
      Cloning into 'testrepo2'...
      done.

Now we'll make sure we have both our current files and our history:

    $ cd testrepo2
    $ ls
      testfile.txt      
    $ cat testfile.txt
      Some Text
      More text
    $ git log
      commit ...
          Add more text

      commit ...
          Add a test file


## A quick note about problems before we go:

If you try to pull or push while files are still syncing, or if something gets messed up in the repository, you may see an error message like this:

    error: unable to find ea2ae105b8955b3f73d79065bc52ee3126cd2e4f
    error: refs/heads/master does not point to a valid object!
    Your configuration specifies to merge with the ref 'master'
    from the remote, but no such ref was fetched.

The error may vary depending on what's gone wrong, but the solution in any case is the same: wait until Dropbox finishes syncing to make sure it's not just out-of-date files, then if the problem still exists, delete your Dropbox repository (mytestrepo.git) and create a new bare repository with the same name in its place:

    $ cd ~/Dropbox/Git
    $ rm -rf mytestrepo.git
    $ git init --bare mytestrepo.git
      Initialized empty Git repository in /Users/abeym/Dropbox/Git/mytestrepo.git/

Then push to it from the most up-to-date copy of the code:

    $ git push origin master
      Counting objects: 9, done.
      ...
      * [new branch]      master -> master

That should restore your repository state (as long as the pushing copy of the code was entirely up-to-date).
