# Spec Delta

## MODIFIED Requirements

### Requirement: starter 聚合坐标

`meili-orm-spring-boot-starter` SHALL 以纯聚合 pom 形式依赖 Boot 基础 starter、`meili-orm-core` 与 `meili-orm-spring-boot-autoconfigure`，自身不含源码；用户仅声明该坐标即可获得全部运行期类与自动配置。该坐标名 SHALL 遵循 Spring Boot 第三方 starter 命名约定（`<名称>-spring-boot-starter`，不使用官方保留前缀 `spring-boot-starter-`）。

#### Scenario: 单依赖引全栈

- **WHEN** 应用 pom 仅声明 meili-orm-spring-boot-starter 坐标
- **THEN** classpath 含 core、autoconfigure、SDK 传递依赖与 Boot 基础件，上下文可装配
