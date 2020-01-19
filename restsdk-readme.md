## 环境搭建 
1. 安装VMware-workstation-full-15.1.0+ubuntu18.04.02  
2. 设置`PC本地IP:192.168.2.120`，`v4主板IP：192.168.2.180`     
3. 交工具链:***gcc-linaro-7.4.1-2019.02-x86-64-aarch64-linux-gnu***  
4. V4主板、串口线、1000M网线、等  
5. 安装串口调试工具SecureCRT  
6. 安装tftp32工具  
  
## 下载、编译restsdk及其依赖库  
### Boost 库  
1. 下载[boost-1.63.0.zip](https://sourceforge.net/projects/boost/files/boost/1.63.0/)。  
2. 解压boost-1-63-0.zip
```  
$ 7z x boost-1-63-0.zip 
``` 
3. 编译 
``` 
$ cd boost-1-63-0  
$ mkdir usr-room    # --prefix对应的install dir  
$ ./bootstrap.sh  --prefix=usr-room  
$ vim project-config.jam    
编辑project-config.jam，修改using gcc成交叉编译方式：  
using gcc  :  :    
/home/test/gcc-linaro-7.4.1-2019.02-x86-64-aarch64-linux-gnu/bin/aarch64-linux-gnu-g++ ;   
注意：空格” ”，”**:**”，”**;**”号前后都要有一个空格。  
编辑后保存 project-config.jam。    
$ ./bjam install   
```  
### Openssl库  
1. 下载[openssl-1.0.2d-src.tar.gz](https://sourceforge.net/projects/openssl/files/openssl-1.0.2d-fips-2.0.10/)。  
2. 解压openssl-1.0.2d-src.tar.gz。 
``` 
$ tar -xzvf  openssl-1.0.2d-src.tar.gz  
```
3. 进入openssl-1.0.2d目录，并编译。  
```
$ cd openssl-1.0.2d  
$ mkdir usr-room   # --prefix对应的install dir  
$ ./config no-asm -shared \  
--prefix=/home/test/WorkSpace/cpprestsdk-cross/openssl-1.0.2d/usr-room  
$ vim Makefile   
编辑Makefile 修改：CC=aarch64-linux-gnu-gcc  
搜索-m64选项并删除，共两处。   
$ make install  
```  

### websocketpp-master库  
1. 下载[websocketpp-master.zip](https://github.com/zaphoyd/websocketpp)。  
2. 解压websocketpp-master.zip。  
```
$ 7z x websocketpp-master.zip  
```
3. 根据websocketpp-master readme.md描述：WebSocket++ is a header only C++ library，所以websocketpp-master不需要编译，使用时直接引用其头文件。  
    
### cpprestsdk-master库  
1. 下载[cpprestsdk-master.zip](https://github.com/Microsoft/cpprestsdk)。  
2. 解压cpprestsdk-master.zip。  
```
$ 7z x cpprestsdk-master.zip  
```
3. 进入cpprestsdk-master目录，修改cpprestsdk-master/Release/CmakeLists.txt，cpprestsdk-master/Release/cmake/*.cmake文件，具体修改处见源文件，修改的地方都有注释。  
4. 新建编译目录  
```
$ cd cpprestsdk-master  
$ mkdir build.debug  
$ cd build.debug  
$ mkdir usr-room  
```
5. 编译cpprestsdk-master  
```
$ cmake  ..  -DCMAKE-BUILD-TYPE=Debug -DCMAKE-SYSTEM-NAME=Linux \  
-DCMAKE-CXX-COMPILER=aarch64-linux-gnu-g++ \  
-DCMAKE-SYSTEM-PROCESSOR=arm \  
-DCMAKE-INSTALL-PREFIX=usr-room  
$make install   
```
## 调用restsdk  
1. 将boost库文件、openssl库文件、websocketpp库文件、cpprestsdk库文件拷贝在文件夹lib-room中。  
2. 将boost头文件、openssl头文件、websocketpp头文件、cpprestsdk头文件拷贝在文件夹include-room中。  
3. 参考[官方restsdk测试工程](https://github.com/Microsoft/cpprestsdk/wiki/Getting-Started-Tutorial)，新建一个测试工程，测试工程存放在文件夹restsdk-test中。  
4. 编译测试工程。  
```
$ cd restsdk-test  
$make  
```
5. 将lib-room、restsdk test .out文件压缩，并放入tftp32文件夹。 
``` 
$ tar  -cvf  ./restsdk-test.rar  ./ restsdk-test  
```
6. 上电主板，linux起来后，将restsdk-test.rar下传到/mnt/common目录。  
```
$ cd /mnt/common  
$ tftp -g -r restsdk-test.rar  192.168.2.120  
```
7. 解压、运行.out文件  
```
$ tar -xvf restsdk-test.rar  
$ chmod -R 777 ./*  
$ export LD-LIBRARY-PATH=$LD-LIBRARY-PATH:/mnt/common/restsdk-test/lib-room  
$ route add default gw 192.168.2.1  
$cd /mnt/common/restsdk-test  
$./hello-restsdk &  
```
注：运行成功后，会生成一个保存搜索结果的html文件。  



