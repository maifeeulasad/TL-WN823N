If you have internet access by any other means (wired or through USB tethering), you can installed the RTL8192EU drivers for your TP-LINK TL-WN823N wireless adapter from [Mange's GitHub repo][1]. Here are the steps as described on the GitHub page:

**Building and installing using DKMS**

(1) Install DKMS and other required tools:

    sudo apt-get install git linux-headers-generic build-essential dkms

(2) Clone this repository and change your directory to cloned path.

    git clone https://github.com/Mange/rtl8192eu-linux-driver
    cd rtl8192eu-linux-driver

(3) Add the driver to DKMS. This will copy the source to a system directory so that it can used to rebuild the module on kernel upgrades.

    sudo dkms add .

(4) Build and install the driver.

    sudo dkms install rtl8192eu/1.0

(5) Distributions based on Debian & Ubuntu have RTL8XXXU driver present & running in kernelspace. To use our RTL8192EU driver, we need to blacklist RTL8XXXU.

    echo "blacklist rtl8xxxu" | sudo tee /etc/modprobe.d/rtl8xxxu.conf

(6) Force RTL8192EU Driver to be active from boot.

    echo -e "8192eu\n\nloop" | sudo tee /etc/modules

(7) Newer versions of Ubuntu has weird plugging/replugging issue (Check #94). This includes weird idling issues, To fix this:

    echo "options 8192eu rtw_power_mgnt=0 rtw_enusbss=0" | sudo tee /etc/modprobe.d/8192eu.conf

(8) Update changes to Grub & initramfs

    sudo update-grub; sudo update-initramfs -u

(9) Reboot system to load new changes from newly generated initramfs.

    systemctl reboot -i

(10) After the reboot, you can check that your kernel has loaded the right module:

    sudo lshw -c network

You should see the line `driver=8192eu`


  [1]: https://github.com/Mange/rtl8192eu-linux-driver
