# GrimoireLab  
grimoirelab-showcase（GrimoireLab展示）

GrimoireLab是CHAOSS项目旗下的软件开发分析工具集。它包含一系列协同工具，用于从支持软件开发的系统（代码仓库）中检索数据，将数据存储在数据库中，通过计算相关指标来富集数据，并便于对数据进行分析和可视化。

你可以在GrimoireLab教程中了解更多关于GrimoireLab的信息，或访问GrimoireLab官网。

GrimoireLab中提供的指标部分是在CHAOSS项目中开发的。有关CHAOSS指标的更多信息，请参见最新版本：https://chaoss.community/metrics/


## 快速开始
为了方便新手使用，我们提供了一个默认设置来分析此仓库的git活动。要进行此设置，有多种方式可以运行GrimoireLab：


### 使用docker-compose
**要求：**

- 软件：git、docker客户端和docker compose。工作配置示例：
  ```
  root@test-68b8628f:~# git --version
  git version 2.17.1
  root@test-68b8628f:~# docker --version
  Docker version 19.03.1, build 74b1e89
  root@test-68b8628f:~# docker-compose --version
  docker-compose version 1.22.0, build f46880fe
  ```
- 硬件：2个CPU核心、8GB内存以及足够的Elasticsearch虚拟内存

**步骤：**

1. 克隆此项目：
   ```bash
   git clone https://github.com/chaoss/grimoirelab
   ```

2. 进入docker-compose文件夹并运行以下命令：
   ```bash
   cd grimoirelab/docker-compose
   docker-compose up -d
   ```

一段时间后，你的仪表盘将在http://localhost:8000就绪。等待时间取决于从仓库获取的数据量，对于小型仓库，预计10-15分钟后可在仪表盘中看到数据。

更多详细信息或故障排除请查看docker-compose文件夹。


### 使用docker run
**要求：**

- 软件：git和docker客户端。工作配置示例：
  ```
  root@test-68b8628f:~# git --version
  git version 2.17.1
  root@test-68b8628f:~# docker --version
  Docker version 19.03.1, build 74b1e89
  ```
- 硬件：2个CPU核心、8GB内存，并且ElasticSearch和Kibana已启动运行，SortingHat已启动运行

**步骤：**

1. 克隆此项目：
   ```bash
   git clone https://github.com/chaoss/grimoirelab
   ```

2. 进入项目文件夹并运行以下命令：
   ```bash
   cd grimoirelab
   docker run --net=host \
       -v $(pwd)/default-grimoirelab-settings/projects.json:/home/grimoire/conf/projects.json \
       -v $(pwd)/default-grimoirelab-settings/setup-docker.cfg:/home/grimoire/conf/setup.cfg \
       -t grimoirelab/grimoirelab
   ```

一段时间后，你的仪表盘将在http://localhost:8000就绪。等待时间取决于从仓库获取的数据量，对于小型仓库，预计10-15分钟后可在仪表盘中看到数据。

更多详细信息请查看docker文件夹。


### 在FreeBSD上使用
最近，FreeBSD采用了GrimoireLab进行Bugzilla分析，并在其集群中部署了GrimoireLab。FreeBSD基金会友好地分享了在FreeBSD上运行Grimoire的过程，相关内容可在此处查看。本指南也将帮助那些有兴趣从源代码运行Grimoirelab而不是通过Docker部署的人。


## 重大变更
GrimoireLab 1.3.0：SortingHat权限组

从GrimoireLab 1.3.0开始，在SortingHat中创建新用户需要将他们分配到权限组。默认情况下，他们将拥有只读权限。请参考以下文档了解如何更新权限：将用户分配到权限组


## GrimoireLab组件
目前，GrimoireLab工具集由以下仓库组成：

- **数据检索相关组件：**
  - Perceval：从数据源检索数据
  - Perceval（OPNFV专用版本）
  - Perceval（Mozilla专用版本）
  - Perceval（Puppet专用版本）
  - Perceval（Weblate专用版本）
  - Graal：使用外部工具进行源数据分析
  - KingArthur：用于大规模检索的批处理

- **数据富集相关组件：**
  - GrimoireElk：数据存储和富集
  - Cereslib：通用数据处理器
  - SortingHat：身份管理

- **数据消费相关组件：**
  - Kibiter：仪表盘，Kibana的下游版本
  - Sigils：可视化和仪表盘
  - Kidash：可视化和仪表盘管理器
  - Manuscripts：报告生成

- **平台管理、编排和通用工具：**
  - Mordred：编排工具
  - GrimoireLab Toolkit：通用工具
  - Bestiary：基于Web的用户界面，用于管理Mordred的仓库和项目

还有一些由GrimoireLab社区构建的组件，可能对你有用。其他相关仓库包括：
- GrimoireLab教程
- 整体的GrimoireLab（本仓库）


## 本仓库内容
本仓库包含与整体GrimoireLab相关的内容。例如：

- 影响多个GrimoireLab模块的新功能或错误报告的问题。在这种情况下，请在此处打开一个问题，并且在实施修复或功能时，请评论所使用的特定模块中的特定工单。例如，当支持新的数据源时，我们至少需要在Perceval、GrimoireELK和面板中进行补丁。在这种情况下，我们会为整个案例打开一个功能请求（或用户故事），在Perceval中为数据检索器打开一个问题（以及后来的拉取请求），在GrimoireELK中为富集代码打开同样的问题，在面板中为Kibiter面板打开同样的问题。

- 大多数GrimoireLab组件的发行说明（releases目录）。

- 用于展示GrimoireLab的Docker容器（docker目录）。包括可用于演示该技术的GrimoireLab容器的Dockerfile和配置文件，并且可以作为实际部署的基础。更多信息请参见docker目录下的README.md文件。

如果你更习惯使用docker-compose，docker-compose文件夹包含使用docker-compose命令部署GrimoireLab的说明和配置文件。

GrimoireLab组件的源代码在src中可用。每个目录都是一个Git子模块，因此克隆仓库后其内容不会立即可用。要获取所有数据并获取最新版本，可以运行以下命令：
```bash
git submodule update --init --remote
```


## GrimoireLab的发布构建和测试方式：构建


## 引用
如果你在研究论文中使用GrimoireLab，请参考《GrimoireLab：一个软件开发分析工具集》：

- APA格式：
  Dueñas S, Cosentino V, Gonzalez-Barahona JM, del Castillo San Felix A, Izquierdo-Cortazar D, Cañas-Díaz L, Pérez García-Plaza A. 2021. GrimoireLab: A toolset for software development analytics. PeerJ Computer Science 7:e601 https://doi.org/10.7717/peerj-cs.601

- BibTeX / BibLaTeX格式：
  ```
  @Article{duenas2021:grimoirelab,
    author =      {Dueñas, Santiago and Cosentino, Valerio and Gonzalez-Barahona, Jesus M. and del Castillo San Felix, Alvaro and Izquierdo-Cortazar,  Daniel and Cañas-Díaz, Luis and Pérez García-Plaza, Alberto},
    title =       {GrimoireLab: A toolset for software development analytics},
    journaltitle = {PeerJ Computer Science},
    date =        {2021-07-09},
    volume =      7,
    number =      {e601},
    doi =         {10.7717/peerj-cs.601},
    url =         {https://doi.org/10.7717/peerj-cs.601}}
  ```


## 贡献
欢迎贡献，请查看《贡献指南》。
