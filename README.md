# vex-iq-parts-lib

**VEX IQ 零件库管理系统** — L3 试验田 + L4 发布库

配套项目：[vex-iq-build-assistant](https://github.com/kaigeliao2018/vex-iq-build-assistant)  
系统设计：见 `vex-iq-build-assistant/docs/PARTS-LIB-MANAGEMENT.md`

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
│   ├── v1.0/            ← 版本快照（只读）
│   ├── parts-manifest.json
│   └── CHANGELOG.md
└── shared/
    └── parts-metadata.json   ← SKU → 零件类型映射（source of truth）
```

## 不在此仓库的内容

- **L1 官方 STEP 文件**：本地存储，路径待确认
- **L2 历史 .dat 库**：本地存储，只读冻结，路径待确认
- **L5 灾难恢复备份**：本地 tar.gz，不入 git

## 操作指南

详见 `vex-iq-build-assistant/docs/PARTS-LIB-OPERATIONS.md`
