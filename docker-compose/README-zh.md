# 使用 Docker Compose 运行 GrimoireLab

本文件夹包含通过 [Docker Compose](https://docs.docker.com/compose/) 运行 GrimoireLab 的配置文件，这是开始使用 GrimoireLab 最简单的方式之一。

# 系统要求

- [Git](https://git-scm.com/) 客户端
- [Docker 引擎](https://docs.docker.com/install/) 和 [Docker Compose](https://docs.docker.com/compose/install/)
- 推荐配置：至少 2 核 CPU、8GB 内存和 2GB 交换空间（Mac 用户可通过 [Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac) 管理）

例如，在我最近的一次演示中使用的环境配置：

```
root@qiskit-6fc71ea6:~# git --version
git version 2.17.1
root@qiskit-6fc71ea6:~# docker --version
Docker version 19.03.1, build 74b1e89
root@qiskit-6fc71ea6:~# docker-compose --version
docker-compose version 1.22.0, build f46880fe
root@qiskit-6fc71ea6:~# free
              total        used        free      shared  buff/cache   available
Mem:        8167996     3463652      128844        1440     4575500     4413328
Swap:             0           0           0
root@qiskit-6fc71ea6:~# du -hs /
du: cannot access '/proc/2234/task/2234/fd/4': No such file or directory
du: cannot access '/proc/2234/task/2234/fdinfo/4': No such file or directory
du: cannot access '/proc/2234/fd/3': No such file or directory
du: cannot access '/proc/2234/fdinfo/3': No such file or directory
26G     /
```

**注意**：`free` 和 `du` 是 Linux 工具，分别用于显示系统内存使用情况和文件空间占用情况。

请确保为 Elasticsearch（GrimoireLab 组件之一）分配足够的虚拟内存。在 Linux 上可以以 root 用户运行以下命令，在 Mac 上可以以管理员身份运行：

## Linux 系统

```
sysctl -w vm.max_map_count=262144
```

## Mac 系统

```
$ screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty
(然后运行:) sysctl -w vm.max_map_count=262144
```

同时请通过 Docker 图形界面确保分配了足够的资源。8GB 内存和 2GB 交换空间应该足够。

./mac-docker-configuration.png

# 三步快速开始

1. 克隆本项目：

```
git clone https://github.com/chaoss/grimoirelab
```

1. 默认情况下，本仓库包含 [`default-grimoirelab-settings`](https://yuanbao.tencent.com/chat/default-grimoirelab-settings) 中的一些配置文件。这些文件是分析本仓库 Git 活动的最小配置集。如果要分析不同项目或使用不同的代码库和数据源，需要修改 [`projects.json`](https://yuanbao.tencent.com/chat/default-grimoirelab-settings/projects.json) 和 [`setup.cfg`](https://yuanbao.tencent.com/chat/default-grimoirelab-settings/setup.cfg) 文件。关于这些文件格式的更多信息，请参见下文。
2. 进入 [`docker-compose`](https://yuanbao.tencent.com/chat/naQivTmsDa/) 文件夹并运行以下命令部署 GrimoireLab：

```
cd grimoirelab/docker-compose
docker-compose up -d
```

如果一切正常，数据将被收集和处理。访问 `http://localhost:8000/` 查看结果。

要使用 [SortingHat](https://github.com/chaoss/grimoirelab-sortinghat) 管理贡献者资料信息，请访问 `http://localhost:8000/identities/`。登录凭据：

- 用户名：`root`
- 密码：`root`

**注意**：可以在 `docker-compose.yml` 文件中修改用户名和密码。

## 安全基础设施

按照上述步骤部署的基础设施不会对生成的数据提供任何安全保护，因此**不要在生产环境或公开访问环境中使用此配置**。

如果需要部署具有基本安全保护的基础设施，请使用*安全*环境：

- 编辑 [`default-grimoirelab-settings/setup-secured.cfg`](https://yuanbao.tencent.com/chat/default-grimoirelab-settings/setup-secured.cfg) 以满足您的需求
- 使用 `docker-compose-secured.yml` 文件代替常规的 `docker-compose.yml`：

```
docker-compose -f docker-compose-secured.yml up -d
```

不同之处在于它使用了安全的 Elasticsearch 镜像和安全的 Kibiter 镜像。

访问仪表板的方式与之前相同，但如果需要修改设置，系统会要求登录。

- 用户名：`admin`
- 密码：`admin`

要修改这些凭据，需要进入 `elasticsearch` 容器并更改 [SearchGuard 插件](https://search-guard.com/) 的参数。

## OpenSearch 支持

OpenSearch 是 Elasticsearch 的开源分支，包含额外的功能和插件。它有自己的安全插件用于身份验证和访问控制。

GrimoireLab 支持 OpenSearch，但仪表板不会自动生成，需要手动导入。

如果要部署具有基本安全保护的基础设施，请使用 `docker-compose-opensearch.yml` 文件：

```
docker-compose -f docker-compose-opensearch.yml up -d
```

访问仪表板的方式与之前相同，但需要登录：

- 用户名：`admin`
- 密码：`admin`

此版本不包含可视化图表。需要从 [Sigils 仓库](https://github.com/chaoss/grimoirelab-sigils/tree/main/panels/json/opensearch_dashboards) 手动导入仪表板或自行创建。

更多关于 OpenSearch 的信息：

# 更多信息

- [`projects.json` 文件格式](https://github.com/chaoss/grimoirelab-sirmordred#projectsjson-)
- [`setup.cfg` 文件格式](https://github.com/chaoss/grimoirelab-sirmordred#setupcfg-)
- 获取不同服务的 API 令牌：
  - [GitHub](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line)
  - [GitLab](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html)
  - [Slack](https://get.slack.help/hc/en-us/articles/215770388-Create-and-regenerate-API-tokens)

## 常见问题

### Mac 上 Elasticsearch 容器无法启动

在 Mac 上运行默认的 `docker-compose.yml` 可能会导致 Elasticsearch 和 Kibana 版本出现问题。要解决这些问题，请使用 [OpenSearch 版本](https://yuanbao.tencent.com/chat/naQivTmsDa/60caf570-ba2c-48ff-a0e2-22eb178d07e0#opensearch)。

### 如何停止和重启已部署的软件基础设施？

可能有两种情况：

1. 如果只想进行简单的配置更改并重启平台：

```
docker-compose restart
```

1. 如果想销毁当前部署的配置，并在做出一些更改后重新开始：

```
docker-compose down
```

然后在它们*停止*后：

```
docker-compose up -d
```

如果使用的是*安全*环境，请记住命令不同：

```
docker-compose -f docker-compose-secured.yml up -d
```
