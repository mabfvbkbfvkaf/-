# 视频分享网站（video-tube）

一个用于学习 Spring Boot 全栈开发的视频分享网站单体应用（类似 B 站）。学生可以注册登录、浏览、上传视频并发表评论。（目前只是个 demo，仅搭建了简单框架，云端数据库、对象存储等都还没部署，后续会继续完善。）

## 目录

- [技术栈](#技术栈)
- [项目结构](#项目结构)
- [数据库设计](#数据库设计)
- [已实现功能](#已实现功能)
- [运行方式](#运行方式)

## 技术栈

| 分类 | 技术 |
| --- | --- |
| 语言 | Java 17 |
| 框架 | Spring Boot 3.2.5 |
| 模板引擎 | Thymeleaf（服务端渲染 HTML 页面） |
| ORM | MyBatis-Plus 3.5.7（`mybatis-plus-spring-boot3-starter`） |
| 数据库 | MySQL 8（驱动 `mysql-connector-j`） |
| 连接池 | HikariCP（Spring Boot 默认自带） |
| 前端样式 | Bootstrap 5.3（通过 CDN 引入） |
| 构建工具 | Maven |

## 项目结构

```
video-tube
├── pom.xml # Maven 依赖配置
├── sql/init.sql # 建表脚本 + 测试数据
└── src/main
    ├── java/com/example/videotube
    │   ├── VideoTubeApplication.java # 启动类
    │   ├── controller/ # 控制层（IndexController、VideoController、CommentController）
    │   ├── entity/ # 实体类（User、Video、Comment）
    │   ├── mapper/ # 数据访问层（继承 BaseMapper）
    │   └── service/ + service/impl/ # 服务层（继承 IService / ServiceImpl）
    └── resources
        ├── application.yml # 应用配置（端口、数据源、MyBatis-Plus）
        └── templates/ # Thymeleaf 页面（index.html、video_list.html、video_detail.html）
```

项目采用经典的分层架构：**Controller → Service → Mapper → MySQL** 。

## 数据库设计

- `user` 表：用户（id、username、password、nickname、avatar、create_time），username 唯一。

- `video` 表：视频（id、user_id、title、description、cover、video_url、category、view_count、create_time），user_id 关联用户。

- `comment` 表：评论（id、video_id、user_id、content、create_time），video_id 关联视频。

建表与测试数据见 `sql/init.sql`。

## 已实现功能

- 首页展示（`/`），展示分区与推荐视频

- 视频列表查询与展示（`/video/list`），通过 Thymeleaf `th:each` 遍历渲染

- 视频详情与评论区展示（`/video/{id}`）

## 运行方式

1. 执行 `sql/init.sql` 初始化数据库。

2. 修改 `src/main/resources/application.yml` 中的数据库连接地址与密码。

3. 运行启动类 `VideoTubeApplication`，或执行 `mvn spring-boot:run`。
