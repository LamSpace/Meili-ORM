# Proposal

## Why

现 starter 坐标 `spring-boot-starter-meili-orm` 以保留前缀 `spring-boot-starter-` 起头，违反 Spring Boot 官方对第三方 starter 的命名约定（官方文档 *Creating Your Own Starter → Naming a starter* 明确该前缀保留给 Spring 官方发布的 starter，第三方应采用 `<名称>-spring-boot-starter` 形式，如 MyBatis 的 `mybatis-spring-boot-starter`）。项目自身已按约定命名 `meili-orm-spring-boot-autoconfigure`，starter 是家族中唯一不一致的异类；且项目尚未发布 Maven Central（`1.0-SNAPSHOT`、无下游用户），此刻改名是零迁移成本的最后窗口。

## What Changes

- starter 模块目录与 Maven 坐标由 `spring-boot-starter-meili-orm` 重命名为 `meili-orm-spring-boot-starter`（artifactId、`<name>`、root pom `<modules>` 条目同步；聚合依赖内容不变）。**BREAKING**（仅对以本地安装方式引用旧坐标的使用者；发布前无外部影响）
- `it/meili-orm-it-boot3`、`it/meili-orm-it-boot4`、`it/meili-orm-it-boot4-jackson3`、`examples/meili-orm-example-common`、`examples/meili-orm-example-boot3`、`examples/meili-orm-example-boot4` 六个模块 pom 中对该坐标的依赖声明更新。
- `meili-orm-repository` 的 `MeiliRepositoryFactoryBean` 异常消息中提示的 classpath 坐标名更新（src/main 文案，随 CLAUDE.md 注释约定同步）。
- 双语文档同步：`README.md` / `README.zh-CN.md`（安装坐标片段、目录树）、`CONTRIBUTING.md` / `CONTRIBUTING.zh-CN.md`、`docs/boot3-to-boot4.md` / `docs/zh-CN/boot3-to-boot4.md`。
- CHANGELOG 双语条目记录坐标重命名。
- openspec 活动 specs 同步（经本变更 delta，归档时落主 spec）：`starter-packaging`、`module-build-foundation`、`starter-documentation`、`dual-boot-compatibility-matrix` 四个能力中逐字引用旧坐标的需求/场景文本。
- 不改动历史留档：`openspec/changes/archive/**` 与 `docs/internal/**`（过程记录按原样保留）。

## Capabilities

### New Capabilities

- 无。

### Modified Capabilities

- `starter-packaging`：「starter 聚合坐标」需求的坐标名与 Purpose 措辞改为 `meili-orm-spring-boot-starter`，聚合语义（纯聚合 pom、单依赖引全栈）不变。
- `module-build-foundation`：「多模块聚合结构」需求模块清单及 `dependency:tree -pl` 场景中的模块名更新。
- `starter-documentation`：「README 徽章与未发布真相」场景中以本地仓库解析 starter 坐标的坐标名更新。
- `dual-boot-compatibility-matrix`：「双代矩阵模块」需求中两 IT 模块所依赖的 starter 坐标名更新。

## Impact

- 构建：root `pom.xml`（`<modules>`）、starter 模块 `pom.xml`、6 个 it/examples 模块 pom；目录 `spring-boot-starter-meili-orm/` → `meili-orm-spring-boot-starter/`（git mv）。
- 源码：`meili-orm-repository` 一处异常消息文案（英文，随消息属 src/main 用户可见文本）。
- 文档：README/CONTRIBUTING/boot3-to-boot4 双语六份 + CHANGELOG 双语两份。
- 规格：4 个活动 spec 的坐标引用（delta 覆盖）。
- 验证：根 reactor `mvn -s /home/lam/repo/settings.xml clean verify`（含双代 IT 矩阵，需本地 Docker——已具备）；旧坐标不再出现在任何活动文件（历史留档除外）。
- 发布面不变：仍为 core、autoconfigure、starter、jackson3、repository 五个产品模块，starter 聚合内容与 enforcer 护栏不受影响。
