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

Extract downloaded Flutter file from `Downloads` directory to `Applications` directory where you currently navigated.

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

Verify Flutter installed, you might get a welcome message from Flutter when you execute the `flutter` command for the first time.

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

Navigate to `Downloads` directory where you created before.

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

Extract downloaded Android SDK Command Line Tools file from `Downloads` directory to `Applications` directory where you currently navigated. This command will put the files into `android` directory inside `Applications` directory.

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

You may use `sdkmanager --list` to find the latest version of each sdk tools. _(The latest tools are for android 33 as of writing)_

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

If you doing fresh install, you will need to configure Android Studio based on `flutter` command. Next step is to install Android Studio.

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

Extract Android Studio files from `Downloads` directory to `Applications` directory where you currently navigated

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

If you want to develop Flutter for web, you will need to configure Chrome based on `flutter` command. Next step is to configure Chrome for flutter.

## Chrome Configuration (Flutter for web)

Flutter Chrome configuration are able to use any chromium based browser. Setting the environment variable `CHROME_EXECUTEABLE` to the path of Chrome executable installed on Windows. Options below are required Browser to be **installed** on Windows.

### Chrome

If you use Google Chrome for developement.

```
echo -e "\n# Windows Chrome\nexport CHROME_EXECUTABLE='/mnt/c/Program Files/Google/Chrome/Application/chrome.exe'" >> $HOME/.bashrc
```

### Brave

If you use Brave for developement.

```
echo -e "\n# Windows Brave\nexport CHROME_EXECUTABLE='/mnt/c/Program Files/BraveSoftware/Brave-Browser/Application/brave.exe'" >> $HOME/.bashrc
```

Running flutter doctor to check the remaining dependencies for flutter

```
flutter doctor
```

If you want to develop Flutter for Linux Desktop, you will need to configure Linux toolchain based on `flutter` command. Next step is to configure Linux toolchain for flutter.

## Linux Desktop

When you run flutter doctor you will find that `ProcessException: Failed to find "pkg-config" in the search path`, which means that `pkg-config` tool is required. After that there will be 4 more tools required which are `clang` `cmake` `ninja-build` `libgtk-3-dev`.

```
flutter doctor
```

![pkg-config tool missing](https://i.imgur.com/ahaGJY0.png)

After installing pkg-config and run `flutter doctor` again

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