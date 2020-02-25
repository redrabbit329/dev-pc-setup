# dev-pc-setup
Redrabbit's Development Environment Configuration

Ubuntu 18.04 LTS : redrabbit-crossworks lenovor laptop computer
=================================================================

1. Install Ubuntu 18.04 LTS using booting usb
----------------------------------------------------

# BIOS
UEFI Disable (or Fast Boot Option Disable)
Legacy Boot First/ To Recognize USB Boot
Installation : Partition /, /boot(primary/ex4,512MB), /home(logical), /opt(logical), swap(primary, 2~8GB) 
            
            >> Occured : ACPI Error, AHCP  ---> Need to BIOS Update, When boot up linux, push 'Exc' key & try recovery option
          
2. Install Packages 
----------------------------------------------------
    Software Updater - automatically proceed with GUI.
    Build-Essential
            sudo apt-get update
            sudo apt-get install build-essential (요놈 시간이 좀 걸림)
    Drivers (gpu)
            sudo add-apt-repository ppa:graphics-drivers/ppa
            sudo apt update (if needed)
            sudo apt install nvidia-driver-430 (요놈 시간이 좀 걸림)
            sudo reboot
    Nvidia graphic card 설치 : https://gist.github.com/mari-linhares/cef4cb3440408e44963d1447a7db5ae0
    개인적 기호에 의한 Application
            sudo apt install terminator
            slack
            
    
3. FPGA Tools Installation
----------------------------------------------------
    Vitis 2019.2 (include Vivado 2019.2)
    
    Ultra96 개발환경 갖추기  - Vitis 2019.2 (with Vivado 2019.2) Install to make hardware platform design (*.xsa)
                            - PetaLinux 2019.2 (Install Essential Packages, then use final_ver_petalinux_installer)
                            - DNNDK 3.1 for Compiling DPU (Caffe or TensorFlow Library)
 
    Python개발 환경은?       - Anaconda 설치하면 되는지? 확인 후 업데이트
 
    Vitis-AI Installation   - git clone https://github.com/Xilinx/Vitis-AI  
                            - cd Vitis-AI (vitis-ai 소문자여도 상관 없드라)
                            - AI-Model-Zoo/script실행 : Model 다운로드 받아야 샘플 프로토 겟할수 있음 
                            - OS별 Docker Install :
                              (1) Reference Webpage : https://github.com/Xilinx/Vitis-AI
                              (2) Install Docker    : https://github.com/Xilinx/Vitis-AI/blob/master/doc/install_docker/README.md
                              (3) Ensure your linux user is in the group docker : https://docs.docker.com/install/linux/linux-postinstall/
                              (4) Load & Run Docker Container : https://github.com/Xilinx/Vitis-AI/blob/master/doc/install_docker/load_run_docker.md
                              (5) Docker Image Download : https://hub.docker.com/r/xilinx/vitis-ai/tags
                            
                            
