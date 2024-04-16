# Git 操作

### 在 `git clone` 时指定使用的 ssh key

在 `git clone` 命令中指定使用的 SSH 密钥可以通过临时或永久的方式进行配置。

**临时指定 SSH Key**：
当你希望仅针对一次 `git clone` 操作使用特定的 SSH 密钥时，可以在命令行中添加 `--config` 参数来指定：

```shell
git clone git@github.com:user/repo.git --config core.sshCommand="ssh -i ~/.ssh/your_private_key"
```
请将 `~/.ssh/your_private_key` 替换为你的实际私钥文件路径。

**永久指定 SSH Key**：
如果你想让 Git 对所有操作都默认使用某个特定的 SSH 密钥，可以全局配置 `core.sshCommand`：

```shell
git config --global core.sshCommand "ssh -i ~/.ssh/your_private_key"
```
同样，这里的 `~/.ssh/your_private_key` 是你想要使用的私钥文件路径。

请注意，在某些情况下，你可能需要管理多个 GitHub/GitLab/Bitbucket 等账户，并为每个账户各自配置不同的 SSH 密钥。此时，更推荐使用 `ssh-config` 文件来管理多个身份：

1. 在 `~/.ssh/config` 文件中添加相应的配置项，例如：

   ```ini
   # GitHub Personal Account
   Host github-personal
     HostName github.com
     User git
     IdentityFile ~/.ssh/id_rsa_personal

   # GitHub Work Account
   Host github-work
     HostName github.com
     User git
     IdentityFile ~/.ssh/id_rsa_work
   ```

2. 使用配置好的主机名来 `git clone`：

   ```shell
   git clone git@github-personal:user/repo.git
   ```

这样，当通过 `github-personal` 主机名访问 GitHub 时，Git 将自动使用配置中的 `id_rsa_personal` 私钥。