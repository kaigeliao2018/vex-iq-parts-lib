# vex-iq-parts-lib — Claude 项目规则

## 项目身份
VEX IQ 零件库管理系统，配套 vex-iq-build-assistant。
采用五层架构管理零件数据。主要工作机器：**Mini**（Gemini 负责执行）。

## 五层架构（铁律）
| 层级 | 名称 | 规则 |
|------|------|------|
| Lib1 | Official_Reference | 只读，官方原始文件 |
| Lib2 | Accumulated_Legacy | 只读，长期稳定运行的基石库 |
| Lib3 | Experimental_Field | **主战场**，所有改动在此 |
| Lib4 | Official_Release | 通过验收后才移入 |
| Lib5 | Disaster_Recovery | 定期备份 Lib4 |

## 零件原子化验收标准
- 命名：`SKU_Official_English_Name.dat`
- 子组件：分两类，统一放共享 `s/` 目录（跟原版 LDraw 结构一致）
  - **零件专属子件**：命名 `SKU + s + 序号.dat`（如 228-7721-000s1.dat）
  - **公共共享子件**：直接从原版库复制（如 vexpin1s.dat 及其依赖），不改名不修改
- 路径：内部引用行 `\` 全部改为 `/`
- 色号：灰色零件统一 **71**
- **铁律：公共子件跟原版走，不造轮子，不轻易改动，一处改动影响全部引用它的零件**

## 上线必读：技术手册

进入此项目工作前，必须读取以下技术手册：

- LDCad Shadow/Snap 机制：`~/kaige-brain/tech-notes/ldcad-shadow-snap-mechanism.md`

> 手册记录了踩过的坑和调查结论，遇到吸附/snap/shadow问题先查手册，不要重复调查。

## 当前进度
气动类目（Pneumatics）已完成，梁类目（Beams）整理中 — 见 `~/vex-iq-parts-lib/plans/architecture_parts_library_v1.md`

## 禁止
- 修改 Lib1 / Lib2 任何文件
- 跳过 App 预览验收直接移入 Lib4
- 测试环境引用生产环境路径（铁律：不跨环境）
