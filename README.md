# [shiraha.moe](https://shiraha.moe/) 域名保护页面

没什么好说的，只能说不能备案的域名就是这么卑微（

溜了溜了，看来只能自墙国内了！

## Github Action 使用说明

增加了 Github Action 支持；现在可以更新时或手动触发 CI，自动部署到对应的 Git 仓库中使用 Page 服务；使用前，你需要使用 `ssh-keygen` 生成一对 SSH 密钥用于自动部署。

### 目标仓库

将生成的密钥的公钥增加为仓库的部署密钥（Deploy Key）；如果在 Github 上托管的仓库，这个入口在 仓库详情页 > Settings > Deploy keys 处，**并允许写入权限**。

### 代码仓库

你需要为代码仓库（一般地，就是本仓库）增加以下的 Secrets：

| 变量名           | 说明                                      | 推荐值              |
| ---------------- | ----------------------------------------- | ------------------- |
| `SSH_SECRET_KEY` | 用于推送到目标仓库的 SSH 私钥。           | 填入上述生成的私钥  |
| `SITE_CNAME`     | 你将要部署的 Github Page 的自定义 CNAME。 | 填入 `*.github.io`  |
| `TARGET_BRANCH`  | 你要推送到目标仓库的分支名                | `main` 或 `gh-page` |
| `TARGET_REPO`    | 你要推送到的目标仓库的 SSH 地址           | 仓库的 SSH 地址     |

如果你的仓库托管在 Github 上，则添加这些变量的入口在 仓库详情页 > Settings > Secrets 处。

### 特别说明

如果你将要部署的仓库不在 Github 上，请按照下面的提示修改 CI 脚本 `deploy.yml`：

```diff
- ssh-keyscan github.com >> ~/.ssh/known_hosts
+ ssh-keyscan 你的托管服务提供商 >> ~/.ssh/known_hosts
```

此外，站点的静态资源请提前放置在 `public` 目录下，并将它们增加到工作树中。