# 新机器同步指南（Pro / Mini）

> 本文档告诉你在新机器上如何完整建立零件库工作环境。

---

## 第一步：git 同步（Lib3/4）

```bash
git clone https://github.com/kaigeliao2018/vex-iq-parts-lib.git
```

已经 clone 过的：

```bash
cd ~/vex-iq-parts-lib
git pull
```

---

## 第二步：U 盘拷贝（Lib1/2）

从 Windows 机器用 U 盘拷贝以下两个文件夹，放到对应目录：

| 拷贝内容 | 来源（Windows）| 目标（本机）|
|---------|--------------|------------|
| `Lib1_Official_Reference/` | `C:\Users\liao_\vex-iq-build-assistant\LDCad项目\Lib1_Official_Reference\` | `~/vex-iq-build-assistant/LDCad项目/Lib1_Official_Reference/` |
| `Lib2_Accumulated_Legacy/` | `C:\Users\liao_\vex-iq-build-assistant\LDCad项目\Lib2_Accumulated_Legacy\` | `~/vex-iq-build-assistant/LDCad项目/Lib2_Accumulated_Legacy/` |

> ⚠️ 如果目标路径已有旧版本，直接覆盖。

**Lib1 包含**：453 个官方 .step 零件（449 官方包 + 4 补充件，只读）
**Lib2 包含**：2363 个 .dat 文件总计（主件 1245 + s/子件 494 + p/图元 624，只读）

---

## 第三步：确认完成

同步后用以下命令核验数量（在 Mac 终端执行）：

```bash
# Lib1 核验（应得 453）
find ~/vex-iq-build-assistant/LDCad项目/Lib1_Official_Reference/ -name "*.step" | wc -l

# Lib2 核验（应得 2363）
find ~/vex-iq-build-assistant/LDCad项目/Lib2_Accumulated_Legacy/ -name "*.dat" | wc -l
```

目录结构：
```
~/vex-iq-parts-lib/
├── L3-sandbox/
│   ├── passed/     ← Air_Tank（已验证）
│   └── in-progress/ ← 气动件剩余6个
└── L4-release/

~/vex-iq-build-assistant/LDCad项目/
├── Lib1_Official_Reference/   ← 453个.step ✅
├── Lib2_Accumulated_Legacy/   ← 2363个.dat ✅
├── Lib3_Experimental_Field/   ← 已迁移到 vex-iq-parts-lib，忽略
├── Lib4_Official_Release/     ← 已迁移到 vex-iq-parts-lib，忽略
└── Lib5_Disaster_Recovery/    ← 暂无内容，忽略
```

---

## 日常工作流

```
Windows LDCad 整理零件
  └─► 放入 vex-iq-parts-lib/L3-sandbox/in-progress/
        └─► 验证通过后移至 passed/
              └─► git push
                    └─► 其他机器 git pull
```

**Lib1/2 是只读根源，拷贝一次后永远不用动。**
