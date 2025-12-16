here's the flow of git: Working Directory => Staging Area (index) => Commit History (objects)

git init -> create a local repository (by default will give you a master)
rm -rf .git -> delete git
git init -b main -> create local repos with git main NOT master
git rm --cached <file> -f -> to unstage, f for forces the removal
git add <file> or git add . or git add --a -> add changes from all tracked or untracked files
git commit or git commit -m "commit message" -> to commit the changes
main (root-commit) 3bed821 -> this number is checksum (3bed821), checksum is something as a fingerprint for a data.
                              so example, every time I change something in a data, it will change the checksum as well. git creates a checksum for every commit, even for small changes. checksum is
                              consisted of 40 characters, but it's only showing the 7 characters
git log -> give you the log of all the commit
git log --oneline -> give you log summary of the commit
git restore <file> -> to discard changes in working directory
git commit -a -m "second commit" -> -a here is to move the file from working directory to the commits (skip staging 
                                    area)
git diff -> can be used if you have file in the working directory, and if you want to check the differences
git diff --staged  -> can be used if your file is into staging area, then you have to use --staged to check the     
                      changes
tig -> better version of UI to check git log
-a -> means of adding directly in one line itself

PS*: to delete certain file in our git repos, firstly, you have to delete your file from the Git.
     by git rm --cached <file> and now it's untracked (U) and you can happily delete the local file now.

git clone <url address> -> to clone git repos
ls -a -> to show up all of type of files, including hidden file.