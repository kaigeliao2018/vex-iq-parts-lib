# vex-iq-parts-lib

**VEX IQ 零件库管理系统** — L3 试验田 + L4 发布库

配套项目：[vex-iq-build-assistant](https://github.com/kaigeliao2018/vex-iq-build-assistant)

---

## Windows 快速启动（首次设置）

> 此仓库是 Windows（LDCad 整理）→ Mac（搭建助手读取）的数据桥梁。

### Clone 仓库

```bash
cd C:\Projects
git clone https://github.com/kaigeliao2018/vex-iq-parts-lib.git
```

### Windows 工作流

```
LDCad 整理零件（.dat 文件）
  └─► 放入 L3-sandbox/in-progress/
        └─► 验证通过后移至 L3-sandbox/passed/
              └─► 升入 L4-release/parts/{子库}/
                    └─► git push → Mac git pull → 搭建助手读取
```

**L3 子库对应关系：**

| L3 目录 | 对应 ling_jian_ku 子库 |
|---------|----------------------|
| `L4-release/parts/VEX_IQ/` | `VEX_IQ/` |
| `L4-release/parts/VEX_IQ_2_dai/` | `VEX_IQ_2_dai/` |
| `L4-release/parts/VEX_IQ_qi_dong/` | `VEX_IQ_Pneumatics/` |
| `L4-release/parts/VEX_IQ_dao_ju/` | `VEX_IQ_dao_ju/` |
| `L4-release/parts/VEX_IQ_te_shu_slim/` | `VEX_IQ_te_shu/`（精简版）|

---

## 仓库结构

```
vex-iq-parts-lib/
├── L3-sandbox/          ← 标准化工作区（试验田）
│   ├── in-progress/     ← 处理中的零件 + 工作日志
│   ├── passed/          ← 验证通过，等待升入 L4
│   └── failed/          ← 验证失败，记录原因
├── L4-release/          ← 生产版本库（版本控制）
│   ├── parts/           ← 当前版本零件文件
│   │   ├── VEX_IQ/
│   │   ├── VEX_IQ_2_dai/
│   │   ├── VEX_IQ_te_shu_slim/
│   │   ├── VEX_IQ_dao_ju/
│   │   └── VEX_IQ_qi_dong/
│   ├── parts-manifest.json
│   └── CHANGELOG.md
└── shared/
    └── parts-metadata.json   ← SKU → 零件类型映射（source of truth）
```

## 不在此仓库的内容

- **L1 官方 STEP 文件**：本地存储，不入 git（体积过大）
- **L2 历史 .dat 库**（`ling_jian_ku`）：本地存储，只读冻结，通过 U 盘同步
- **L5 灾难恢复备份**：本地 tar.gz，不入 git
