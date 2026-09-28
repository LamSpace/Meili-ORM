# Spec Delta

## MODIFIED Requirements

### Requirement: README 徽章与未发布真相

`README.md` 与其镜像顶部 SHALL 含徽章行，恰为五项：License Apache-2.0（链接 `LICENSE`）、CI 状态（指向本仓库真实 workflow，公开仓库匿名可访问）、Java 17+ 字节码基线、Spring Boot 3.5.x 与 4.x 双代、Meilisearch v1.x 服务端兼容；除 CI 徽章外 SHALL 以静态徽章呈现且语义与实现事实一致。构件未发布至公共仓库期间，README SHALL NOT 含 Maven Central 版本类徽章或任何暗示坐标可远程解析的表述；装配小节 SHALL 明示"尚未发布到 Maven Central"并给出源码构建安装步骤（clone → 根 `mvn install` → 本地解析坐标）与其适用边界。

#### Scenario: 幻影徽章缺席

- **WHEN** 在构件未发布状态下检查两份 README 的徽章与装配小节
- **THEN** 不存在 Central/Javadoc 托管/下载量类徽章链接，装配小节含未发布声明与源码构建路径

#### Scenario: 源码构建安装实测走通

- **WHEN** 干净环境（非维护者本机）严格按 README 源码构建步骤安装后按最小装配建工程编译
- **THEN** `meili-orm-spring-boot-starter` 经本地仓库解析成功，工程编译通过
