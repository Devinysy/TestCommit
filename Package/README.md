# VSBuilder 自动化打包流程工具

## VSBuilder 菜单接口介绍

1. VSBuilder/Exprot/一键打包 Release      用于构建对应平台的Release版本。
2. VSBuilder/Exprot/一键打包 Debug     用于构建对应平台的Debug版本。
3. VSBuilder/Exprot/一键打包 Release AAB     用于构建安卓Release版本的AAB包。
4. VSBuilder/Exprot/一键打包 Debug AAB     用于构建安卓Debug版本的AAB包。
5. VSBuilder/Create VSBuilderSetting     用于创建客户端打包相关配置。

## 使用需知

为了支持客户端自定义打包配置，打包前需执行VSBuilder/Create VSBuilderSetting。

执行完成之后，在Assets/VSBuildConfig/目录会生成VSBuilderSetting.asset配置文件。

配置默认启用了包体加密，如果需要解包分析，可以暂时关闭包体加密。

## 打包配置文件

![示例图片](Documentation~/image-Setting.png)

   配置目前主要分为基础设置、Global-Metadata加密设置、YooAsset相关设置、宏定义配置和混淆相关配置。
   客户端可通过项目需求修改打包配置进行打包。

## 构建工作流

![示例图片](Documentation~/image-BuildPipeline.png)

## 依赖

由于构建过程中需要对热更DLL和资源进行处理，使用此打包工具需依赖HybridCLR和YooAsset。

***

## YooAsset相关设置

![示例图片](Documentation~/image-Setting3.png)

配置可参照YooAsset/AssetBundle Builder面板，其中VSExcryption是自定义的加密资源包方法，构建正式包的时候需要进行配置。

## 宏定义列表处理

![示例图片](Documentation~/image-Setting2.png)

在打包配置中，客户端可配置正式包和开发包的宏定义列表。

## 代码混淆处理

代码混淆文档详见https://rcnj9f825ky8.feishu.cn/wiki/Q9eOwEgS1iPCYOk0If1cNrInnTe



## 新项目创建自动化打包

### 操作步骤
   1.保证Assets/VSBuildConfig/目录存在VSBuilderSetting.asset配置文件，如果不存在，执行VSBuilder/Create VSBuilderSetting生成打包相关配置。
   2.在打包机上创建对应项目的仓库相关目录，拉取工程。(如果没有权限，可以找打包机管理员创建)
   3.在库中复制流水线代码，根据项目需求修改其中的【项目自定义配置修改】部分。
      3.1 安卓 AndroidPipeline.groovy 相关配置修改
```
//**************************【项目自定义配置修改】**************************

   // 项目的分支名称，用于快速选取
   choice(name: 'SVN_TARGET', choices: ['svn://svn.yj.wanmei.com/ProjectT/trunk',
      'svn://svn.yj.wanmei.com/ProjectT/branches/TileVacation_1.0.0_2025-02-26',],description: '基于目标分支拉取')
   //项目名称
   PACKAGE_NAME = 'TileVacation'

   //svn 主项目仓库路径
   SVN_TRUNK = 'svn://svn.yj.wanmei.com/ProjectT/trunk'
   //svn 分支仓库路径
   SVN_BRANCH_PREFIX = 'svn://svn.yj.wanmei.com/ProjectT/branches'

   //打包机 仓库路径
   PROJECT_DIR_ROOT = '/Volumes/YuanJing/Tile/YJ_Android_Project'
   //打包机 项目路径
   PROJECT_DIR = '/Volumes/YuanJing/Tile/YJ_Android_Project/Client-ProjectT'
   //打包机 项目输出路径
   OUTPUT_DIR = '/Volumes/YuanJing/Tile/YJ_Android_Project/Client-ProjectT/Release-Android'
   //打包机 签名配置路径
   TILECONFIG_PATH = '/Volumes/YuanJing/Tile/TileConfig'
   //打包机 项目备份路径
   TILESTORE_PATH = '/Volumes/YuanJing/Tile打包仓库/Android'

   //上传网盘路径
   UPLOAD_DIR_ANDROID = '/Tile/Android'

   //firebase app id 联系sdk修改
   FIREBASE_APP_ID_ANDROID = '1:764942745066:android:ac7071aa15e87af62ac693'

   //**************************【项目自定义配置修改】**************************
```
      3.2 iOS  iOSPipeline.groovy 相关配置修改
```
//**************************【项目自定义配置修改】**************************

//项目名称

   // 项目的分支名称，用于快速选取
   choice(name: 'SVN_TARGET', choices: ['svn://svn.yj.wanmei.com/ProjectT/trunk',
      'svn://svn.yj.wanmei.com/ProjectT/branches/TileVacation_1.0.0_2025-02-26',
      'svn://svn.yj.wanmei.com/ProjectT/branches/TileVacation_1.1.0_2025-03-10',
      'svn://svn.yj.wanmei.com/ProjectT/branches/TileVacation_1.2.0_2025-03-14',
      'svn://svn.yj.wanmei.com/ProjectT/branches/TileVacation_1.3.0_2025-04-01'],description: '基于目标分支拉取')

   PACKAGE_NAME = 'TileVacation'

   //svn 主项目仓库路径
   SVN_TRUNK = 'svn://svn.yj.wanmei.com/ProjectT/trunk'
   //svn 分支仓库路径
   SVN_BRANCH_PREFIX = 'svn://svn.yj.wanmei.com/ProjectT/branches'
   //打包机 仓库路径
   PROJECT_DIR_ROOT = '/Volumes/YuanJing/Tile/YJ_iOS_Project'
   //打包机 项目路径
   PROJECT_DIR = '/Volumes/YuanJing/Tile/YJ_iOS_Project/Client-ProjectT'
   //打包机 项目输出路径
   OUTPUT_DIR = '/Volumes/YuanJing/Tile/YJ_iOS_Project/Client-ProjectT/Release-iOS'
   //打包机 项目备份路径
   TILESTORE_PATH = '/Volumes/YuanJing/Tile打包仓库/iOS'

   //上传网盘路径
   UPLOAD_DIR_IOS = '/Tile/iOS'

   //firebase app id 联系sdk修改
   FIREBASE_APP_ID_IOS = '1:764942745066:ios:d12e284857d5ed112ac693'

   //ipa名字 用于上传
   IOS_BUNDLENAME = 'TileVacation'

   //**************************【项目自定义配置修改】**************************
```
   4.配置Jenkins
      4-1 创建新的项目仓库，存储该项目所有的打包任务。
      4-2 创建新的Jenkins项目，类型选择流水线。(也可以选择已经存在的项目通过模版创建)
      4-3 Pipeline类型选择Pipeline script,把*.groovy中的代码复制到此处。第一次直接运行，注意只有运行过一次之后(无论成功与否)，参数列表才可以刷新。
      4-4 执行任务Build with Parameters，设置参数打包。

### 注意事项
   #### 安卓
      1.一般来说keystore文件不会放在工程里，联系sdk生成好后放入打包机项目对应的配置目录中。
   #### iOS
      1.确保证书和签名文件已经导入到打包机中。
      2.ios导出工程的相关配置在VSBuildConfig\Config\iOS\build.json中，项目需要结合需要修改。如设置"CFBundleDisplayName": "Tile Vacation"显示最终包名;"NSCameraUsageDescription": "Need your consent, APP can access the camera."权限相关描述等配置。
