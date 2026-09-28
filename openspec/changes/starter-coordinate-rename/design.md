# Design

## Context

见 proposal.md - Why。当前状态：starter 模块目录、artifactId 与 `<name>` 均为 `spring-boot-starter-meili-orm`，全仓活动文件中共 18 处逐字引用（root pom `<modules>`、starter 自身 pom、3 个 it 与 3 个 examples pom、双语 README/CONTRIBUTING/boot3-to-boot4、4 个活动 spec、`MeiliRepositoryFactoryBean` 异常消息一处）；历史留档（`openspec/changes/archive/**`、`docs/internal/**`，含定稿该名的 D2 决策）另有若干处。构件未发布，版本号仍为 `1.0-SNAPSHOT`。项目以线性 master 交付，无分支合并流程。

## Goals / Non-Goals

**Goals:**
- 坐标、目录、`<name>`、全部活动引用一次性原子切换为新名，旧名在仓内仅剩历史留档。
- 构建与双代 IT 矩阵在改名后全绿（enforcer 护栏、license/javadoc/引用门禁照常生效）。

**Non-Goals:**
- 不改 groupId、version、包根与 `meili.*` 配置前缀。
- 不改 starter 的聚合依赖内容与自动配置装配语义。
- 不为旧坐标保留 alias/deprecated 兼容构件（发布前无下游，兼容层是纯负担）。

## Decisions

1. **新名取 `meili-orm-spring-boot-starter`**：官方约定格式 `<名称>-spring-boot-starter`（MyBatis `mybatis-spring-boot-starter` 同型），且与家族词干 `meili-orm-*` 及既有约定名 `meili-orm-spring-boot-autoconfigure` 完全对齐。备选：`meili-spring-boot-starter`（丢 ORM 词干，与仓库名/其他坐标不一致）、`meili-orm-boot-starter`（不符合约定格式）——均弃。
2. **目录随 artifactId 重命名（`git mv`）**：Maven 惯例目录名=artifactId，root pom `<modules>` 以目录名引用；`git mv` 保留文件历史。备选（目录留旧名只改 artifactId）会造成目录名与坐标长期背离，弃。
3. **历史留档不改**：`openspec/changes/archive/**` 与 `docs/internal/**` 是带日期的过程记录，改名它们等于篡改历史；新命名决策由本变更的 delta spec 承载。D2 的旧结论作为历史保留，不追改。
4. **主 spec 的 Purpose 文案直改**：`starter-packaging` 主 spec 的 Purpose 行含旧坐标名；openspec delta 只同步 Requirements，Purpose 须在 apply 时直接编辑主 spec（此为本变更有意选择的例外通道，归档时不会冲突——Requirement 块由 delta 覆盖、Purpose 保持手改结果）。
5. **验收判定用 grep 白名单法**：活动文件集合（排除 `openspec/changes/archive/**`、`docs/internal/**`、`target/`、本变更自身目录）内 `grep -rn 'spring-boot-starter-meili-orm'` 零命中即达标，避免逐文件人工清点遗漏。

## Risks / Trade-offs

- [本地 Maven 仓库残留旧坐标 `1.0-SNAPSHOT` 构件，验证时可能误解析到陈旧产物] → 改名后先 `mvn clean install` 重装全量 reactor 再跑验证；`dependency:tree` 场景断言新坐标出现。
- [全量 `clean verify` 依赖 Docker 与 Meilisearch v1.49.0 镜像（IT 矩阵起真容器）] → 本机 Docker 已启用；若镜像缺失先 `docker pull`，验证失败不得以 skip 规避。
- [README 双语镜像与 docs/boot3-to-boot4 双语共 6 份文档，漏改破坏"中文镜像逐段对应"约定] → 用同一替换清单批量处理后 diff 复核，grep 验收兜底。
- [一次性原子切换中途构建不可用] → 单 commit 落地（改名+全量替换+验收同提交），失败即 revert，不存在半改状态入库。

## Migration Plan

1. `git mv` 目录 + 改 starter pom（artifactId/`<name>`）+ root pom `<modules>`。
2. 替换 6 个 it/examples pom、异常消息文案、6 份双语文档、2 份 CHANGELOG、主 spec Purpose。
3. `mvn -s /home/lam/repo/settings.xml clean install` 重装本地构件 → 根 reactor `clean verify`（含双代 IT）+ `dependency:tree -pl meili-orm-spring-boot-starter` 与 `check-source-citations.sh` 抽查。
4. grep 白名单法验收零残留 → 单 commit 线性提交推送 master。回滚：revert 该 commit（历史留档未动，回滚无残留态）。

## Open Questions

- 无。
