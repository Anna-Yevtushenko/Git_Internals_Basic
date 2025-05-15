#  Part 1: Git Internals Basic


| Command                        | Meaning                                                              |
|-------------------------------|----------------------------------------------------------------------|
| mkdir gitTest                 | Create a new folder                                                  |
| cd gitTest                    | Change directory to `gitTest`                                       |
| git init                      | Initialize a Git repository and create the hidden `.git` folder     |
| ls -a .git                    | List all files (including hidden) in the `.git` directory            |
| cat .git/HEAD                 | Show the branch that Git currently points to                        |
| cat .git/description          | Repository description (used mainly in bare repositories)           |
| cat .git/info/exclude         | Local ignore rules not tracked by Git                               |
| cat .git/config               | Configuration settings for the repository                           |
| ls -l .git/hooks              | List of hook scripts that can run on Git events       |
| ls -l .git/refs/heads         | List of local branches                                               |
| ls -l .git/refs/tags          | List of tags                                                         |
| ls -l .git/objects            | List of object directories by hash prefix     |
| ls -l .git/objects/info       | Contains auxiliary info about Git objects       |
| ls -l .git/objects/pack       | Shows if Git objects are compressed into pack files                 |

<img src="https://github.com/user-attachments/assets/a0e248eb-5414-47fc-8555-1c54829e2836" style="height: 300px;"/>
</div>

I have two new directories `4b/` and `e1/` but my new file is not yet visible in `.git/objects`

<img src="https://github.com/user-attachments/assets/c64dc322-7d2b-4695-8f9f-b3691828efe7" style="height: 170px;"/>
</div>


After the `git add 1.txt` command, the `ac` folder appeared, and inside the file 
5ab8fbdb5f9b410cba904145f975593182dced is the generated hash for the `1.txt` file

<img src="https://github.com/user-attachments/assets/20a1d531-2e39-415a-94af-3f805231af26" style="height: 170px;"/>
</div>

After the command `git restore --staged 1.txt` - nothing changed

<img src="https://github.com/user-attachments/assets/560fbf14-9e4a-4fa8-97a7-24c75202248f" style="height: 370px;"/>
</div>

When we created the first non-empty commit, the `ec/` folder appeared, which contains the commit object and the `1d` -tree object (directory structure)

<img src="https://github.com/user-attachments/assets/b50cd7ef-e52c-4349-8cd7-55ab1c460fc4" style="height: 300px;"/>
</div> 

**git log** - commit history starting from the one pointed to by HEAD (in my case this is the last commit)

**cat .git/HEAD** - indicates which branch HEAD is currently on

**cat .git/refs/heads/main** - hash of the last commit

<img src="https://github.com/user-attachments/assets/2b30e4af-4834-42f3-8c04-d067370192a3" style="height: 330px;"/>
</div> 


**git log** - now starts with the latest commit 

**cat .git/refs/heads/main** - now points to the hash of the new commit

<img src="https://github.com/user-attachments/assets/5a2dbe80-9d78-4d49-b7bf-7227ae504fb2" style="height: 230px;"/>
</div> 

**echo ecbf7e7fc72a91e8b1943ffb26a5b568b2ca4442 > .git/refs/heads/main** - 

here I manually moved the main branch back, and now Git will think that this commit is the last in the main branch

`git log` now shows me the history from the commit that the branch now points to

<img src="https://github.com/user-attachments/assets/d2b4a515-d62d-4539-ad1a-a9039b222db8" style="height: 330px;"/>
</div> 

turned everything back

<img src="https://github.com/user-attachments/assets/db1455b1-829b-45e2-9cdf-d834588a4ab1" style="height: 330px;"/>
</div>

`git branch` - shows which branch you are currently on and all other local branches

`git branch test_1_branch` - creates a new branch

`git log` - shows branches that point to the last commit

`git branch` - after creating a new branch will show two branches and highlight the one you are currently on

When a new branch is created, a new file is created in .git/refs/heads/. - which points to the latest commit of each branch

```
.git/refs/heads/test_1_branch

.git/refs/heads/main
```

<img src="https://github.com/user-attachments/assets/516397d5-56c5-4236-b292-523d2cfacb64" style="height: 330px;"/>
</div> 

<img src="https://github.com/user-attachments/assets/beb66cec-800f-4daf-9dd1-c7f53601b30e" style="height: 130px;"/>
</div> 

`echo HASH_OF_YOUR_SECOND_COMMIT > .git/refs/heads/test_2_branch` - manually create a branch that points to the hash of the second commit. 

During `git log` - displays it in the list of branches that point to the corresponding (last) commit.

`rm .git/refs/heads/test_1_branch` - delete the branch, i.e. delete the file. And git branch no more displays this branch

<img src="https://github.com/user-attachments/assets/a0b5089e-47ae-4c02-960b-43a026f60501" style="height: 260px;"/>
</div> 

`git log --oneline` - prints each commit on one line
(shortened hash - takes the first 7 characters, shows branches that point to the commit, commit description)

**main branch** - was active when we created the new commit, so it moved forward

<img src="https://github.com/user-attachments/assets/7ed8866e-61bc-4a9f-925b-c12934f0ecb8" style="height: 330px;"/>
</div> 


`echo "ref: refs/heads/test_2_branch" > .git/HEAD` - write the instruction to `.git/HEAD`, now **HEAD** points to the branch **test_2_branch** instead of **main**

`git log` - shows the commit history from the branch **test_2_branch**, instead of **main**

**HEAD** -> the file that the user is currently looking at

<img src="https://github.com/user-attachments/assets/1b3da4fc-070e-4508-9264-4929c4b48f9d" style="height: 230px;"/>
</div> 

`rm .git/HEAD` - deleted the HEAD file, which indicated which branch/commit to look at

`git status` and `git log` - do not work, because GIT now cannot determine the active branch/commit

So without HEAD - Git considers the repository broken

<img src="https://github.com/user-attachments/assets/4ca966cd-9a7c-4c53-be8f-4ce7ad2cb15b" style="height: 330px;"/>
</div> 

`echo "ref: refs/heads/main" > .git/HEAD` - restored the HEAD file

`git status` - shows if there are any new non-commit files on the main branch

`git log` - now prints the commit history

<img src="https://github.com/user-attachments/assets/434d161d-1e17-4955-9a0c-55ccd0e97627" style="height: 330px;"/>
</div> 

`git checkout` - the command that is needed to switch to another branch, and when we switch to a branch, it becomes active. Also the **.git/HEAD** file - now points to this branch instead of the previous one

`.git/HEAD` - the active branch

`.git/refs/heads/main` - all branches and their latest commit hash

<img src="https://github.com/user-attachments/assets/ae4b5af4-aec6-44ee-ae6f-e424a7a3acc2" style="height: 330px;"/>
</div> 

`ls -l .git/logs` - internal Git logs that help you restore or view history even after branch changes or loss of access

shows the total size of the folder contents(4)
the folder has two objects HEAD and refs

`ls -l .git/logs/HEAD` - history of HEAD moves

`ls -l .git/logs/refs/heads/main` - history of changes to the main branch

`ls -l .git/logs/refs/heads/test_2_branch` - log file missing because there was still a commit in this branch

#  Part 2: Exploring Git Objects in Detail

<img src="https://github.com/user-attachments/assets/5d2f9b50-22dd-4c3f-84f2-ca249e27e16e" style="height: 170px;"/>
</div> 

`cat .git/objects/6b/2d9cc55f49b25ce78f7e4e8f6f9d1b64992e5f` - output unreadable code with garbage (objects in git are stored in zlib format)
further using 
`python3 -c 'import sys,zlib; sys.stdout.buffer.write(zlib.decompress(open(sys.argv[1], "rb").read()))' .git/objects/6b/2d9cc55f49b25ce78f7e4e8f6f9d1b64992e5f` - unpacked the contents:

- object type 
- tree reference
- parent commit
- commit author
- commit text
  
<img src="https://github.com/user-attachments/assets/2ed27e4e-c0b9-44ff-b999-2bae426255cb" style="height: 170px;"/>
</div> 

**blob** - representation of file, commit, tree, and so on) and then file content.

a more convenient option for displaying information about the contents of an object by hash. The information itself

<img src="https://github.com/user-attachments/assets/2d7dce67-e72a-4730-b7f1-8b7960fa167c" style="height: 200px;"/>
</div> 

`git cat-file -t <hesh_object_hash>`- to determine the type of object (tree)

`git cat-file -p d1f29664fcadbbab51281b42b1d440f8a935d70a` - let's look at the tree:

- file permissions
- object type
- object hash
- file name
  
**blob** - is the contents of a single file. It stores only the text of the file, without the name and path.
  
**tree** - is the folder structure. It indicates which files (and folders) are included in the commit, their names, permissions, and corresponding blob hashes.

**commit** - is a snapshot of the project. It stores a link to the tree, has a message, date, author, and previous commit.

<img src="https://github.com/user-attachments/assets/e8ba669b-0494-46cd-aab4-201100d7ca9f" style="height: 330px;"/>
</div> 

`git init -b main` - create a new repository in the current folder with the main branch

<img src="https://github.com/user-attachments/assets/9fb60346-75d3-4a8b-bfb8-e2a8aab1c5da" style="height: 330px;"/>
</div> 

we have commit 13/9d8a14d04ce72acc12765367aa8864b8b0d073 -> refers to tree d882f30fe536ad463f3d0d382a45c11425ad6e05 -> this tree d882f30fe536ad463f3d0d382a45c11425ad6e05 contains file 1.txt ->

<img src="https://github.com/user-attachments/assets/495fd78d-ca38-4fcb-a255-a6ac4649c41c" style="height: 90px;"/>
</div> 

refers to blob 89b24ecec50c07aef0d6640a2a9f6dc354a33125 which has content line1

<img src="https://github.com/user-attachments/assets/43bfb90a-6e0a-4fba-a7dc-8d71b626b11a" style="height: 330px;"/>
</div> 

`git log –oneline` - shows a simplified commit history

`git show ecbf7e7^` - previous commit for commit with unique hash ecbf7e7, displays the following information:

- full hash
- Author
- Date

<img src="https://github.com/user-attachments/assets/9a82d485-5bf7-4b7e-98ee-4ed04cd2e54b" style="height: 250px;"/>
</div> 

<img src="https://github.com/user-attachments/assets/b6051bac-bf26-42dc-b2b9-8062a1dc3600" style="height: 250px;"/>
</div> 

<img src="https://github.com/user-attachments/assets/aee2d60c-9bf6-4d17-8bc2-a55d714ef258" style="height: 250px;"/>
</div> 

Generated three text files (patch) - archive of changes of each commit

Let's initialize a Git repository inside. Note that we can use the -C flag to specify the folder in which our Git repo is placed - it is very useful for scripts.

```
annnnnet@DESKTOP-15LRFBI MINGW64 ~
$ git -C repo1 init
Initialized empty Git repository in C:/Users/annnnnet/repo1/.git/
```

<img src="https://github.com/user-attachments/assets/5874a3f6-e3f6-4111-b9df-138a1cfc80e8" style="height: 170px;"/>
</div> 


Go to the repo1 directory and run git init"
(i.e. initialize a new Git repository in the repo1 folder without leaving the current directory)

<img src="https://github.com/user-attachments/assets/5d0c69fa-f975-4ea4-b72c-f15bdfc60efa" style="height: 170px;"/>
</div> 

make a copy of repo1 in repo2 and output the history then compare it with the history in repo1

<img src="https://github.com/user-attachments/assets/b70caf83-a599-4cf2-895b-6a5b1e4dfd23" style="height: 250px;"/>
</div> 

We can see that the history is the same

Check **.git/objects** in both folders


<img src="https://github.com/user-attachments/assets/42136126-dc61-47b7-8872-c376696ee5a0" style="height: 250px;"/>
</div> 

if the .git/objects hashes are the same then it means the commits and files contents are the same

Created three commits and outputted history

<img src="https://github.com/user-attachments/assets/d5367115-65d2-46fb-9d20-df7c741fce39" style="height: 250px;"/>
</div> 

use format-patch to get the last 2 commits from repo1
the command used and the names of the .patch files generated in the repo1 directory.

<img src="https://github.com/user-attachments/assets/a99d3350-26a1-4194-86bf-9dd22c7ba767" style="height: 170px;"/>
</div> 

the contents of the generated .patch files:

<img src="https://github.com/user-attachments/assets/f6525316-eec4-4365-b574-8e67887ab264" style="height: 230px;"/>
</div> 

What does a patch file consist of:
- Commit hash
- Commit author
- Commit date
- Commit text
- List of changed files


<img src="https://github.com/user-attachments/assets/41c2cedb-f47e-4fc9-9aa5-be747f0a76e4" style="height: 170px;"/>
</div> 

<img src="https://github.com/user-attachments/assets/62738e4d-14d0-4760-9654-7c33b80892fc" style="height: 230px;"/>
</div> 


We can see that the commit hashes are different, because different hashes are generated because their generation is affected by time, author, and parent-commit

make three simple commits. After the first commit, create a new branch git branch test_cherrypick

Report the commands used to switch to the test_cherrypick branch and cherry-pick the second and third commits from the main branch. Report the output of git log after the cherry-picks. Compare the hashes of the cherry-picked commits on the test_cherrypick branch with the original commits on the main branch(git log main). Explain why the hashes are (or are not) the same.


<img src="https://github.com/user-attachments/assets/f1cd9b92-ca6e-470e-b79c-d57506ba2a20" style="height: 370px;"/>
</div> 


We have the same first commit, but after cherry-picking the second and third commits we got a different hash, but the same commit content.
This is because cherry-pick does not make a direct copy of the commit, but creates a new unique commit.

Create **test1** folder and add initial empty commit

**Observation:** Note the commit hash.

We have the same first commit, but after cherry-picking the second and third commits we got a different hash, but the same commit content.
This is because cherry-pick does not make a direct copy of the commit, but creates a new unique commit.

Create **test1** folder and add initial empty commit

**Observation:** Note the commit hash.

<img src="https://github.com/user-attachments/assets/924e3b4a-32d4-4953-8a64-7452d69ae077" style="height: 80px;"/>
</div> 

Copy the folder
`cp -r test1 test2`
Confirm that test2 has the same commit history as test1.

<img src="https://github.com/user-attachments/assets/fc27f9cc-54d4-4ed0-b843-9b0b0b605928" style="height: 230px;"/>
</div> 

**Observation**: Report the command used to add a remote named myLocalRepo pointing to the **test2** directory. Also, report the output of git remote -v to verify the remote was added correctly.

<img src="https://github.com/user-attachments/assets/3d947e73-45db-4ad0-9307-123b45e18846" style="height: 170px;"/>
</div> 

37.	Create new file in **test1**
38.	Commit the file
39.	Report the commands used to add and commit the **1.txt** file in the **test1** repository. Note the new commit hash.

<img src="https://github.com/user-attachments/assets/4dbb2ecb-845e-4a63-b0be-c53b27a75056" style="height: 330px;"/>
</div> 

Push current hash as a new remote branch inside test2 folder aka myLocalRepo remote

<img src="https://github.com/user-attachments/assets/0978fee9-d74a-485c-a57f-d9fd91e2a671" style="height: 170px;"/>
</div> 

Switch to test2 folder:

Confirm that the myNewBranchInTest2 branch now exists.

<img src="https://github.com/user-attachments/assets/850738db-9f1d-406f-9251-d95ba194b665" style="height: 170px;"/>
</div> 


<img src="https://github.com/user-attachments/assets/2ff6822e-dfe0-44d6-987b-2c3a325d1ca9" style="height: 230px;"/>
</div> 

