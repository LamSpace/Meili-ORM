# Spec Delta

## MODIFIED Requirements

### Requirement: 多模块聚合结构

系统 SHALL 提供聚合根 pom（packaging=pom），模块清单为：`meili-orm-core`、`meili-orm-spring-boot-autoconfigure`、`meili-orm-spring-boot-starter`、`meili-orm-serializer-jackson3`、`meili-orm-repository`、`it`（含 `it/meili-orm-it-boot3`、`it/meili-orm-it-boot4`）、`examples`（含 common 与 boot3/boot4 两 app）；`meili-orm-core` 的编译期依赖 SHALL NOT 含任何 `org.springframework` 坐标；`meili-orm-repository` SHALL 依赖 `meili-orm-core` 与 `meili-orm-spring-boot-autoconfigure`，其编译期 `spring-data-commons` 版本 SHALL 由 Boot 3.5.16 BOM 管理的 3.5.13 解析（最低支持代编译基线），且 SHALL NOT 被 `meili-orm-spring-boot-starter` 聚合传递。

#### Scenario: 全量构建通过

- **WHEN** 执行 `mvn -s /home/lam/repo/settings.xml clean verify`
- **THEN** 全部模块（含 repository、双矩阵与 examples）构建成功（BUILD SUCCESS），无测试编译错误

#### Scenario: core 零 Spring 依赖

- **WHEN** 执行 `mvn dependency:tree -pl meili-orm-core`
- **THEN** 输出中不存在 `org.springframework` 组坐标

#### Scenario: starter 不传递 commons

- **WHEN** 执行 `mvn dependency:tree -pl meili-orm-spring-boot-starter`
- **THEN** 输出不含 `meili-orm-repository` 与 `org.springframework.data:*` 坐标
