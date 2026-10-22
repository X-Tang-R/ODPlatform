# ODPlatform

> 目标检测开发平台 —— 把「数据接入 → 训练 → 评估 → 推理」这条工程链路一次性沉淀、长期复用。

**算法是可替换的变量，工程链路是稳定的常量。**
本项目当前处于 **V1.0 地基阶段**：Monorepo 骨架 + 公共基础层 + 项目初始化/重置命令。

## 目录结构

```
ODPlatform/
├── .odp-workspace          ← 根标记：代码自任意位置向上查找，遇到它即为根
├── pyproject.toml          ← 仓库根：只放全仓共享的工具配置(ruff/mypy/pytest)；它不是包
│
├── apps/                   ← 把"端"归类，为未来的端预留
│   ├── platform/           ← 今天唯一的端：训练 + 推理引擎
│   │   ├── pyproject.toml  ← 该端自身的依赖 + 命令入口
│   │   ├── src/od_platform/
│   │   │   ├── common/     ← 公共基础层：各模块只向下依赖它
│   │   │   └── cli/        ← 入口层：站在依赖最上层
│   │   ├── tests/          ← 该端的测试
│   │   ├── configs/        ← 端私有：描述"这个端怎么跑"
│   │   └── logging/        ← 端私有：该端运行时日志
│   ├── web-backend/        ← 占位：今天空置，到来时即为 platform 的同级
│   └── desktop/            ← 占位：同上
│
├── data/                   ← 共享数据
│   ├── raw/                ← 原始数据：只读，绝不就地修改
│   └── processed/          ← 派生数据：含固定 train/val/test 划分
├── models/                 ← 共享权重(pretrained=输入 / trained=产物)
├── runs/                   ← 每一次训练运行的完整现场
├── scripts/                ← 开发期入口(装包前用 sys.path 临时桥跑 CLI)
├── tests/                  ← 占位：跨端(E2E)测试的家
└── docs/                   ← 项目文档：设计、ADR、使用说明
```

## 环境

Python ≥ 3.10（推荐 3.12），项目内独立虚拟环境：

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -e ".\apps\platform[dev]"
```

无独立显卡时 torch 走 CPU 轮子：

```powershell
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
```

## 常用命令

```bash
odp-init                                  # 初始化：建目录 + 环境快照 + 数据集状态检查
python -m od_platform.cli.init_project    # 同上，模块路径写法
python scripts/init_project.py            # 装包前的开发期过渡入口
```

## 开发流程

```bash
ruff format . && ruff check . --fix       # 格式化 + 静态检查
mypy .                                    # 类型检查
pytest                                    # 单元测试
```

## 相关文档

- [[D0-设计说明（精简版）]] —— 七个设计决定与目录树由来
- [[D1-项目初始化（精简实操版）]] —— 从 `git init` 到可安装包的完整实操
- [[D2-项目重置（精简实操版）]] —— 危险删除操作的安全设计
- [[工程目录创建Demo]] —— pip / hatchling / ruff / pytest / mypy / pre-commit / CI 的最小可运行演示
