# Use personal access tokens on github

新的 github 访问安全策略, github 禁止了使用 password over https 的方式访问.

like this:

    https://username:password@github.com/username/gitrepos.git

从安全的设计来看, 不应该每次连接使用账户密码对来直接访问 github. 新的推荐方式是 token over https.

like this:

    https://token@github.com/username/gitrepos.git

Token 是一种可以指定受限规则的方式, 例如你可以指定一个只读的 rule for token, 并且可以设置有效期.

当然你仍然还可以使用 ssh with ~/.ssh/id_ed25519

## Personal Access Tokens 使用方式

- create personal access tokens

In your web Github Account, go to `Settings` / `Developer settings` / `Personal access tokens` and select `Generate New Token`.

这是你唯一能看到一次的地方,因为 token 在服务端不是明文存储的.

[personal access tokens](https://github.com/settings/tokens)

在 mac 上的 git 默认使用 osxkeychain 来存储 pasword 或 token.

- Add the token to your OSX Key Chain

首次使用:

```
git clone https://github.com/username/repo.git
Cloning into 'repo'...
Username for 'https://github.com': your_github_username
Password for 'https://username@github.com': your_access_token
```

- maybe reset your old git credentials on macos

```
$ git config credential.helper
osxkeychain
```

```
$ security find-internet-password -l github.com
keychain: "/Users/xxx/Library/Keychains/login.keychain-db"
version: 512
class: "inet"
attributes:
    0x00000007 <blob>="github.com"
    0x00000008 <blob>=<NULL>
    "acct"<blob>="bnse"
    "atyp"<blob>="dflt"
    ...(truncated)
```

```
$ security delete-internet-password -l github.com
keychain: "/Users/xxx/Library/Keychains/login.keychain-db"
version: 512
class: "inet"
attributes:
    0x00000007 <blob>="github.com"
    0x00000008 <blob>=<NULL>
    "acct"<blob>="bnse"
    "atyp"<blob>="dflt"
    ... (truncated)
password has been deleted.
```

or use next command:

```
$ git credential-osxkeychain erase ⏎
 host=github.com  ⏎
 protocol=https   ⏎
 ⏎
```

你也可以使用 GUI 来查看, open `Keychains Access` / Search `github.com`, 你会看到一个 `kind` 是 `internet password` 的`login`.
