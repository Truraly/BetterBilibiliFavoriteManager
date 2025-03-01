# B 站视频收藏管理工具 | Better Bilibili Favorite Manager (BBFM)

B 站视频收藏工具

目标：比原生 B 站更好的视频收藏管理工具，同时提供更好的数据安全和隐私保护（通过开源&本地化部署）

> 项目策划中，欢迎提出建议

## 功能介绍&完成情况

### B 站视频播放页

1. - [ ] 收藏/取消收藏该视频到一个或多个分类下
2. - [ ] 新建分类
3. - [ ] 用户访问任意视频时，会更新数据库中该视频的信息
4. - [ ] 记录用户观看视频的历史，包括观看时长和位置，以及点击数
5. - [ ] _添加收藏时预判分类（基于机器学习或者分类算法）（暂不考虑）_

### B 站收藏夹页

1. - [ ] 当收藏夹中视频失效时，用数据库中的信息提供帮助

### 视频收藏管理面板

1. - [ ] 查看各个分类下的视频列表
2. - [ ] 批量收藏/取消收藏一些视频到一个或多个分类下
3. - [ ] 可视化查看收藏夹数据/观看记录数据
   - [ ] 通过统计数据，提供数据分析
   - [ ] _实现类似飞书的多媒体数据库功能（暂不考虑）_
4. - [ ] 查看某个视频历史数据
5. - [ ] 通过多条件的筛选搜索视频
   - UP 主
   - 发布时间
   - tags
   - 分类
6. - [ ] 数据导出，备份和恢复

### _B 站显示优化（暂不考虑）_

1. 在视频推荐列表屏蔽指定 UP 主/关键词的视频
2. 优化广告显示
3. 关闭指定筛选条件的视频
4. 优化收藏夹的随机播放功能

---

因为相关的规定和 B 站的反爬虫机制，工具不会主动爬去视频信息，而是对用户获取到的视频信息进行处理，故而信息的更新仅可以在用户访问时发生

## 项目结构

代码分为 3 个部分

1. 油猴插件
2. 信息处理服务器
3. 用户界面（单独的 web 或者 app）

## 数据结构，对象

视频信息

- BVid
- UP 主（Array）
  - UP 主 ID
  - UP 主头像
  - UP 主简介
- 视频封面
- 简介
- 标题
- 播放量
- 当前数据更新时间
- 点赞数
- 硬币数
- 收藏数
- 转发数
- 弹目数
- 视频发布时间
- 视频时长
- 所属收藏夹（可选）
- 所属专题（可选）
- tags（Array）
- 视频点击数

视频历史信息列表

- 视频历史信息（Array）

用户观看记录

- 视频观看时常/位置
- BVid
- uid
- 开始时间
- 结束时间

收藏夹

- 收藏夹 ID
- 收藏夹名称
- 收藏夹描述
- 收藏夹所属用户
- 收藏夹视频（Array）

用户信息

- 用户 id
- 用户头像
- 用户简介

## 数据库

视频信息表

- BV 号
- UP 主 ID
- 视频封面
- 简介
- 标题
- 播放量
- 当前数据更新时间
- 点赞数
- 硬币数
- 收藏数
- 转发数
- 弹目数
- 视频发布时间
- 视频时长

- 所属收藏夹（可选）
- 所属专题（可选）

> 联合投稿怎么算？视频-UP 主表

视频-tag 表

- tag 名称
- 视频

用户帐号头像对应信息（UP 主也是用户）

- 用户 id
- 用户头像
- 用户简介

观看记录表

- 视频观看时常
- 视频观看位置
- timestamp
- 观看用户

> 因为观看记录数会明显大于视频更新数，所以观看记录和视频信息分开存储

收藏夹

- 视频 ID
- 收藏夹 ID
- 收藏序号

收藏夹信息

- 收藏夹 ID
- 收藏夹名称
- 收藏夹描述
- 收藏夹所属用户

视频点击数表

- 视频 BV
- 用户 ID
- 点击时间

## 接口

记录用户观看记录

- 观看记录表

更新视频信息

- 视频信息表
- 视频-tag 表
- UP 主信息表

创建收藏夹

- 收藏夹信息表

删除收藏夹

- 收藏夹信息表
- 收藏夹表

收藏夹改名

- 收藏夹信息表

收藏视频

- 收藏夹表

取消收藏视频

- 收藏夹表

查看收藏夹（uid，fid，页签）

- 收藏夹表
- 视频信息表
- 视频-tag 表
- UP 主信息表
- 收藏夹信息表

查看观看记录（开始时间，结束时间，用户 ID）

- 观看记录表
- 视频信息表
- 视频-tag 表
- UP 主信息表
- 收藏夹信息表
- 收藏夹表
- 视频点击数表

查询视频：

- 是否是收藏视频
- 视频信息

> 用户可以通过很多限定条件来查询视频，比如 UP 主，发布时间，标题，简介，历史信息等等


## 草稿

根据标题搜索
排除或包含某些tag
排除或包含某些分类
排除或包含某些up主
发布的时间范围
历史记录/收藏夹
上次观看的时间范围

视频信息和视频历史信息是否应该分开来呢？
记录视频的历史标题有多大意义？
历史记录是不是只需要播放点赞收藏转发硬币数就行了呢？