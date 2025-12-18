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
ls -a -> to show up all of type of files, including hidden file
git branch -M main -> got main branch which previously you need to do "git init" first

PS*: after above git command, we need to create remote repos, but the problem is how will you connect your local
     repos to the remote repos? that's a big issue.
     now basically there are two optons one is HTTPS and one is SSH.
     so first of all, when we use HTTPS, basically to push, how do i know that who should be allowed?
     now thats where when we use HTTPS, it will ask you to login. 
     otherwise, you can use SSH, the problem with login is you have to login every time.
     what if you can set up your machine once with a key and then every time you do something,
     it will automatically detect who you are. and thats why you have to setup the SSH.
     in order do this, can be used "ssh-keygen -o" and when you enter it will be generating private/public keys.
     once the SHA256 file is created, we just need to go back to the folder for one step and type "ls -a".
     then will appear .ssh folder, then goes to the folder by typing "cd .ssh" -> "ls -a" -> "cat id_rsa.pub" -> we got the KEY.
     then, copy the entire key -> Goes to Github -> Settings -> SSH and GPG keys -> New SSH Key -> Add SSH Key. that's it.
     and now your client and your server is connected. so connected does not mean it's continuously connected.
     every time you push something, it'll verify, okay, it's coming from a right person because I can see the SSH key,
     which is mentioned here. and on the local machine, they are the same.

git remote add origin git@github.com:thatperson63/git-course.git -> we are doing git remote cuz thats remote repos of Github
git push -u origin main -> to push the data/code from the local copy to the remote copy. u means upstream.

PS*: once you mapped it, now we have to move this file, which is in the local repos, the remote is empty at this point.
     so what i will do is to push the code, by git push and enter. it's done. now to verify, just refresh the page and
     we got the files which we created in the local machine.