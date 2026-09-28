# Spec Delta

## MODIFIED Requirements

### Requirement: 双代矩阵模块

系统 SHALL 提供 `it/meili-orm-it-boot4` 与 `it/meili-orm-it-boot3` 两个 IT 模块：前者 dependencyManagement 导入 `spring-boot-dependencies` 4.0.3，后者导入 3.5.16（子模块 BOM 声明先于继承的父 BOM 取得覆盖）；两模块均依赖当前 reactor 产出的 `meili-orm-spring-boot-starter` 与 `meili-orm-core` test-jar，并在根 `mvn -s /home/lam/repo/settings.xml clean verify` 时各执行一轮。

#### Scenario: 根全量构建含双矩阵

- **WHEN** 在 Docker 与 v1.49.0 镜像可用的环境执行根 pom `clean verify`
- **THEN** it-boot3 与 it-boot4 两模块的 IT 均执行且 BUILD SUCCESS

#### Scenario: 各模块钉住对应代

- **WHEN** 分别执行 `mvn dependency:tree -pl it/meili-orm-it-boot3` 与 `-pl it/meili-orm-it-boot4`
- **THEN** 前者 `org.springframework.boot` 坐标全部解析为 3.5.16，后者全部解析为 4.0.3
