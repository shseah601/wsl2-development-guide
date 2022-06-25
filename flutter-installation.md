# Installation Guide For Flutter In WSL2

This page will have easy copy & paste commands with images if available showing.
This guide wil be using [Windows Subsystem for Linux][1] version 2 (WSL2) with Ubuntu 22 and Windows 11. I suggest follow [Installing Flutter 2.0 on WSL2][2] by Josh Kautz until the sdkmanager installation and continue [Installing Android Studio on WSL2 for Flutter][3] by addshore for the Android Studio installation in WSL2.

## Flutter Installation
Create a `Downloads` directory if it doesn’t already exist, and navigate to it.

```
mkdir -p $HOME/Downloads && cd "$_"
```

Download latest [Flutter SDK release][5]. _Use right click and copy link address of the latest Flutter version._

![Copy Latest Flutter Download Link](https://i.imgur.com/9Sc68iq.png)

```
wget https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.0.2-stable.tar.xz
```

Create an `Applications` directory if it doesn’t already exist, and navigate to it.

```
mkdir -p $HOME/Applications && cd "$_"
```

Extract downloaded Flutter file from `Downloads` directory to `Applications` directory where we currently navigated.

```
tar xfv $HOME/Downloads/$(ls -d $HOME/Downloads/flutter*.tar.xz | xargs basename)
```

Add Flutter path to Bash shell PATH in `$HOME/.bashrc`

```
echo -e "\n# Flutter\nexport PATH=\$HOME/Applications/flutter/bin:\$PATH" >> $HOME/.bashrc
```

Execute `source` to update PATH variable to current working session

```
source $HOME/.bashrc
```

Verify Flutter installed, we might get a welcome message from Flutter when we execute the `flutter` command for the first time.

```
flutter --version
```

![Displaying installed flutter version](https://i.imgur.com/kiE5mw5.png)

Check for Flutter setup dependencies

```
flutter doctor
```

## Android SDK Installation

Install the [unzip][6] utility and the [default-jdk][7] development utility

```
sudo apt install unzip default-jdk -y
```

Navigate to `Downloads` directory where we created before.

```
cd $HOME/Downloads
```

Download latest Android SDK [Command line tools][8] for Linux.

![Select Linux command line tools zip](https://i.imgur.com/ybpgc0z.png)

Right click on the Download Button and click copy link address

![Copy Android Command Line Tools For Linux Link Address](https://i.imgur.com/YuRjzbN.png)

Download the file

```
wget https://dl.google.com/android/repository/commandlinetools-linux-8512546_latest.zip
```

Navigate to `Applications` directory

```
cd $HOME/Applications
```

Extract downloaded Android SDK Command Line Tools file from `Downloads` directory to `Applications` directory where we currently navigated. This command will put the files into `android` directory inside `Applications` directory.

```
unzip $HOME/Downloads/$(ls -d $HOME/Downloads/commandlinetools-linux*.zip | xargs basename) -d ./android
```

Get installed `sdkmanager` version, and store to `$version` bash variable.

```
version=$($HOME/Applications/android/cmdline-tools/bin/sdkmanager --sdk_root=$HOME/Applications/android --version | head -n1);
echo $version;
```

Create new version directory for command line tools

```
mkdir -p $HOME/Applications/android/cmdline-tools/$version
```

Move command line tools to new version directory.
```
mv -vt $HOME/Applications/android/cmdline-tools/$version/ $HOME/Applications/android/cmdline-tools/!($version)
```

Add cmdline-tools path to Bash shell PATH variable in `$HOME/.bashrc`.

```
echo -e "\n# Android\nexport PATH=\$HOME/Applications/android/cmdline-tools/$version/bin:\$PATH" >> $HOME/.bashrc;
```

Execute `source` to update PATH variable to current working session

```
source $HOME/.bashrc
```

We may use `sdkmanager --list` to find the latest version of each sdk tools. _(The latest tools are for android 33 as of writing)_

```
sdkmanager --list | grep 'android-3\|build-tools;3'
```

Then install the tools with `sdkmanager`.

```
sdkmanager --install "system-images;android-33;google_apis;x86_64" "platform-tools" "platforms;android-33" "build-tools;33.0.0" "cmdline-tools;latest"
```

List all successfully installed tools from sdkmanager

```
sdkmanager --list_installed
```

![List of installed tools from sdkmanager](https://i.imgur.com/4Nrt9wH.png)

Add installed platform-tools and emulator path to Bash shell PATH variable in `$HOME/.bashrc`

```
echo -e "export PATH=\$HOME/Applications/android/emulator:\$PATH" >> $HOME/.bashrc;
echo -e "export PATH=\$HOME/Applications/android/platform-tools:\$PATH" >> $HOME/.bashrc;
```

Execute `source` to update PATH variable to current working session

```
source $HOME/.bashrc
```

Configure Flutter to use Android SDK

```
flutter config --android-sdk $HOME/Applications/android/
```

Accept Android SDK licenses
```
sdkmanager --licenses
```

Running flutter doctor to check the remaining dependencies for flutter

```
flutter doctor
```

If we are doing fresh install, we will need to configure Android Studio based on `flutter` command. Next step is to install Android Studio.

## Android Studio Installation

If Android Studio is installed please skip until flutter configuration of Android Studio to continue.

Navigate to `Downloads` directory.

```
cd $HOME/Downloads
```

Download [Android Studio for Linux][9]. Click Download options button or scroll down to find Android Studio downloads list for different platforms.
Select the download link for Linux.

![List of Android Studio download link for different platforms](https://i.imgur.com/wo433BY.png)

Right click on the Download Button and click copy link address

![Copy download link for Android Studio Linux](https://i.imgur.com/WEEKipq.png)

```
wget https://redirector.gvt1.com/edgedl/android/studio/ide-zips/2021.2.1.15/android-studio-2021.2.1.15-linux.tar.gz
```

Navigate to `Applications` directory

```
cd $HOME/Applications
```

Extract Android Studio files from `Downloads` directory to `Applications` directory where we currently navigated

```
tar xfv $(ls -1t $HOME/Downloads/android-studio-* | head -n1)
```

Configure Flutter to use Android Studio

```
flutter config --android-studio-dir $HOME/Applications/android-studio/
```

Running flutter doctor to check the remaining dependencies for flutter

```
flutter doctor
```

If we want to develop Flutter for web, we will need to configure Chrome based on `flutter` command. Next step is to configure Chrome for flutter.

## Chrome Configuration (Flutter for web)

Flutter Chrome configuration are able to use any chromium based browser. Setting the environment variable `CHROME_EXECUTEABLE` to the path of Chrome executable installed on Windows. Options below are required Browser to be **installed** on Windows.

### Chrome

If we use Google Chrome for developement.

```
echo -e "\n# Windows Chrome\nexport CHROME_EXECUTABLE='/mnt/c/Program Files/Google/Chrome/Application/chrome.exe'" >> $HOME/.bashrc
```

### Brave

If we use Brave for developement.

```
echo -e "\n# Windows Brave\nexport CHROME_EXECUTABLE='/mnt/c/Program Files/BraveSoftware/Brave-Browser/Application/brave.exe'" >> $HOME/.bashrc
```

Running flutter doctor to check the remaining dependencies for flutter

```
flutter doctor
```

If we want to develop Flutter for Linux Desktop, we will need to configure Linux toolchain based on `flutter` command. Next step is to configure Linux toolchain for flutter.

## Linux Desktop

When we run flutter doctor we will find that `ProcessException: Failed to find "pkg-config" in the search path`, which means that `pkg-config` tool is required. After that there will be 4 more tools required which are `clang` `cmake` `ninja-build` `libgtk-3-dev`.

```
flutter doctor
```

![pkg-config tool missing](https://i.imgur.com/ahaGJY0.png)

After installing pkg-config and run `flutter doctor` again, we will see that it is complaining clang cmake ninja-build libgtk-3-dev are required.

![clang cmake ninja-build libgtk-3-dev tools missing](https://i.imgur.com/iDH2Xpo.png)

So we will need to install 5 tools for the Linux Desktop development

```
sudo apt install pkg-config clang cmake ninja-build libgtk-3-dev -y
```

Verify Linux toolchain has no issue

```
flutter doctor
```

![Linux toolchain no issue](https://i.imgur.com/VOpwAwh.png)

Voila! We can continue to build flutter application from now on.

## Android Studio AVD **/dev/kvm device: permission denied** Warning

After launching android studio in WSL2, walks through the Android Studio setup if first time launching. Accepts all the tools license. Installs the tools.

```
sh $HOME/Applications/android-studio/bin/studio.sh
```

After setup, we might see a warning message showing **/dev/kvm device: permission denied**.

![AVD configuration showing /dev/kvm device: permission denied](https://i.imgur.com/NaxRdgs.png)

So we will need to checks our `/dev/kvm` user permission.

```
ls -la /dev/kvm
```

![/dev/kvm permissions, root group without group read write permission](https://i.imgur.com/zXKVRWw.png)

From the permission we able to know that, `/dev/kvm` is currently accessible by root user only, we will have different options for us to access it.

Add current user to **kvm** group, change `/dev/kvm` group to **kvm** and grant read and write access for **kvm** group.

```
sudo usermod -a -G kvm $USER;
sudo chgrp kvm /dev/kvm;
sudo chmod g+rw /dev/kvm;
```

After alter the permissions and we checks the permission for `/dev/kvm`.

```
ls -la /dev/kvm
```

![new /dev/kvm permissions, kvm group with read write permission](https://i.imgur.com/Hk3BTDb.png)

Relaunch Android Studio and tries to create a new AVD we will see the warning message is gone.

![AVD configuration without /dev/kvm device: permission denied warning](https://i.imgur.com/xqbWqGh.png)

### `/dev/kvm` Permission Gone When WSL Reboot

We will find what when rebooting WSL we may not start AVD due to `/dev/kvm` permission reset back to root access. We will need to use `crontab` to let system execute `chgrp` and `chmod` for us whenever WSL reboot. Since the commands are requried `sudo` we will edit cron job in sudo.

```
sudo crontab -e
```

When we are the first time using `crontab` we will be prompted to choose a editor. Press 1 which is `nano` as it is quite easy to use for terminal and press enter to proceed.

![crontab prompt to choose editor](https://i.imgur.com/mwy5Ies.png)

Add lines below
```
# Execute on booted
@reboot /bin/chgrp kvm /dev/kvm
@reboot /bin/chmod g+rw /dev/kvm
```

![Add on boot commands, chmod and chgrp of /dev/kvm into crontab](https://i.imgur.com/uVIRdq2.png)

Then press `Ctrl + O` and press enter to save changes. Press `Ctrl - X` to exit `nano` editor.

We have our cron job ready but it will not execute anything, so we need to start the `cron` daemon.

```
sudo service cron start
```

![Start cron server](https://i.imgur.com/JabUvss.png)

In WSL, the `cron` daemon will not start automatically, so we need to modify our WSL configuration to let it start `cron` daemon when it is booted. According to [Advanced settings configuration in WSL][11], we need to create a wsl.config inside our WSL.

```
sudo nano /etc/wsl.config
```

Then add the following lines into the file. This will tells WSL to start `cron` when it is booted. **All commands are executed as root**. We are able to execute multiple commands by adding semicolon (;) for each of the commands.

```
# Set a command to run when a new WSL instance launches. This example starts the Docker container service.
[boot]
command="service cron start"
#command="command 1; command 2"
```

Then press `Ctrl + O` and press enter to save changes. Press `Ctrl - X` to exit `nano` editor.

When we restart our WSL we will find that `/dev/kvm` will be the permission we set as before.

![/dev/kvm is correct permission on WSL booted](https://i.imgur.com/0Jar6Nb.png)

If `/dev/kvm` does not have the same permission as shown, you need to check if `cron` daemon service is runnning.

```
service cron status
```

![Cron daemon is running](https://i.imgur.com/zPKNiDP.png)

If `cron` is not running, please review the steps above and try again.

## References
1. [Installing Flutter 2.0 on WSL2][2] by Josh Kautz
2. [Installing Android Studio on WSL2 for Flutter][3] by addshore
3. [Flutter Chrome category fix][4] by Michel Feinstein
4. [Linux Desktop Development tools missing][10] by Hemant Jaiman

[1]: https://docs.microsoft.com/en-us/windows/wsl/install
[2]: https://medium.joshkautz.com/installing-flutter-2-0-on-wsl2-2fbf0a354c78
[3]: https://addshore.com/2022/01/installing-android-studio-on-wsl2-for-flutter/
[4]: https://stackoverflow.com/questions/62293857/how-to-develop-flutter-web-app-on-windows-subsystems-for-linux-debian-10
[5]: https://docs.flutter.dev/development/tools/sdk/releases?tab=linux
[6]: https://packages.ubuntu.com/focal/unzip
[7]: https://packages.ubuntu.com/focal/default-jdk
[8]: https://developer.android.com/studio#command-tools
[9]: https://developer.android.com/studio#downloads
[10]: https://stackoverflow.com/questions/67169985/exception-unable-to-generate-build-files-in-flutter
[11]: https://docs.microsoft.com/en-us/windows/wsl/wsl-config#example-wslconf-file