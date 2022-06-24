# Customize Windows Terminal and WSL Theme

## Oh My Posh Installation

### Windows Terminal
Download Oh My Posh [Windows Installation](https://ohmyposh.dev/docs/installation/windows) from Microsoft store or using `winget` command in Windows Powershell.

```
winget install JanDeDobbeleer.OhMyPosh -s winget
```

After installed **Oh My Posh**, we can use code editor of any choice to edit Powershell profile from `$PROFILE`.

notepad
```
notepad $PROFILE
```

VS Code
```
code $PROFILE
```

After that, add the line inside the file and save.

```
oh-my-posh init pwsh | Invoke-Expression
```

For theme selection, go to Oh My Posh [Theme Page](https://ohmyposh.dev/docs/themes) and find the theme name. For example **night-owl** theme, replace *paradox* to **night-owl**. we just need to find the theme we like and copy the theme name and replace and save it.

```
oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\paradox.omp.json" | Invoke-Expression
```

After that when we restart the Powershell, we will see the new theme.

### WSL

#### Homebrew installation
To install Oh My Posh for Linux we will need to use [Homebrew](https://brew.sh/). Go to Homebrew page and install via bash script command. *The script may change from time to time, visit [Homebrew](https://brew.sh/) for latest install script.*

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

After installed, remember to add Homebrew to PATH by following the guide, which looks like the command.

```
echo "eval \"\$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)\"" >> ~/.profile
```

#### Oh My Posh Linux Installation

We will follow Oh My Posh [Linux Installation](https://ohmyposh.dev/docs/installation/linux).

```
brew install jandedobbeleer/oh-my-posh/oh-my-posh
```

This installs two things:

- oh-my-posh - Executable, added to $(brew --prefix)/bin
- themes - The latest Oh My Posh [themes](https://ohmyposh.dev/docs/themes)

The installed themes are located at `$(brew --prefix oh-my-posh)/themes`. We able to check themes in the `theme` directory.

```
ls $(brew --prefix oh-my-posh)/themes
```

#### Bash Shell Theme

To setup default terminal to bash for user, execute `chsh`

```
chsh -s $(which bash)
```

Execute the command below to add `oh-my-posh` init everytime `bash` started. Rememeber to replace *bubblesextra* to the theme we want.

```
echo -e "\n# Oh My Posh Shell Theme\nexport OH_MY_POSH_DIR=\"\$(brew --prefix oh-my-posh)\"/themes" >> ~/.bashrc;
echo "eval \"\$(oh-my-posh init bash --config \$OH_MY_POSH_DIR/bubblesextra.omp.json)\"" >> ~/.bashrc;
```

oh-my-posh init script should looks like screenshot below in `.bashrc`.

![oh-my-posh init script added in .bashrc](https://i.imgur.com/0GJS8sW.png)

#### Fish Shell Theme

To setup default to Fish for current user, execute `chsh`

```
chsh -s $(which fish)
```

If you using Fish shell, you will need to edit `~/.config/fish/config.fish`. Rememeber to replace *night-owl* to the theme we want.

```
code ~/.config/fish/config.fish
```

And add fish script as screenshot below.

![oh-my-posh init script added in config.fish](https://i.imgur.com/EQcPn8B.png)

#### Different theme for different shells

I recommend to edit `~/.profile`. Move the line `eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"` to above bash version as screenshot to allow different shell themes. Move above including `.bashrc`, so that `.bashrc` able to recognize Homebrew `oh-my-posh` command.

![Move linuxbrew shellenv setup to top](https://i.imgur.com/LaLfz0K.png)

Switching different shell showing different Oh My Posh themes

![Different shell showing different Oh My Posh theme](https://i.imgur.com/z1XH9Nt.png)

#### Not able to switch shell

There is a chance that you encounter `You may not change the shell for 'xxx'.` when trying to switch default shell. You just do sudo chsh for that user.

```
sudo chsh -s "$(which bash)" username
```

## Nerd Fonts

You will find a lot theme does not looks like the screenshot from the theme example page, it is due to the font face we using in Windows Terminal does not support some characters and symbols. To overcome this issue, we will need to download [Nerd Fonts](https://www.nerdfonts.com/font-downloads) which gives better icons, symbol supports when using powerline. Recommends Caskaydia Cove or FireCode.

After Downloaded any Nerf Fonts, we need to install the fonts.

![Install Caskaydia Cove Font](https://i.imgur.com/8uMMlRt.png)

Go to Windows Terminal, navigate to Settings or press `Ctrl + ,` Settings shortcut. Select `Defaults` profile under `Profiles` list, and scroll down to `Appearance` under `Additional settings`.

![Select Appearance from Default profile settings](https://i.imgur.com/EWxLM9u.png)

The `Defaults` profile setting will applies to all terminal in Windows Terminal, if we want to specify the font specifically we can just navigate to the terminal profile and setup the font in that profile only.

At the Font face setting choose the Nerf Fonts we installed.

![Change font face](https://i.imgur.com/h19OQcK.png)

_Sometimes there will be some font and theme that is not fully compatible, we can find solution in GitHub issue if available or create an issue, or use another theme that works well with the font_

## References
1. [Tutorial: Set up a custom prompt for PowerShell or WSL with Oh My Posh](https://docs.microsoft.com/en-us/windows/terminal/tutorials/custom-prompt-setup#customize-your-wsl-prompt-with-oh-my-posh)