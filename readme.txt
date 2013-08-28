在Linux上安装Git
	
	安装完成后，还需要最后一步设置，在命令行输入：
		$ git config --global user.name "Your Name"
			$ git config --global user.email "email@example.com"

			创建版本库
				
					第一步,  创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：
						$ mkdir learngit
							$ cd learngit
								$ pwd
										/Users/michael/learngit

											第二步，通过git init命令把这个目录变成Git可以管理的仓库：
												$ git init
														Initialized empty Git repository in /Users/michael/learngit/.git/

														把文件添加到版本库
															所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。
																强烈建议使用标准的UTF-8编码，所有语言使用同一种编码，既没有冲突，又被所有平台所支持。

																	第一步，用命令git add告诉Git，把文件添加到仓库：
																		$ git add readme.txt

																			第二步，用命令git commit告诉Git，把文件提交到仓库：
																				$ git commit -m "wrote a readme file"
																						[master (root-commit) 81324ed] wrote a readme file
																								 1 file changed, 2 insertions(+)
																										 create mode 100644 readme.txt

																											commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：
																												$ git add file1.txt
																													$ git add file2.txt
																														$ git add file3.txt
																															$ git commit -m "add 3 files."


																															版本回滚

																																现在，运行git status命令看看结果：
																																	$ git status

																																		具体修改了什么内容，需要用git diff这个命令看看：
																																			$ git diff readme.txt 

																																				提交修改和提交新文件是一样的两步，第一步是git add，第二步是git commit -m "abcde"
																																					$ git add readme.txt
																																						$ git commit -m "comment"
																																							
																																								git log命令显示从最近到最远的提交日志,  用git log命令查看历史记录：
																																									(需要友情提示的是，你看到的一大串类似“81324ed2cc4e0a7fa491f4fd9c2b1a93e83d1c9e”的是commit id（版本号），
																																										和SVN不一样，Git的commit id不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字)
																																										
																																											$ git log
																																													commit 2b73e5e39d5bc9c71f0b676a011ac7f795f16773
																																															Author: Michael Liao <askxuefeng@gmail.com>
																																																	Date:   Sun Jul 21 21:51:19 2013 +0800

																																																			    append ......

																																																					
																																																						Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本,
																																																							上一个版本就是HEAD^，上上一个版本就是HEAD^^，
																																																								当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

																																																									回退到上一个版本，就可以使用git reset命令：
																																																										$ git reset --hard HEAD^
																																																												HEAD is now at 811f666 add distributed

																																																													git log再看看现在版本库的状态：
																																																														$ git log
																																																																commit 811f66658635a62c90aa281ceee166755127c1c8
																																																																		Author: Michael Liao <askxuefeng@gmail.com>
																																																																				Date:   Sun Jul 21 21:34:12 2013 +0800
																																																																						.....

																																																																							回到最新的版本,
																																																																								找到最新的版本的commit id，假如是“2b73e5e39d5bc9c71f0b676a011ac7f795f16773"
																																																																									于是就可以指定回到未来的某个版本,版本号没必要写全，前几位就可以了，Git会自动去找。
																																																																										$ git reset --hard 2b73e5e
																																																																												HEAD is now at 2b73e5e append GPL

																																																																													找不到新版本的commit id怎么办？
																																																																														Git提供了一个命令git reflog用来记录你的每一次命令,可以得到commit id了：
																																																																															$ git reflog
																																																																																	811f666 HEAD@{0}: reset: moving to HEAD^
																																																																																			2b73e5e HEAD@{1}: commit: append GPL
																																																																																					811f666 HEAD@{2}: commit: add distributed
																																																																																							81324ed HEAD@{3}: commit (initial): wrote a readme file


																																																																																							工作区和暂存区
																																																																																								工作区（Working Directory）：就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区。
																																																																																									版本库（Repository）：工作区有一个隐藏目录“.git”，这个不算工作区，而是Git的版本库。
																																																																																											Git的版本库里存了很多东西，
																																																																																													其中最重要的就是称为stage（或者叫index）的暂存区，
																																																																																															还有Git为我们自动创建的第一个分支master，
																																																																																																	以及指向master的一个指针叫HEAD。

																																																																																																		前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
																																																																																																				第一步是用“git add”把文件添加进去，实际上就是把文件修改添加到暂存区；
																																																																																																						第二步是用“git commit”提交更改，实际上就是把暂存区的所有内容提交到当前分支。
																																																																																																								因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，commit就是往master分支上提交更改。

																																																																																																									管理修改
																																																																																																											Git跟踪并管理的是修改，而非文件。

																																																																																																													假如有如下操作过程：
																																																																																																															第一次修改 -> git add -> 第二次修改 -> git commit
																																																																																																																	那么第二次的修改就不会被提交
																																																																																																																			原因：
																																																																																																																						Git管理的是修改，当你用“git add”命令后，在工作区的第一次修改被放入暂存区，准备提交，
																																																																																																																									但是，在工作区的第二次修改并没有放入暂存区，
																																																																																																																												所以，“git commit”只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

																																																																																																																														每次修改，如果不add到暂存区，那就不会加入到commit中。

																																																																																																																															撤销修改
																																																																																																																																	工作区的修改的撤销：
																																																																																																																																				如果用git status查看一下，你可以发现，Git会告诉你，git checkout -- file可以丢弃工作区的修改：
																																																																																																																																						$ git checkout -- readme.txt
																																																																																																																																									命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
																																																																																																																																												一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
																																																																																																																																															一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
																																																																																																																																																		总之，就是让这个文件回到最近一次git commit或git add时的状态。

																																																																																																																																																					git checkout -- file命令中的“--”很重要，
																																																																																																																																																								没有“--”，就变成了“创建一个新分支”的命令，我们在后面的分支管理中会再次遇到git checkout命令。

																																																																																																																																																										暂存区的修改的撤销：
																																																																																																																																																													如果修改只是添加到了暂存区，还没有提交，
																																																																																																																																																																Git同样告诉我们，用命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区：
																																																																																																																																																																		$ git reset HEAD readme.txt
																																																																																																																																																																					Unstaged changes after reset:
																																																																																																																																																																								M       readme.txt

																																																																																																																																																																											用命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区。
																																																																																																																																																																														git reset命令既可以回退版本，也可以把工作区的某些文件替换为版本库中的文件。当我们用HEAD时，表示最新的版本。

																																																																																																																																																																																提交之后的修改的撤销：
																																																																																																																																																																																			假设你不但改错了东西，还从暂存区提交到了版本库，怎么办呢？
																																																																																																																																																																																						还记得版本回退一节吗？可以回退到上一个版本。
																																																																																																																																																																																									不过，这是有条件的，就是你还没有把自己的本地版本库推送到远程。

																																																																																																																																																																																											
																																																																																																																																																																																												删除文件
																																																																																																																																																																																													一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
																																																																																																																																																																																														$ rm test.txt
																																																																																																																																																																																																Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了。

																																																																																																																																																																																																	现在你有两个选择，
																																																																																																																																																																																																			一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且commit
																																																																																																																																																																																																					$ git rm test.txt
																																																																																																																																																																																																								rm 'test.txt'
																																																																																																																																																																																																										$ git commit -m "remove test.txt"
																																																																																																																																																																																																													[master 6a5aaa7] remove test.txt
																																																																																																																																																																																																																 1 file changed, 1 deletion(-)
																																																																																																																																																																																																																			 delete mode 100644 test.txt

																																																																																																																																																																																																																					另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本
																																																																																																																																																																																																																							$ git checkout -- test.txt
																																																																																																																																																																																																																										git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

