# Node Setup (Node Manager)

To manage different versions of Node, we will use `n` that able to install and switch different version of Node at anytime.

## n Installation

Install `n` using HomeBrew.

```
brew install n
```

Due to permission issue, we not able to use `n` to install node under `/usr/local/n`, we will use `N_PREFIX` to tell `n` to install the node binaries in another folder.

## n Configuration

1. Create a `n` directory at `$HOME`

    ```
    mkdir $HOME/.n
    ```

2. Then define `N_PREFIX` in choice of shell.

    ### bash ($HOME/.bashrc)

    ```
    echo -e "\n# n\nexport N_PREFIX=\$HOME/.n" >> $HOME/.bashrc
    ```

    ### zsh ($HOME/.zshrc)

    ```
    echo -e "\n# n\nexport N_PREFIX=\$HOME/.n" >> $HOME/.zshrc
    ```

    ### fish

    use `nano`, or other choice of editor to edit and add below code into `~/.config/fish/config.fish`.

    ```
    set N_PREFIX $HOME/.n
    ```

3. Add new binary to `$PATH`.

    ### bash

    ```
    echo -e "export PATH=\$N_PREFIX/bin:\$PATH" >> $HOME/.bashrc
    ```

    ### zsh

    ```
    echo -e "export PATH=\$N_PREFIX/bin:\$PATH" >> $HOME/.zshrc
    ```

    ### fish

    use `nano`, or other choice of editor to edit and add below code into `~/.config/fish/config.fish`.

    ```
    set PATH $PATH $N_PREFIX/bin
    ```

