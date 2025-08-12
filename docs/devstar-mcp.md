# Gitea MCP 服务器

**Gitea MCP 服务器** 是一个集成插件，旨在将 Gitea 与 Model Context Protocol (MCP) 系统连接起来。这允许通过 MCP 兼容的聊天界面无缝执行命令和管理仓库。

[![在 VS Code 中使用 Docker 安装](https://img.shields.io/badge/VS_Code-Install_Server-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=gitea&inputs=[{%22id%22:%22gitea_token%22,%22type%22:%22promptString%22,%22description%22:%22Gitea%20Personal%20Access%20Token%22,%22password%22:true}]&config={%22command%22:%22docker%22,%22args%22:[%22run%22,%22-i%22,%22--rm%22,%22-e%22,%22GITEA_ACCESS_TOKEN%22,%22docker.gitea.com/gitea-mcp-server%22],%22env%22:{%22GITEA_ACCESS_TOKEN%22:%22${input:gitea_token}%22}}) [![在 VS Code Insiders 中使用 Docker 安装](https://img.shields.io/badge/VS_Code_Insiders-Install_Server-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=gitea&inputs=[{%22id%22:%22gitea_token%22,%22type%22:%22promptString%22,%22description%22:%22Gitea%20Personal%20Access%20Token%22,%22password%22:true}]&config={%22command%22:%22docker%22,%22args%22:[%22run%22,%22-i%22,%22--rm%22,%22-e%22,%22GITEA_ACCESS_TOKEN%22,%22docker.gitea.com/gitea-mcp-server%22],%22env%22:{%22GITEA_ACCESS_TOKEN%22:%22${input:gitea_token}%22}}&quality=insiders)

## 目录

- [Gitea MCP 服务器](#gitea-mcp-服务器)
  - [目录](#目录)
  - [什么是 Gitea？](#什么是-gitea)
  - [什么是 MCP？](#什么是-mcp)
  - [🚧 安装](#-安装)
    - [🔧 从源代码构建](#-从源代码构建)
    - [📁 添加到 PATH](#-添加到-path)
  - [🚀 使用](#-使用)
  - [✅ 可用工具](#-可用工具)
  - [🐛 调试](#-调试)
  - [🛠 疑难排解](#-疑难排解)

## 什么是 Gitea？

Gitea 是一个由社区管理的轻量级代码托管解决方案，使用 Go 语言编写。它以 MIT 许可证发布。Gitea 提供 Git 托管，包括仓库查看器、问题追踪、拉取请求等功能。

## 什么是 MCP？

Model Context Protocol (MCP) 是一种协议，允许通过聊天界面整合各种工具和系统。它能够无缝执行命令和管理仓库、用户和其他资源。

## 🚧 安装

### 🔧 从源代码构建

您可以使用 Git 克隆仓库来下载源代码：

```bash
git clone https://gitea.com/gitea/gitea-mcp.git
```

在构建之前，请确保您已安装以下内容：

- make
- Golang (建议使用 Go 1.24 或更高版本)

然后运行：

```bash
make install
```

### 📁 添加到 PATH

构建后，将二进制文件 gitea-mcp 复制到系统 PATH 中包含的目录。例如：

```bash
cp gitea-mcp /usr/local/bin/
```

## 🚀 使用

此示例适用于 Cursor，您也可以在 VSCode 中使用插件。
要配置 Gitea 的 MCP 服务器，请将以下内容添加到您的 MCP 配置文件中：

- **stdio 模式**

```json
{
  "mcpServers": {
    "gitea": {
      "command": "gitea-mcp",
      "args": [
        "-t",
        "stdio",
        "--host",
        "https://devstar.cn"
        // "--token", "<your personal access token>"
      ],
      "env": {
        // "GITEA_HOST": "https://devstar.cn",
        // "GITEA_INSECURE": "true",
        "GITEA_ACCESS_TOKEN": "<your personal access token>"
      }
    }
  }
}
```

- **sse 模式**

```json
{
  "mcpServers": {
    "gitea": {
      "url": "http://localhost:8080/sse"
    }
  }
}
```

- **http 模式**

```json
{
  "mcpServers": {
    "gitea": {
      "url": "http://localhost:8080/mcp"
    }
  }
}
```

**默认日志路径**: `$HOME/.gitea-mcp/gitea-mcp.log`

> [!注意]
> 您可以通过命令行参数或环境变量提供您的 Gitea 主机和访问令牌。
> 命令行参数具有最高优先级

一切设置完成后，请尝试在您的 MCP 兼容聊天框中输入以下内容：

```text
列出我所有的仓库
```

## ✅ 可用工具

Gitea MCP 服务器支持以下工具：

|             工具             |   范围   |             描述             |
| :--------------------------: | :------: | :--------------------------: |
|       get_my_user_info       |   用户   |     获取已认证用户的信息     |
|        get_user_orgs         |   用户   |   获取已认证用户关联的组织   |
|         create_repo          |   仓库   |        创建一个新仓库        |
|          fork_repo           |   仓库   |         复刻一个仓库         |
|        list_my_repos         |   仓库   | 列出已认证用户拥有的所有仓库 |
|        create_branch         |   分支   |        创建一个新分支        |
|        delete_branch         |   分支   |         删除一个分支         |
|        list_branches         |   分支   |     列出仓库中的所有分支     |
|        create_release        | 版本发布 |      创建一个新版本发布      |
|        delete_release        | 版本发布 |       删除一个版本发布       |
|         get_release          | 版本发布 |       获取一个版本发布       |
|      get_latest_release      | 版本发布 |      获取最新的版本发布      |
|        list_releases         | 版本发布 |       列出所有版本发布       |
|          create_tag          |   标签   |        创建一个新标签        |
|          delete_tag          |   标签   |         删除一个标签         |
|           get_tag            |   标签   |         获取一个标签         |
|          list_tags           |   标签   |         列出所有标签         |
|      list_repo_commits       |   提交   |     列出仓库中的所有提交     |
|       get_file_content       |   文件   |    获取文件的内容和元数据    |
|        get_dir_content       |   文件   |      获取目录的内容列表      |
|         create_file          |   文件   |        创建一个新文件        |
|         update_file          |   文件   |         更新现有文件         |
|         delete_file          |   文件   |         删除一个文件         |
|      get_issue_by_index      |   问题   |       根据索引获取问题       |
|       list_repo_issues       |   问题   |     列出仓库中的所有问题     |
|         create_issue         |   问题   |        创建一个新问题        |
|     create_issue_comment     |   问题   |       在问题上创建评论       |
|          edit_issue          |   问题   |         编辑一个问题         |
|      edit_issue_comment      |   问题   |      在问题上编辑评论         |
| get_issue_comments_by_index  |   问题   |     根据索引获取问题的评论     |
|  get_pull_request_by_index   | 拉取请求 |     根据索引获取拉取请求     |
|   list_repo_pull_requests    | 拉取请求 |   列出仓库中的所有拉取请求   |
|     create_pull_request      | 拉取请求 |      创建一个新拉取请求      |
|         search_users         |   用户   |           搜索用户           |
|       search_org_teams       |   组织   |       搜索组织中的团队       |
|         search_repos         |   仓库   |           搜索仓库           |
| get_gitea_mcp_server_version |   服务器    |        获取 Gitea MCP 服务器的版本         |

## 🐛 调试

要启用调试模式，请在使用 sse 模式运行 Gitea MCP 服务器时添加 `-d` 标志：

```sh
./gitea-mcp -t sse [--port 8080] --token <your personal access token> -d
```

## 🛠 疑难排解

如果您遇到任何问题，以下是一些常见的疑难排解步骤：

1. **检查您的 PATH**: 确保 `gitea-mcp` 二进制文件位于系统 PATH 中包含的目录中。
2. **验证依赖项**: 确保您已安装所有所需的依赖项，例如 `make` 和 `Golang`。
3. **检查配置**: 仔细检查您的 MCP 配置文件是否有任何错误或遗漏的信息。
4. **查看日志**: 检查日志中是否有任何错误消息或警告，可以提供有关问题的更多信息。

享受通过聊天探索和管理您的 Gitea 仓库的乐趣！
