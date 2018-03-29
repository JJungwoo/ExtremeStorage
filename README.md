## Extreme Cache
Log-structured Caching for Linux

## Update Function
- **Adaptive write function**: 

## Experiment Setting
- **실험 환경**
 * CentOS Version: CentOS Linux release 7.4.1708
 * Kernel Version: 3.10.0-693.17.1.el7.x86_64
 * CPU: Intel(R) core™ i7-6700 CPU@3.40GHz
 * Storage:
	- Backing device: 1TB HDD
	- Cache device: 128GB SSD (Samsung 850 pro)
 * Memory: 8GB
 * BenchMark: FIO(3.3-18.v), DD
 **테스트 방법**
 **Create Extreme Cache**
 * sudo git clone https://github.com/JJungwoo/extremestorage.git
 아래의 방법으로 rpmforge를 설치할 수 없을 경우 코드를 직접 받아서 설치해야함.
 * sudo rpm -ivh http://repository.it4i.cz/mirrors/repoforge/redhat/el6/en/x86_64/rpmforge/RPMS/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm 
 * sudo yum check-update 
 * sudo yum install -y dkms
 * sudo make install (extreme-cache 가상 상위 디렉터리에서 실행.)
 * sudo modprobe dm-writeboost
 * sudo dd if=/dev/zero of=/dev/myssd count=1
  - zero out the first 1 sector to trigger formatting
 * sudo dmsetup create mylv --table “0 `blockdev --getsize /dev/myraid` writeboost /dev/myraid /dev/myssd”
  - create /dev/mapper/mylv
 * Play with /dev/mapper/mylv
 dd, fio, mkfs, ...
 * sudo mkfs.ext4 /dev/mapper/mylv
 * sudo mount /dev/mapper/mylv /target_directory
 **Delete Extreme Cache**
 * sudo dmsetup remove mylv
  - remove /dev/mapper/mylv
 * sudo modprobe -r dm-writeboost
 * sudo make uninstall (extreme-cache 가장 상위 디렉터리에서 실행.)

## Related Projects
* https://github.com/akiradeveloper/dm-writeboost-tools: Tools to help users analyze the state of the cache device  
* https://gitlab.com/onlyjob/writeboost: A management tool including init script  
* https://github.com/akiradeveloper/writeboost-test-suite: Testing framework written in Scala
* https://github.com/kazuhisya/dm-writeboost-rpm: Providing RPM packages

## Related works
* Y. Hu and Q. Yang -- DCD Disk Caching Disk: A New Approach for Boosting I/O Performance (1995)
  (http://www.ele.uri.edu/research/hpcl/DCD/DCD.html)  
* G. Soundararajan et. al. -- Extending SSD Lifetimes with Disk-Based Write Caches (2010)
  (https://www.usenix.org/conference/fast-10/extending-ssd-lifetimes-disk-based-write-caches)  
* Y. Oh -- SSD RAID as Cache (SRC) with Log-structured Approach for Performance and Reliability (2014)
  (https://ysoh.files.wordpress.com/2009/05/dm-src-ibm.pdf)
