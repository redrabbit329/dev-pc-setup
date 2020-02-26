# dev-pc-setup
Redrabbit's Development Environment Configuration

Ubuntu 18.04 LTS : redrabbit-crossworks lenovor laptop computer
=================================================================


1. Install Ubuntu 18.04 LTS using booting usb
----------------------------------------------------

### BIOS
##### UEFI Disable (or Fast Boot Option Disable)
            Legacy Boot First/ To Recognize USB Boot
            Installation : Partition /, /boot(primary/ex4,512MB), /home(logical), /opt(logical), swap(primary, 2~8GB) 
            >> Occured : ACPI Error, AHCP  ---> Need to BIOS Update, When boot up linux, push 'Exc' key & try recovery option


2. Install Packages 
----------------------------------------------------
    ##### Software Updater - automatically proceed with GUI.
    ##### Build-Essential
            sudo apt-get update
            sudo apt-get install build-essential (요놈 시간이 좀 걸림)
    ##### Drivers (gpu)
            sudo add-apt-repository ppa:graphics-drivers/ppa
            sudo apt update (if needed)
            sudo apt install nvidia-driver-430 (요놈 시간이 좀 걸림)
            sudo reboot
    ##### Nvidia graphic card 설치 : https://gist.github.com/mari-linhares/cef4cb3440408e44963d1447a7db5ae0
    ##### 개인적 기호에 의한 Application
            sudo apt install terminator
            slack
            
    
3. FPGA Tools Installation
----------------------------------------------------
    ##### Vitis 2019.2 (include Vivado 2019.2) 설치 : /opt 아래 설치하려면 sudo ./xsetup
    
    ##### Ultra96 개발환경 갖추기  
                            - Vitis 2019.2 (with Vivado 2019.2) Install to make hardware platform design (*.xsa)
                            - PetaLinux 2019.2 (Install Essential Packages, then use final_ver_petalinux_installer)
                            - DNNDK 3.1 for Compiling DPU (Caffe or TensorFlow Library)
 
    ##### Python개발 환경은?       - Anaconda 설치하면 되는지? 확인 후 업데이트
 
    ##### Vitis-AI Installation   
                            - git clone https://github.com/Xilinx/Vitis-AI  
                            - cd Vitis-AI (vitis-ai 소문자여도 상관 없드라)
                            - AI-Model-Zoo/script실행 : Model 다운로드 받아야 샘플 프로토 겟할수 있음 
                            - OS별 Docker Install :
                              (1) Reference Webpage : https://github.com/Xilinx/Vitis-AI
                              (2) Install Docker    : https://github.com/Xilinx/Vitis-AI/blob/master/doc/install_docker/README.md
                              (3) Ensure your linux user is in the group docker : https://docs.docker.com/install/linux/linux-postinstall/
                              (4) Load & Run Docker Container : https://github.com/Xilinx/Vitis-AI/blob/master/doc/install_docker/load_run_docker.md
                              (5) Docker Image Download : https://hub.docker.com/r/xilinx/vitis-ai/tags
                            
                            
4. PetaLinux Tools Installation
----------------------------------------------------
# PetaLinux Tools : v2019.2.final installer download 
 ## 1. Prerequisites : 
  ### Ubuntu bash shell 확인 :
            Ubuntu의 경우 기본 쉘이 dash이므로, PetaLinux및 Xilinx command환경을 위해 bash shell로 변경해준다.
            >> $ sudo dpkg-reconfigure dash
            >> $ ls -al /bin/sh
  ##### PetaLinux가 구동되기 위한 기본 Package를 설치한다. (OS Dependency) - UG1144 Ch2. Table2참고
            For Ubunt Linux 16.04.5, 16.04.6, 18.04.1, 18.04.2 (64bit) 기준, 테이블에 나온 패키지들 설치
            
            >> $ sudo apt-get install gawk python build-essential gcc git make net-tools libncurses5-dev tftpd zlib1g-dev libssl-dev flex bison libselinux1 gnupg wget diffstat chrpath socat xterm autoconf libtool tar unzip texinfo zlib1g-dev gcc-multilib zlib1g:i386 screen pax gzip python 2.7.5
            
            생각보다 한참 걸린다. 그리고 노트북이 몹시 힘들어 함
            
            
  ##### PetaLinux 2019.2 Installation
            PetaLinux는 YoctoProject Base이므로, User권한으로 설치 및 사용해야 한다.
            즉, 설치 파일(installer)도 /home/user 디렉토리 이하에 존재해야 하고
            설치될 root경로의 디렉토리도 권한을 다음과 같이 풀어 준다.
            
            >> $ sudo chgrp -R username install_directory
            >> $ sudo chown -R username install_directory
            >> $ sudo chmod -R 775 install_directory
            
            설치 폴더는 만들어줘야 함 ('install_directory' = /opt/pkg/petalinux/2019.2 일 경우)
            >> $ ./petalinux-v2019.2-final-installer.run /opt/pkg/petalinux/2019.2   (sudo쓰면 아니됨)
            
            설치 중 라이센스를 세 번 묻는다. 'q'눌러 빠져나오고 'y' 눌러 다음으로 넘어가자.
            한참 걸림. 나의 노트북이여...
            
            설치가 종료되면, 
            
            >> $ source path_to_installed_PetaLinux/setting.sh
            
            를 터미널에 입력하여, 환경 변수를 적용하고
            
            >> $ echo $PETALINUX
            
            확인해 본다. 
            설치 경로를 응답하면, 올바르게 설치되었음.
            
            윈도우랑 다르게, petalinux명령을 적용하기 위해서 매번 고민하기 싫으면 bashrc에 추가해 줘야 하고,
            까먹지 않을 자신이 있다면 이 경로의 setting.sh 스크립트를 매번 소싱해줘야 함.
            
            WARNING: No tftp server found - please refer to "PetaLinux SDK Installation Guide" for its impact and solution.
            -Problem Description : TFTP service가 없으면 u-boot의 network/TFTP capabilities를 사용한 리눅스 이미지 다운이 불가하다고 함
            
            TFTP를 사용하기 위한 설치 들어감
            
            >> $ sudo apt-get install tftp tftpd xinetd
            
            설치 후, TFTP를 사용하기 위해서 /etc/xinetd.d/tftp 파일을 만들고, 아래 내용을 붙여넣기 후 저장
            
            >> $ sudo vim /etc/xinetd.d/tftp
            
            >> service tftp         {
                        socket_type     = dgram
                        protocol        = udp
                        wait            = yes
                        user            = root
                        server          = /usr/sbin/in.tftpd
                        server_args     = -s /tftpboot
                        disable         = no
                        per_source      = 11
                        cps             = 100 2
                        flags           = IPv4
             }

            tftp용 디렉토리 생성
            
            >> $ sudo mkdir /tftboot
            >> $ sudo chmod 777 /tftboot
            
            그리고 나서 tftp 서버를 시작해 준 다음. 설정파일을 등록
            
            >> $ sudo /etc/init.d/xinetd restart
            
            어떤 문서에서는 source petalinux설치경로/setting.sh 를 실행하여 설정파일을 등록하면,
            tftp 오류문구가 안나온다고 이야기하는데, 실제로는... bash shell에서 실행하면 계속 이 문구가 뜸
            dash shell로 바꿔놓고 tftp를 작업해야 하는 건가? 오류가 나면 다시 포스팅하는 걸로.
            
            
            ###### TFTP (Trivial File Transfer Protocol) : 
                        FTP보다 더 단순한 방식으로 파일 전송하는 프로토콜. 구현간단. 데이터 손실위험.
                        임베디드 시스템이나 운영체제 업로드에 주로 사용한다                             
  
  ##### PetaLinux BSP Download
            PetaLinux Tool은 config명령에서 사용할 bsp를 포함하고 있지 않으므로, 
            사용할 Device에 맞는 Platform/ BSP/ 를 내려받아야 하며,
            Customizing할 경우에는 Vivado에서 작업한 결과물을 가져와서 사용하면 된다.
            2019.2버전부터 vivado가 *.xsa를 토해 냄. (그 전에는 bdf였다)
       
       
5. Vitis-AI 개발환경을 위해 준비할 것
----------------------------------------------------
#### 5.0. Common
            1. vitis-ai repository를 clone 한다.
            
                        >> $ git clone https://github.com/xilinx/vitis-ai
                        
            2. Docker를 설치하고 group docker user에 linux user (=나)를 추가한다.
               Ubuntu용 Docker Install Guide : https://docs.docker.com/install/linux/docker-ce/ubuntu/
               
                     
            3. Docker image를 내려받는다.
            4. XRT 2019.2를 install한다.
            5. Target Device의 platform을 내려받거나 생성(customizing)한다.
#### 5.1. For MPSoC
#### 5.2. For Alveo
