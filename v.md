Vagrant 시작하기

Vagrant (단어의 뜻은 부랑자, 거지라는 뜻)는 가상 머신을 편리하게 생성하고 관리하게 해주는 툴이다.
베이그런트를 이용하면 가상머신의 생성과, 간단한 네트워크 설정을 자동화 할 수 있다.

본 문서에서는 Fedora 리눅스 환경에서 하이퍼바이져로 VirtualBox를 설치한 상태에서 베이그런트를 이용하여 가상 머신을 관리하는 방법을 소개한다.

설치 방법
Vagrant 홈페이지를 방문한다. https://www.vagrantup.com/ 
Download 메뉴로 들어가면, Debian, Windows, Centos, Mac OS X 를 지원한다.
설치된 리눅스는 Fedora 24 64비트 버전이므로, 같은 redhat 계열인 centos 64-bit 를 받아 설치한다.
최신 버전인 2.0.1에 해당하는 vagrant_2.0.1_x86_64.rpm 파일을 받아서 rpm 을 설치하면 된다.
 rpm -ivh vagrant_2.0.1_x86_64.rpm
설치된 파일은 /opt/vagrant/ 아래에 위치하게 된다.

설치가 되었다면, vagrant 를 실행하면 간단한 도움말을 볼 수 있다.

# vagrant
Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
    -h, --help                       Print this help.

Common commands:
     box             manages boxes: installation, removal, etc.
     connect         connect to a remotely shared Vagrant environment
     destroy         stops and deletes all traces of the vagrant machine
     global-status   outputs status Vagrant environments for this user
     halt            stops the vagrant machine
     help            shows the help for a subcommand
     init            initializes a new Vagrant environment by creating a Vagrantfile
     login           log in to HashiCorp's Vagrant Cloud
     package         packages a running vagrant environment into a box
     plugin          manages plugins: install, uninstall, update, etc.
     port            displays information about guest port mappings
     powershell      connects to machine via powershell remoting
     provision       provisions the vagrant machine
     push            deploys code in this environment to a configured destination
     rdp             connects to machine via RDP
     reload          restarts vagrant machine, loads new Vagrantfile configuration
     resume          resume a suspended vagrant machine
     scp             copies data into a box via SCP
     share           share your Vagrant environment with anyone in the world
     snapshot        manages snapshots: saving, restoring, etc.
     ssh             connects to machine via SSH
     ssh-config      outputs OpenSSH valid configuration to connect to the machine
     status          outputs status of the vagrant machine
     suspend         suspends the machine
     up              starts and provisions the vagrant environment
     validate        validates the Vagrantfile
     version         prints current and latest Vagrant version

For help on any individual command run `vagrant COMMAND -h`

Additional subcommands are available, but are either more advanced
or not commonly used. To see all subcommands, run the command
`vagrant list-commands`.


Vagrant 맛보기

사이트에서 Get Started 문서를 보면 https://www.vagrantup.com/intro/getting-started/index.html
 단 2줄의 명령으로 Ubuntu 12.04 머신을 실행할 수 있음을 알수 있다.
아래와 같이 2줄의 명령을 입력하면 hashicorp 라는 사용자가 공개해둔 precise64 라는 머신을 다운로드 받아 실행하게 된다.

$ vagrant init hashicorp/precise64
$ vagrant up

“hashicorp/precise64” 을 box명이라고 하는데, Vagrant Cloud box catalog https://app.vagrantup.com/boxes/search
 사이트를 방문하면 다른 사람들이 올려놓은 box를 검색할 수 있다.
첫번째 줄을 실행하면 단지 현재 디렉터리에 Vagrantfile 이 생성된다. 두번째 명령을 실행하면 해당 box를 다운로드 받아서 실행한다.
불행히도, 위 명령을 실행하면 리눅스 설치 이미지를 인터넷으로 내려 받아야 하므로 실행하기까지 오랜 시간이 걸린다. 물론 한번 다운받은 box는 로컬에 저장되어 있기 때문에, 다음번 실행시에는 다운로드를 받지 않는다.
다운로드 받은 box는 /root/.vagrant.d/boxes 에 저장된다. (사용자 계정은 root로 가정)

# vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'hashicorp/precise64'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'hashicorp/precise64' is up to date...
==> default: Setting the name of the VM: precise64_default_1517217787086_24991
==> default: Fixed port collision for 22 => 2222. Now on port 2200.
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2200 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2200
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: 
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default: 
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default: 
    default: Guest Additions Version: 4.2.0
    default: VirtualBox Version: 5.1
==> default: Mounting shared folders...
    default: /vagrant => /home/root/vagrant_boxes/precise64

가상머신이 위와 같이 정상적으로 부팅이 되었다면, vagrant status 명령으로 상태를 조회할 수 있다.

# vagrant  status
Current machine states:

default                   running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.


vagrant ssh 라고 입력하면 가상 머신안으로 로그인이 가능하다.

# vagrant  ssh
Welcome to Ubuntu 12.04 LTS (GNU/Linux 3.2.0-23-generic x86_64)

 * Documentation:  https://help.ubuntu.com/
New release '14.04.5 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

Welcome to your Vagrant-built virtual machine.
Last login: Fri Sep 14 06:23:18 2012 from 10.0.2.2

vagrant@precise64:~$ uname -a
Linux precise64 3.2.0-23-generic #36-Ubuntu SMP Tue Apr 10 20:39:51 UTC 2012 x86_64 x86_64 x86_64 GNU/Linux

가상머신을 삭제하려면 destroy 명령을 쓰면된다.

# vagrant  destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Forcing shutdown of VM...
==> default: Destroying VM and associated drives...


Box 패키징 하기

VM을 생성하기 위해서는 box가 필요한데, 위의 경우에서 처럼 클라우드로 부터 box를 내려 받을 수도 있고, 직접 만들어 사용할 수도 있다. Box를 직접 만들기 위해서는 홈페이지에서 Docs 메뉴 -> Boxes 를 참고하면된다. https://www.vagrantup.com/docs/boxes/base.html

Vagrant 가 VM에 대한 기본적인 설정을 할 수 있도록 만들어 놓은 것을 Base Box(베이스 박스) 라고 부르는데, Base Box를 만들기 위해서는 다음과 같은 조건을 충족해야 한다.
 - 가상머신에 "vagrant" 사용자를 추가해야 함
 - "vagrant" 사용자의 홈디렉터리에 ssh 로긴을 위한 insecure keypair 를 등록해 두어야 한다. ( insecure keypair 에 대한 정보는 https://github.com/mitchellh/vagrant/tree/master/keys 에서 얻을 수 있다. )
 - root 패스워는 “vagrant"로 통일하는 것이 좋다.(public 으로 공개할 경우 더더욱)
 - "vagrant" 사용자가 패스워드 없이 sudo 작업을 할 수 있도록 하기 위해서  visudo 에서 다음과 같이 설정한다.
   vagrant ALL=(ALL) NOPASSWD: ALL

위와 같이 설정되어 있는 가상 머신이 존재한다고 할 때, 해당 VM을 box파일로 만드는 방법은 다음과 같다.

  vagrant package --base [VM이름] --output [파일명.box]

예를 들어, Virtual Box 에 centos6-base 라는 가상머신이 존재한다.

# vboxmanage  list vms | grep centos6
"centos6-base" {ad48ce95-71ed-4af1-b01e-90a122c1179a}

이 VM을 box 로 만들기 위해서는 다음 명령을 실행한다. 현재 폴더에 package.box 라는 파일이 생성된다. (--output 옵션을 주면 지정한 파일명으로 생성된다)

#   vagrant package --base centos6-base
==> centos6-base: Exporting VM...
==> centos6-base: Compressing package to: /home/root/vagrant_boxes/test/package.box
# file package.box 
package.box: gzip compressed data, last modified: Mon Jan 29 09:47:14 2018, from Unix

생성된 box를 등록해 주고 쓰려면 다음과 같이 한다.

# vagrant box add my-box package.box 
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'my-box' (v0) for provider: 
    box: Unpacking necessary files from: file:///home/root/vagrant_boxes/test/package.box
==> box: Successfully added box 'my-box' (v0) for 'virtualbox'!
등록된 박스 리스트를 조회하기

# vagrant box list
hashicorp/precise64 (virtualbox, 1.1.0)
my-box              (virtualbox, 0)

등록된 박스를 삭제하기

vagrant box remove [박스명]

# vagrant box remove my-box
Removing box 'my-box' (v0) with provider 'virtualbox'...

# vagrant box list
hashicorp/precise64 (virtualbox, 1.1.0)


같은 방법으로 CentOS 7 64 비트 버전으로 VM을 설치하고, box 로 등록하려면 다음과 같이 한다.

# vagrant package --base centos7-base --output centos7-base.box

# vagrant box add  centos7 centos7-base.box 
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'centos7' (v0) for provider: 
    box: Unpacking necessary files from: file:///home/root/vagrant_boxes/centos7-base.box
==> box: Successfully added box 'centos7' (v0) for 'virtualbox'!

# vagrant box list
centos7             (virtualbox, 0)
hashicorp/precise64 (virtualbox, 1.1.0)


VM 실행 하기

이제 등록된 box를 이용하여 VM을 실행하는 방법을 알아보자. 여기서는 위에서 등록한 centos7 을 이용하여 가상머신을 생성해 본다.
vagrant는 vagrantfile 이라는 파일을 생성하므로, 디렉터리를 따로 생성하여 작업하는 것이 좋다.
# mkdir centos7-test ; cd centos7-test 
# vagrant init centos7
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.

Vagrantfile 라는 파일이 생성되었고, 내용을 살펴보면, config.vm.box 부분만 제외 하고 다른 부분은 모두 주석으로 처리되어 있다.

# more Vagrantfile 
....
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos7"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false
....
end

일단 파일을 수정하기 않고, 실행해 보자.

[root@fedora1 centos7-test]# vagrant up 
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'centos7'...
Progress: 90%
.........

VM 이 실행되었으면 status 명령으로 상태를 확인한다.

# vagrant status
Current machine states:

default                   running (virtualbox)

ssh 명령으로 VM 안으로 로긴할 수 있다.

[root@fedora1 centos7-test]# vagrant ssh
Last login: Tue Jan 23 11:13:01 2018
[vagrant@localhost ~]$ 

생성된 VM 삭제는 vagrant destroy 를 입력하면 된다.


가상머신의 사설 IP와 호스트명을 설정하려면, 다음 예제와 같이 Vagrantfile을 수정한다.
Virtual Box에서 인식하는 VM 이름과 메모리 할당량은 customize 문에서 다음과 같이 설정해 준다.

# vi  Vagrantfile 
Vagrant.configure("2") do |config|

  config.vm.define "node1" do |node|
    node.vm.box = "centos7"
    node.vm.network "private_network", ip: "192.168.56.199"
    node.vm.hostname = 'myhost'
    node.vm.synced_folder ".", "/vagrant", disabled: true
    node.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--name", "myhost"]
      v.customize ["modifyvm", :id, "--memory", "4096"]
      v.customize ["modifyvm", :id, "--usb", "on"]
      v.customize ["modifyvm", :id, "--usbehci", "off"]
      v.customize ["modifyvm", :id, "--description", "some description"]
    end
  end

end

참고)
다음과 같이 공유폴더 설정에서 에러가 발생하는 경우
==> default: Mounting shared folders...
    default: /vagrant => /home/root/vagrant_boxes/centos7-test

다음과 같이 synced_folder 를 disabled 로 설정한다.
    config.vm.synced_folder ".", "/vagrant", disabled: true

참고)
다음과 같이 USB 컨트롤러 관련해서 에러가 발생하는 경우
Stderr: VBoxManage: error: Implementation of the USB 2.0 controller not found!
VBoxManage: error: Because the USB 2.0 controller state is part of the saved VM state, the VM cannot be started. To fix this problem, either install the 'Oracle VM VirtualBox Extension Pack' or disable USB 2.0 support in the VM settings.

다음과 같이 usbehci 설정을 off 로 한다. (virtualbox 만 해당)
    config.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--usb", "on"]
      v.customize ["modifyvm", :id, "--usbehci", "off"]
    end


# vagrant up
Bringing machine 'node1' up with 'virtualbox' provider...
==> node1: Importing base box 'centos7'...
==> node1: Matching MAC address for NAT networking...
==> node1: Setting the name of the VM: centos7-test_node1_1517220855947_84156
==> node1: Fixed port collision for 22 => 2222. Now on port 2200.
==> node1: Clearing any previously set network interfaces...
==> node1: Preparing network interfaces based on configuration...
    node1: Adapter 1: nat
    node1: Adapter 2: hostonly
==> node1: Forwarding ports...
    node1: 22 (guest) => 2200 (host) (adapter 1)
==> node1: Running 'pre-boot' VM customizations...
==> node1: Booting VM...
==> node1: Waiting for machine to boot. This may take a few minutes...
    node1: SSH address: 127.0.0.1:2200
    node1: SSH username: vagrant
    node1: SSH auth method: private key
    node1: Warning: Connection reset. Retrying...
    node1: Warning: Remote connection disconnect. Retrying...
    node1: Warning: Connection reset. Retrying...
    node1: 
    node1: Vagrant insecure key detected. Vagrant will automatically replace
    node1: this with a newly generated keypair for better security.
    node1: 
    node1: Inserting generated public key within guest...
    node1: Removing insecure key from the guest if it's present...
    node1: Key inserted! Disconnecting and reconnecting using new SSH key...
==> node1: Machine booted and ready!
==> node1: Checking for guest additions in VM...
    node1: No guest additions were detected on the base box for this VM! Guest
    node1: additions are required for forwarded ports, shared folders, host only
    node1: networking, and more. If SSH fails on this machine, please install
    node1: the guest additions and repackage the box to continue.
    node1: 
    node1: This is not an error message; everything may continue to work properly,
    node1: in which case you may ignore this message.
==> node1: Setting hostname...
==> node1: Configuring and enabling network interfaces...
    node1: SSH address: 127.0.0.1:2200
    node1: SSH username: vagrant
    node1: SSH auth method: private key

호스트명과 두번째 NIC의 IP 주소가 올바로 설정된 것을 확인한다.

[root@fedora1 centos7-test]# vagrant ssh

[vagrant@myhost ~]$ hostname
myhost
[vagrant@myhost ~]$ ifconfig enp0s8
enp0s8: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.56.199  netmask 255.255.255.0  broadcast 192.168.56.255
        inet6 fe80::a00:27ff:fe07:1b87  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:07:1b:87  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 27  bytes 3996 (3.9 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


IP주소로 직접 로그인 할 수 도 있다. 이 경우 패스워드를 입력해야 한다.

[root@fedora1 centos7-test]# ssh -u root 192.168.56.199 
The authenticity of host '192.168.56.199 (192.168.56.199)' can't be established.
ECDSA key fingerprint is SHA256:92rwh82ulGzX6xqnG4sK/gYkcR7Hqiy43mI5DXBz3+M.
ECDSA key fingerprint is MD5:13:2f:17:05:7b:45:5e:a3:cf:9a:d7:72:2e:0a:0c:d3.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.56.199' (ECDSA) to the list of known hosts.
root@192.168.56.199's password: 
[root@myhost ~]# 





한번에 여러개의 VM을 실행 하기

한번에 여러개의 VM을 동시에 실행하려면 다음과 같이 Vagrantfile을 수정한다.


Vagrant.configure("2") do |config|

  config.vm.define "node1" do |node|
    node.vm.box = "centos7"
    node.vm.network "private_network", ip: "192.168.56.198"
    node.vm.hostname = 'myhost1'
    node.vm.synced_folder ".", "/vagrant", disabled: true
    node.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--name", "myhost1"]
      v.customize ["modifyvm", :id, "--memory", "4096"]
      v.customize ["modifyvm", :id, "--usb", "on"]
      v.customize ["modifyvm", :id, "--usbehci", "off"]
      v.customize ["modifyvm", :id, "--description", "some description"]
    end
  end

  config.vm.define "node2" do |node|
    node.vm.box = "centos7"
    node.vm.network "private_network", ip: "192.168.56.199"
    node.vm.hostname = 'myhost2'
    node.vm.synced_folder ".", "/vagrant", disabled: true
    node.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--name", "myhost2"]
      v.customize ["modifyvm", :id, "--memory", "4096"]
      v.customize ["modifyvm", :id, "--usb", "on"]
      v.customize ["modifyvm", :id, "--usbehci", "off"]
      v.customize ["modifyvm", :id, "--description", "some description"]
    end
  end

end



# vagrant  up
Bringing machine 'node1' up with 'virtualbox' provider...
Bringing machine 'node2' up with 'virtualbox' provider...
==> node1: Importing base box 'centos7'...
==> node1: Matching MAC address for NAT networking...
==> node1: Setting the name of the VM: centos7-test_node1_1517221301352_4722
==> node1: Fixed port collision for 22 => 2222. Now on port 2200.
.........
    node1: SSH username: vagrant
    node1: SSH auth method: private key
==> node2: Importing base box 'centos7'...
==> node2: Matching MAC address for NAT networking...
==> node2: Setting the name of the VM: centos7-test_node2_1517221422627_46004
==> node2: Fixed port collision for 22 => 2222. Now on port 2201.
.........
    node2: SSH address: 127.0.0.1:2201
    node2: SSH username: vagrant
    node2: SSH auth method: private key

[root@fedora1 centos7-test]# vagrant  ssh node1
[vagrant@myhost1 ~]$ ifconfig enp0s8 | grep 192.168
        inet 192.168.56.198  netmask 255.255.255.0  broadcast 192.168.56.255
[vagrant@myhost1 ~]$ exit

[root@fedora1 centos7-test]# vagrant  ssh node2
[vagrant@myhost2 ~]$ ifconfig enp0s8 | grep 192.168
        inet 192.168.56.199  netmask 255.255.255.0  broadcast 192.168.56.255
[vagrant@myhost2 ~]$ exit


# vboxmanage  list runningvms
"myhost1" {6ba4bb6f-c68c-4566-befb-9430cc1be49b}
"myhost2" {7de28158-c730-4cf7-8f7c-f553d5927dd4}

[root@fedora1 centos7-test]# vagrant  halt node1
==> node1: Attempting graceful shutdown of VM...
[root@fedora1 centos7-test]# vagrant  status
Current machine states:

node1                     poweroff (virtualbox)
node2                     running (virtualbox)

[root@fedora1 centos7-test]# vagrant up node1
Bringing machine 'node1' up with 'virtualbox' provider...
==> node1: Clearing any previously set forwarded ports...
==> node1: Fixed port collision for 22 => 2222. Now on port 2200.
..........
