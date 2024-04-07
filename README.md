http://giantpandacv.com/academic/%E7%AE%97%E6%B3%95%E7%A7%91%E6%99%AE/Transformer/%E6%97%A0%E9%9C%80nms%EF%BC%8Connxruntime20%E8%A1%8C%E4%BB%A3%E7%A0%81%E7%8E%A9%E8%BD%ACRT-DETR/



1、 WSL root用户与普通用户切换
打开一个command window，
 ubuntu2004 config --default-user root
 ubuntu2004 config --default-user 普通用户名
 

2、目前 WSL 不支持 systemd（Linux 中的服务管理系统）

把 systemctl 命令换成 /etc/init.d/  例如 sudo systemctl status docker  ---》 sudo /etc/init.d/docker status
    也可以使用 service 命令，sudo service docker status
    
3、docker运行相关命令行
    
    运行容器 docker run --name bsnn_tools（运行起来的名字） -d -it -p 8888:8888(端口映射，为了jupyter) bsnn_tools:4.1.0(输入的image名字和版本) 
    ctrl + d 退出容器
    docker exec -it <容器 ID> bash  进入容器
	
	
	docker run --name bsnn_tools_4.1.0 -d -it -p 8899:8888 <CONTAINER ID>
	
	
	
	http://10.27.40.18/ 服务器上model zoo的docker创建命令
	docker run --ipc=host --gpus all --name zpt_bsnn_tool -d -it -p 8899:8888 -v /data1/bsnn_modelzoo:/workspace/model_zoo_dev -v /:/workspace/server -v /etc/localtime:/etc/localtime dal-tool:v1.0 nohup jupyter notebook --ip=0.0.0.0 --port=8888 --allow-root --NotebookApp.password=sha1:bb9985cca39b:f994b8c1673f0d1fa7b6dbee9d5276d2aa0111cd --NotebookApp.contents_manager_class='notedown.NotedownContentsManager' 
	
    
    在容器中打开jyputer 命令 jupyter-notebook   --allow-root --ip=0.0.0.0 --port=8888，参考https://www.cnblogs.com/tujia/p/13474726.html
    
    wsl中linux的ip 172.27.104.247
      
    源码文件拷贝到docker： `docker cp file <容器 id>:/home`   ## file 可以是文件，也可以是文件夹
     
    拷贝生成的boot.img到你的主机：docker cp <容器 id>:/root/src/kernel/boot.img ./      ##这都是在wsl环境下的操作的，不是在docker中操作
	
	
	复制镜像和复制容器都是通过保存为新镜像而进行的。

	具体为：

	保存镜像

	docker save ID > xxx.tar

	docker load < xxx.tar

	保存容器

	docker export ID >xxx.tar

	docker import xxx.tar containr:v1

	然后再docker run -it containr:v1 bash
	
	
	docker desktop启动失败，删除C:\Users\zhoupengtao\AppData\Roaming\Docker文件夹下面的内容就可以了
 
4、adb命令行  adb push testfile /home/root/workspace/userdata 推送文件到开发板， 在开发板上再sync同步一下，确保传输过来的文件写入到外存
               adb pull /userdata/testfile ./  在开发板上输入命令，将文件下载到本地


5、linux下文件夹压缩命令
    zip -r XXX.zip XXX(文件夹) 压缩文件夹
    unzip XXX.zip  解压文件夹
    
6、 -sh: ./bsnn_sample_batch: Permission denied  解决办法    chmod 777 bsnn_sample_batch

7、pip install -i https://pypi.tuna.tsinghua.edu.cn/simple
	
	服务器代理方式安装包
	
	pip install numpy  --proxy="http://10.27.11.1:3128"  -i https://pypi.tuna.tsinghua.edu.cn/simple

	sudo apt -o Acquire::http::proxy="http://10.27.11.1:3128" install vim
	
	git clone -c https.proxy="http://10.27.11.1:3128" https://github.com/jonny-xhl/FastReport.git

8、/opt/bstos/2.3.1.5/sysroots/aarch64-bst-linux/usr/share/samples -----bsnn samples batch脚本位置

9、ip设置 网络配置  ifconfig eth0 192.168.8.3


9、    scp -r local_folder remote_username@remote_ip:remote_folder
    scp -r local_folder remote_ip:remote_folder
    
    scp local_file remote_username@remote_ip:remote_folder
    scp local_file remote_username@remote_ip:remote_file
    
    从远程设备设备上拷贝文件：
    
    scp 远程机器@IP地址:远程文件路径/文件名 当前系统文件路径
    
10、
    $ ifconfig  // 查看所有网卡的配置信息
    $ ifconfig eth0  // 查看某网卡的配置信息，如eth0
    $ ifconfig eth0 172.16.129.108 netmask 255.255.255.0  // 配置网卡的临时生效的IP地址
    $ route add default gw 172.16.129.254  // 配置网关

11、复制文件夹下所有文件到另一个文件夹
    
    cp -r src-path  des-path

12、inux下建立软连接和查询依赖库
        ldd + 可执行文件/so库
        ln  -s   [源文件]   [软链接文件]
    
13 最近调试用的复制文件的指令
    docker cp bsnn_yolov5.cpp bc5348ba19fd:/opt/bstos/2.3.0.4/sysroots/aarch64-bst-linux/usr/share/yolov5

    scp -r yolov5_demo root@192.168.1.1://home/root/exe_file
    

    scp -r root@192.168.1.1://home/root/exe_file/b.txt .     远程把开发板的文件拷贝到本地
    

14、 在wsl中搭建编译环境
    从 hs-linux-sdk 中提取工具链
    1. 进入 docker 镜像，提取/opt/bstos 文件夹到主机目录；
    2. sudo cp -r bstos /opt/
        移动 bstos 到主机 opt 目录，建议使用该目录，无需修改后续的脚本，同时避免链接库导致的问题；
		
	docker cp bc5348ba19fd:/opt/bstos/  /opt/   ---------拷贝bstos命令行

    3. source /opt/bstos/2.3.0.4/environment-setup-aarch64-bst-linux                   
    导入环境变量
    即可编译
    

15、在开发板上使用opencv的imshow的方法步骤：
    （1）在开发板上上插入鼠标，可能需要重启开发板，hdmi连接显示器，在开发板上运行weston.sh，
        这个时候应该可以在显示屏上看到鼠标，就可以使用opencv的imshow了
        
16、修改文件夹权限  chmod 777 可执行文件

17、摄像头开始命令 camera_test -i 0 -w 1920 -h 1080 -x 
	
	打开显示屏幕的命令  weston.sh （以防止忘记）,在串口端才可以用imshow这个接口，ssh的ip端会报错

18 nohup python train.py > myout.file 2>&1 &

19 统计个数
	统计当前目录下文件的个数（不包括目录）：ls -l | grep "^-" | wc -l
	统计当前目录下文件的个数（包括子目录）：ls -lR| grep "^-" | wc -l
	

20、#bstnnx_run --config yolov5m.yaml --result_dir result_test2 --extra XTSC_NET_SIM=True --extra generate_rbf_only=True


**********************************************************************************************************************************
21、git 命令行记录

（1）git pull   同步远程仓库代码到本地。Already up-to-date代表本地代码已经更新到和远程仓库一致了

（2）git status 查看当前状态。当你忘记修改了哪些文件的时候可以使用 git status  来查看当前状态，红色的字体显示的就是你修改的文件
	git diff 可以查看具体修改的代码

（3）git add  提交代码到本地git缓存区。

	命令：git add 文件名1 文件名2 ...

	情形一：如果你git status 查看了当前状态发现都是你修改过的文件，都要提交，那么你可以直接使用 git add .  就可以把你的内容全部添加到本地git缓存区中

	情形二：如果你git status 查看了当前状态发现有部分文件你不想提交，那么就使用git add xxx(上图中的红色文字的文件链接)  就可以提交部分文件到本地git缓存区
	
	使用git add之后，可以再使用git status查看那些文件被添加进去了
	
	
	
	如果add错误了 ，可以用git reset filename来取消，如果想全部取消，则是git reset命令

（4）git commit -m "提交代码"  推送修改到本地git库中
	
	命令：git commit 文件名 -m "提交代码备注"
	
	
	
	git reset HEAD <file>，取消暂存，就是取消commit，加了file就是针对某个文件取消，不加<file>则是取消所有的commit
	
	git reset --hard origin/feature/refactor_MOT
	
（5）git push
	
	//git push <远程主机名> <远程分支名>  把当前提交到git本地仓库的代码推送到远程主机的某个远程分之上

	//比如 git push origin/feature/refactor_MOT
	
	push具有三种情况：
	（1）远程已有remote_branch分支并且已经关联本地分支local_branch且本地已经切换到local_branch
	
		git push
		
		git push origin HEAD:feature/refactor_MOT
		一般像我的做代码重构，已经关联了远程分支，则可以直接使用git push
		
	（2）远程已有remote_branch分支但未关联本地分支local_branch且本地已经切换到local_branch
		 git push -u origin/remote_branch
		 
	（3）远程没有remote_branch分支并，本地已经切换到local_branch
	
		git push origin local_branch:remote_branch
	
	
	如果发现push出问题了，想要取消，首先使用git log查看提交记录
	一般都是commit  XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX111
			Author：xxxxx
			Date：XXXXXXXXXXXXXX
			
			XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX（代码提交备注）
		
			commit  XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX222
			Author：xxxxx
			Date：XXXXXXXXXXXXXX
			
			XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX（代码提交备注）
	
	如果想要撤销commit  XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX111，那么就是要使代码返回到XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX222
	
	使用命令 git reset --soft XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX222
	
其他：
	（1）切换分支
	git branch -a  查看远程分支
	
	git branch     查看本地分支
	
	 git checkout -b  分支名  切换到新建的一个分支上， 切换到一个已有分支上，git  checkout
	 
	 git checkout refactor_MOT -b origin HEAD:feature/refactor_MOT
	如果本地有代码修改，再去切换分支，需要先把代码保存起来，切换之后，再pop出来   先git stash，切换分支， 然后再 git stash pop
	 
	git branch -d <branch>删除一个本地分支
	
	（2）撤销提交
	
	git log 可以查看每次提交的log
	
	 git revert 用法 
	 
	* git revert HEAD                  撤销前一次 commit
    * git revert HEAD^               撤销前前一次 commit
    * git revert commit （比如：fa042ce57ebbe5bb9c8db709f719cec2c58ee7ff）撤销指定的版本，撤销也会作为一次提交进行保存。
	git revert是提交一个新的版本，将需要revert的版本的内容再反向修改回去，
	 
	 
	 
	 
	 （3）切换到远程分支并作开发步骤
	 
	 git branch -r 查看全部远程分支
	 
	 
	 git checkout -b feature-branch origin/feature-branch  上述命令将创建一个本地分支，名称为 feature-branch，与远程分支 origin/feature-branch 建立绑定关系，并自动切换到该分支。
							现在，您可以在此分支上对代码进行修改并提交更改了。
	 
	 git checkout -b refactor_MOT  remotes/origin/feature/refactor_MOT
	 
	 
	 git branch -vv 则是查看本地分支与远程分支的关联关系
	 
	 删除本地分支：先切换到其他分支，再	 git branch -d <branch>
	 
	 （4）把本地修改的文件还原
	 
	 git checkout . #本地所有修改的。没有的提交的，都返回到原来的状态
	 或者
	 git checkout + 文件 
	 
	 
	 
	 (4)合并多次提交
	 
	 两种方式，1） git rebase -id  可以选择合并连续的commit，这样就不容易合并掉别人的commit
			   2） git reset  -------会将reset之前的所有commit一并合并，容易将别人的commit也一并提交了
	 
	 
	 合并中间的多次commit参考：https://blog.csdn.net/liuzubing/article/details/115697826
	 
	 取消rebase： git rebase --abort
	 
	 
	 （5） git cherry-pick用法
		对于多分支的代码库，将代码从一个分支转移到另一个分支是常见需求，
		它的功能是把已经存在的commit进行挑选，然后重新提交。比较合适的一个场景是把A分支的某次或者多次的提交也提交到B分支上，使用方法
	 
	 具体用法参考：https://blog.csdn.net/a1056244734/article/details/112908080
*************************************************************************************************************************************


















# My Python Examples.

Here is some more detailed information about the scripts I have written.  I do not consider myself a programmer; I create these little programs as experiments to have a play with the language, or to solve a problem for myself.  I would gladly accept pointers from others to improve the code and make it more efficient, or simplify the code.  If you would like to make any comments then please feel free to email me at craig@geekcomputers.co.uk.

In the scripts the comments etc are lined up correctly when they are viewed in [Notepad++](https://notepad-plus-plus.org/). This is what I use to code Python scripts.

- [batch_file_rename.py](https://github.com/geekcomputers/Python/blob/master/batch_file_rename.py) - This will batch rename a group of files in a given directory, once you pass the current and new extensions.

- [create_dir_if_not_there.py](https://github.com/geekcomputers/Python/blob/master/create_dir_if_not_there.py) - Checks to see if a directory exists in the users home directory, if not then create it.

- [Fast Youtube Downloader](https://github.com/geekcomputers/Python/blob/master/youtube-downloader%20fast.py) - Downloads youtube videos fastly with parallel threads using aria2c

- [Google Image Downloader](https://github.com/geekcomputers/Python/tree/master/Google%20Image%20Downloader) - Query the specific term and retrieve the images from google image database.

- [dir_test.py](https://github.com/geekcomputers/Python/blob/master/dir_test.py) - Tests to see if the directory `testdir` exists, if not it will create the directory for you.

- [env_check.py](https://github.com/geekcomputers/Python/blob/master/env_check.py) - This script will check to see if all of the environment variables I require are set.

- [fileinfo.py](https://github.com/geekcomputers/Python/blob/master/fileinfo.py) - Show file information for a given file.

- [folder_size.py](https://github.com/geekcomputers/Python/blob/master/folder_size.py) - This will scan the current directory and all subdirectories and display the size.

- [logs.py](https://github.com/geekcomputers/Python/blob/master/logs.py) - This script will search for all `*.log` files in the given directory, zip them using the program you specify and then date stamp them.

- [move_files_over_x_days.py](https://github.com/geekcomputers/Python/blob/master/move_files_over_x_days.py) - This will move all the files from the source directory that are over 240 days old to the destination directory.

- [nslookup_check.py](https://github.com/geekcomputers/Python/blob/master/nslookup_check.py) - This very simple script opens the file `server_list.txt` and then does an nslookup for each one to check the DNS entry.

- [osinfo.py](https://github.com/geekcomputers/Python/blob/master/osinfo.py) - Displays some information about the OS on which you are running this script.

- [ping_servers.py](https://github.com/geekcomputers/Python/blob/master/ping_servers.py) - This script depending on the arguments supplied, will ping the servers associated with that application group.

- [ping_subnet.py](https://github.com/geekcomputers/Python/blob/master/ping_subnet.py) - After supplying the first 3 octets it will scan the final range for available addresses.

- [powerdown_startup.py](https://github.com/geekcomputers/Python/blob/master/powerdown_startup.py) - This goes through the server list and pings the machine, if it's up it will load the putty session, if it's not it will notify you.

- [puttylogs.py](https://github.com/geekcomputers/Python/blob/master/puttylogs.py) -  This zips up all the logs in the given directory.

- [script_count.py](https://github.com/geekcomputers/Python/blob/master/script_count.py) - This scans my scripts directory and gives a count of the different types of scripts.

- [script_listing.py](https://github.com/geekcomputers/Python/blob/master/script_listing.py) - This will list all the files in the given directory, it will also go through all the subdirectories as well.

- [testlines.py](https://github.com/geekcomputers/Python/blob/master/testlines.py) - This very simple script opens a file and prints out 100 lines of whatever is the set for the line variable.

- [tweeter.py](https://github.com/geekcomputers/Python/blob/master/tweeter.py) - This script allows you to tweet text or a picture from the terminal.

- [serial_scanner.py](https://github.com/geekcomputers/Python/blob/master/serial_scanner.py) contains a method called ListAvailablePorts which returns a list with the names of the serial ports that are in use in our computer, this method works only on Linux and Windows (can be extended for mac osx). If no port is found, an empty list is returned.

- [get_youtube_view.py](https://github.com/geekcomputers/Python/blob/master/get_youtube_view.py) - This is very simple python script to get more views for your youtube videos. Sometimes I use it for repeating my favorite songs by this script.

- [CountMillionCharacter.py](https://github.com/geekcomputers/Python/blob/master/CountMillionCharacter.py) And [CountMillionCharacter2.0](https://github.com/geekcomputers/Python/blob/master/CountMillionCharacters-2.0.py).py - This Script is used for 1.counting character script 2.count how much characters are present on any text based file.

- [xkcd_downloader.py](https://github.com/geekcomputers/Python/blob/master/xkcd_downloader.py) - Downloads the latest XKCD comic and places them in a new folder "comics".

- [timymodule.py](https://github.com/geekcomputers/Python/blob/master/timymodule.py) - A great alternative to Pythons 'timeit' module and easier to use.

- [calculator.py](https://github.com/geekcomputers/Python/blob/master/calculator.py) - Uses Python's eval() function to implement a calculator.

- [Google_News.py](https://github.com/geekcomputers/Python/blob/master/Google_News.py) - Uses BeautifulSoup to provide Latest News Headline along with news link.

- [cricket_live_score](https://github.com/geekcomputers/Python/blob/master/Cricket_score.py) - Uses BeautifulSoup to provide live cricket score.

- [youtube.py](https://github.com/geekcomputers/Python/blob/master/youtube.py) - Takes input a song name and fetches the youtube url of best matching song and plays it.  
