# Git Private Access

When repository is private and we try to execute `git` commands for the repository, it will requires authentication everytime. This page is to reduce the count for us to input username and password everytime we execute `git` commands for private repository.

Configure `user.name` and `user.email` if not been done. Replace `name` and `email` to our own name and email.

```
git config --global user.name name;
git config --global user.email email;
```

Go to GitHub website. Navigate to `Settings` > `Developer settings` > `Personal access tokens`.

![Navigate To Developer settings](https://i.imgur.com/2N5scZW.png)

![Navigate to Personal access tokens](https://i.imgur.com/rrrWTgX.png)

Then clicks on `Generate new token`. Fill up `Note`, `Expiration`, `Select scopes`. We will check all repo scopes which give us full control of the repository includign private repository.

![Fill up Personal access token details](https://i.imgur.com/d1x43zt.png)

## Cache username and password

Before we proceed to use the PAT. Execute cache command so that we do not need to re-type username and password everytime.

### Linux

```
git config --global credential.helper cache
```

### Windows

```
git config --global credential.helper manager
```

### MacOS

```
git config --global credential.helper osxkeychain
```

## Git Access

Then git clone from private repository. It will prompt for username and password. Fill in the username and the PAT we generated as password. The operation will be completed.

## Remove cache username and password

### Linux

```
git config --global --unset credential.helper
```

### Windows

Use Windows Search and search for `Credential Manager`. Then Navigate to `Windows Crendetials`. Search for GitHub credentials and remove them.

### MacOS

#### From Terminal
Press `Enter` after enter these 3 lines in terminal
```
git credential-osxkeychain erase 
host=github.com 
protocol=https 
```

####  From Keychain Access
or find GitHub credential in `Keychain Access` and remove from the list.