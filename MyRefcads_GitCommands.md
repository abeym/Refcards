# Helpful Git Commands


#### How can I download only a specific folder / sub-folder or directory from a remote Git repo hosted on GitHub?

Git doesn't support this, but Github does via SVN. If you checkout your code with subversion, Github will essentially convert the repo from git to subversion on the backend, then serve up the requested directory.

Here's how you can use this feature to download a specific folder. I'll use the popular javascript library lodash as an example.

Get the repo URL. First, copy the URL of the Github repo to your clipboard. github repo URL example
Modify the URL for subversion. I want to download the folder at /docs from the master branch, so I will append trunk/docs. Full URL is now https://github.com/lodash/lodash/trunk/docs. See my note below for a more in-depth explanation of why we must use this URL format.
Download the folder. Go to the command line and grab the folder with SVN.  
svn checkout https://github.com/lodash/lodash/trunk/docs
You might not see any activity immediately because Github takes up to 30 seconds to convert larger repositories, so be patient.

Full URL format explanation:

If you're interested in master branch, use trunk instead. So the full path is trunk/foldername
If you're interested in foo branch, use branches/branchname instead. The full path looks like branches/branchname/foldername
Protip: You can use svn ls to see available tags and branches before downloading if you wish
That's all! Github supports more subversion features as well, including support for committing and pushing changes.

----

#### Setup git repo on onedrive.

Go to onedrive folder in your pc and create a folder for git repository.
Usually onedrive resides in `C:\Users\%username%\OneDrive`

Open GitBash

cd ~/OneDrive

mkdir git

cd git 

mkdir myproject

cd myproject

git init

git config --bool core.bare true



Now go to your workspace.. generally this will be any folder in your PC, outside of OneDrive.. 

Never create your workspace in onedrive or it will save all the files from node_modules.

Lets say worspace is in D:\workspace

Open GitBash

cd D:/workspace

git clone ~/OneDrive/git/myproject

cd myproject

touch testfile.txt

git add .

git commit -m "initial commit"

git push origin master


Congratulation.. you have a private git repository on cloud.
