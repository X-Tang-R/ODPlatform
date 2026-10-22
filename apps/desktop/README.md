# apps/desktop (占位)

> 🚧 **状态**: 占位,V1.1 启动开发

本子项目将提供 ODPlatform 的桌面端应用(本地 GUI / 打包为桌面程序)。

## 为什么现在是空的?

这一目录的存在是 **架构层面的预约**——
今天就给它留好位置,这样 V1.1 启动时:

- desktop 团队知道往哪建
- platform 一行代码不用动
- 共享数据(`/data`、`/models`)直接复用

## 与 platform 的关系

复用 `apps/platform/` 的核心引擎(数据转换、推理)—— desktop 和 platform
装在同一个环境里(platform 已 `pip install -e`),`import od_platform` 直接能用,
不涉及打包发布。

参考: `docs/architecture/ADR-001-monorepo.md`
