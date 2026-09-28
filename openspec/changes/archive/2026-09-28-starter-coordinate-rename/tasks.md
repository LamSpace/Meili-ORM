# Tasks

## 1. 坐标与目录切换

- [x] 1.1 `git mv spring-boot-starter-meili-orm meili-orm-spring-boot-starter` 保留文件历史 → 验证：目录列表新名存在、旧名消失，`git status` 呈现 rename
- [x] 1.2 改 starter 模块自身 pom 的 `<artifactId>` 与 `<name>` 为 `meili-orm-spring-boot-starter`，同步 root `pom.xml` 的 `<modules>` 条目 → 验证：`mvn -s /home/lam/repo/settings.xml validate` 全反应堆通过

## 2. 仓内引用同步

- [x] 2.1 更新 `it/meili-orm-it-boot3`、`it/meili-orm-it-boot4`、`it/meili-orm-it-boot4-jackson3`、`examples/meili-orm-example-common`、`examples/meili-orm-example-boot3`、`examples/meili-orm-example-boot4` 六处 starter 依赖坐标 → 验证：`mvn -s /home/lam/repo/settings.xml dependency:tree -pl <各模块>` 输出仅含新坐标
- [x] 2.2 更新 `meili-orm-repository` 的 `MeiliRepositoryFactoryBean` 异常消息中提示的 classpath 坐标名（英文文案，Javadoc 与行为保持一致） → 验证：该文件 `grep spring-boot-starter-meili-orm` 零命中，`mvn -s /home/lam/repo/settings.xml -pl meili-orm-repository -am test` 编译与单测通过
- [x] 2.3 更新双语文档六份：`README.md`/`README.zh-CN.md`（安装坐标片段与目录树）、`CONTRIBUTING.md`/`CONTRIBUTING.zh-CN.md`、`docs/boot3-to-boot4.md`/`docs/zh-CN/boot3-to-boot4.md` → 验证：六文件内旧坐标 `grep` 零命中，且中英镜像对应段落坐标逐字一致

## 3. 规格与发布元数据

- [x] 3.1 直改主 spec `openspec/specs/starter-packaging/spec.md` 的 Purpose 行坐标名（delta 只同步 Requirements，Purpose 须手工） → 验证：主 spec 文件 `grep` 零命中
- [x] 3.2 `CHANGELOG.md` 与 `CHANGELOG.zh-CN.md` 未发布段追加坐标重命名条目（旧名 → 新名 + 命名约定依据） → 验证：双语条目对应存在

## 4. 集成验证与交付

- [x] 4.1 全量重装并跑完整测试流程：`mvn -s /home/lam/repo/settings.xml clean install`（Docker 已启用；双代 IT 矩阵 + jackson3 IT + examples 冒烟全部真实执行，不得 skip） → 验证：BUILD SUCCESS，无测试失败
- [x] 4.2 零残留验收：活动文件范围（排除 `openspec/changes/archive/**`、`docs/internal/**`、`target/`、本变更目录、CHANGELOG 双语更名条目）内 `grep -rn "spring-boot-starter-meili-orm"` → 验证：零命中（实测：仅 CHANGELOG 记录更名事实的两处，口径调整后达成）
- [x] 4.3 门禁复跑：`bash scripts/check-source-citations.sh` → 验证：退出码 0；license 与 javadoc 门禁已由 4.1 构建覆盖
- [x] 4.4 单 commit 线性提交并推送 master（项目交付惯例：不建分支不开 PR） → 验证：`git show --stat` 仅含本次改名相关文件
