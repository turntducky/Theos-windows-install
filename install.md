(NOTE: WHEN YOU SEE THE THINGS IN "" THAT IS WHAT YOU PUT INTO YOUR TERMINAL. LEAVE OUT THE "" AND ONLY PUT WHAT IS IN THE MIDDLE INTO THE TERMINAL)

GUIDE:

1. First of all you will need the latest Windows update, so before proceeding, check for Windows updates and make sure your Windows 10 is up to date. This tutorial only works on Windows 10.

2. Open Windows PowerShell as an Admin. WINKEY + X -> Windows PowerShell (Admin)

3. Inside Windows PowerShell, run this command to enable WSL on your system. This is harmless and won't do anything abnormal to your PC.

Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
4. Reboot your PC after entering the command. Then open the Microsoft Store app and install the Ubuntu app on your Windows.

5. Once the download has completed, select "Launch" from the Microsoft Store page and wait for the terminal Window to do it's thing.

6. When asked for a UNIX username & password, enter anything you want. Example: Username: TurntDucky & Password: turntducky

7. Once that is done, you now need to open CMD & type in ubuntu or just search "Ubuntu" from your start menu.

8. If done correctly so far, you will see the username you set in a green color next to your PC name (as seen in the picture above). From that terminal Window, you will need to run a few commands to get theos up and running properly. Run these in order one at a time:

Installing dependencies:

"sudo apt-get update"
"sudo apt-get install -y build-essential git unzip libio-compress-perl"

Setting the $THEOS variable:

"echo export THEOS="/opt/theos" >> ~/.bashrc"
"echo export PATH="\$THEOS/bin:\$PATH" >> ~/.bashrc"
"echo alias theos="\$THEOS/bin/nic.pl" >> ~/.bashrc"
"echo "umask 0022" >> ~/.bashrc"
"source ~/.bashrc"

Installing Theos, Toolchain & SDK:
I'm installing theos in /opt because I like it there but you can change directories to anywhere you like.

"sudo git clone --recursive git://github.com/theos/theos.git $THEOS"
"cd $THEOS/toolchain && sudo wget https://developer.angelxwind.net/Linux/ios-toolchain_clang%2bllvm%2bld64_latest_linux_x86_64.zip -O LinuxToolchain.zip"
"sudo unzip LinuxToolchain.zip && sudo rm -f LinuxToolchain.zip"
"sudo rm -rf $THEOS/sdks/ && sudo git clone https://github.com/theos/sdks $THEOS/sdks"
 

Installing newer libstdc++:

"cd /tmp && wget http://mirrors.kernel.org/ubuntu/pool/main/g/gcc-5/libstdc++6_5.4.0-6ubuntu1~16.04.9_amd64.deb -O libstdc++.deb"
"dpkg-deb -x libstdc++.deb libstdc++"
"cp libstdc++/usr/lib/x86_64-linux-gnu/libstdc++.so.6.0.21 /usr/lib/x86_64-linux-gnu/"
"cd /usr/lib/x86_64-linux-gnu/"
"ln -sf libstdc++.so.6.0.21 libstdc++.so.6"

Fixing fakeroot problem & possible future permission errors:

"sudo sed -i 's/\$(FAKEROOT) -r/fakeroot-tcp/g'  $THEOS/makefiles/package/deb.mk"

"sudo chown -R $(id -u):$(id -g) $THEOS"
 

And you're done! Theos is officially installed and ready to run. CD into the folder where you want your projects to be stored and run:

"$THEOS/bin/nic.pl"

or

"theos"
 

Other notes:
- You can pin the Ubuntu terminal on your taskbar or create a desktop shortcut for easier access.
- Your projects and whole Ubuntu file system is located in: C:\Users\%USERNAME%\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_xxxxxx\LocalState\rootfs. Your projects are usually stored inside the /home folder.
- You can set root as the default user by running this command in CMD: ubuntu config --default-user root - It's better to login and always use root because of how drag and dropping via Explorer works. If you are going to use root always, you need to run the "setting $THEOS variable" part of the tutorial again as root user. It is actually highly suggested you do this because things might not work properly if you're not root.
- To use make package install you need iFunBox's USB Tunnel running & your iDevice connected via USB. Then run echo export "THEOS_DEVICE_IP=localhost THEOS_DEVICE_PORT=23" >> ~/.bashrc in your terminal window.
- You can add extra helpful aliases by running commands like: echo 'alias mpi="make package install"' >>~/.profile && source ~/.profile so when you want to make and install your package, you can simply run 'mpi' command.
