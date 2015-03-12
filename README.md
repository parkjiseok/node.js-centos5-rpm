# Node.js RPM for CentOS 5 32bit

Three RPM files in here is for install node.js on CentOS 5. I have tested it at CentOS 5.3 32bit, but it may work for 5.x 32bit. 

I have compiled those three RPMs by myself. Hence if you use same CentOS version as me. You just need to install this rpm package. If you want to compile your own rpm. I also attach my steps on below.

### File descrition

- **nodejs-0.10.33-1.i686.rpm**: node.js 0.10.33 for CentOS 5 32bit.
- **nodejs-0.12.0-1.i686.rpm**: node.js 0.12.0 for CentOS 5 32bit.
- **nodejs-0.12.0-1.x86_64.rpm**: node.js 0.12.0 for CentOS 6.5 64bit.

### Installation

Below is a example to install node.js 0.10.33. Make sure you are a root user.
```bash
# wget https://github.com/spacesix/node.js-centos5-rpm/blob/master/nodejs-0.10.33-1.i686.rpm?raw=true --no-check-certificate
# rpm -ivh nodejs-0.10.33-1.i686.rpm
```
You can test it once install finished.
```bash
# node -v
v0.10.33
# npm -v
1.4.28
```
To uninstall(In case if you need it~).
```bash
# rpm -e nodejs-0.10.33-1.i686.rpm
```

Congratulate! you done all the installation steps. 

### Compile your own RPM

If you want to compile your own rpm. please refer to my steps below.

My server is CentOS 5.3 32bit. Another version maybe difference. please refer to [here](http://serverfault.com/questions/299288/how-do-you-install-node-js-on-centos)

**Preparatory steps**

To compile node.js. we need python2.6. Default version came with CentOS 5.3 is 2.4. Hence we need to install it first.

Again, make sure you are a root user
```bash
# yum install gcc-c++ openssl-devel
# yum install python26
```
Install FPM
```bash
# yum install ruby ruby-devel rubygems gcc
```
After above step. you ruby version could be 1.8.5, but to install fpm, we need 1.8.7
```bash
# wget http://centos.karan.org/el5/ruby187/kbs-el5-ruby187.repo
# mv kbs-el5-ruby187.repo /etc/yum.repos.d/
# yum install ruby
# ruby -v
ruby 1.8.7 (2010-06-23 patchlevel 299) [i386-linux]
# gem install fpm
# yum install rpm-build
# rm /etc/yum.repos.d/kbs-el5-ruby187.repo
```
**Compile**

Download node.js 0.10.33 source code from officail site (you can download whatever version you want).
```bash
# wget http://nodejs.org/dist/v0.10.33/node-v0.10.33.tar.gz
# tar zxvf node-v0.10.33.tar.gz
# cd node-v0.10.33
```
As in here, we have two versions python(2.4 and 2.6) installed. we need to do some trick job to make node.js compile scrip to use 2.6 version. 
```bash
# rm /usr/bin/python
# cp /usr/bin/python2.6 /usr/bin/python
```
Start compile job.
```bash
#./configure --prefix=/usr/
# make
# rm /tmp/nodejs -rf
# mkdir /tmp/nodejs
# make install DESTDIR=/tmp/nodejs/
```
Remember to recover python back to 2.4 first. then create an RPM package.
```bash
# rm /usr/bin/python
# cp /usr/bin/python2.4 /usr/bin/python
# yum install tree
# tree -L 3 /tmp/nodejs/
# fpm -s dir -t rpm -n nodejs -v 0.12.0 -C /tmp/nodejs/ usr/bin usr/lib
# ls -l *.rpm
-rw-r--r-- 1 root  root  5478842 Mar  4 15:10 nodejs-0.10.33-1.i686.rpm
```
Ok. After the last step. we will get the rpm file. You can install it on local server. Or you can copy this rpm file only to another server which run the same version of CentOS. then install it as the installation section shows on [top](#Installation).
