# 当前任务交接（2026-04-19）

## 今日完成
- ✅ 发现4件"来源不明"实为生产环境 `ling_jian_ku/VEX_IQ_te_shu/parts/`（共31件）
- ✅ 决策：将整个 VEX_IQ_te_shu 目录纳入原子化流程
- ✅ A类11件原子化完成，写入 Lib3 测试库
  - VEXbeams：1240/1245/1485/1110（4件特形梁）
  - VEXpins：1658/1109/1660
  - VEXgear：1413/1303
  - VEXmisc：421
  - VEXpanels：524
- ✅ pbg 规则踩坑修复：NOMATCH 导致所有类目消失，已恢复原始规则
- ✅ VEXbeams 规则改为 `include description magikid` + `exclude !description beam`（防止非梁件混入）
- ✅ 7件非梁件描述行补加 VGR 标记（-Vgr3-/-Vgr6-/-Vgr8-/-Vgr9-）使其显示在正确类目

## 遗留问题（下次上线验证）

### 1. LDCad 重启验证（凯戈操作）
- **Beams & Plates**：应只有42件（38直梁+4特形梁），不出现其他件
- **Panels & Special Beams**：应出现 2x8 Smooth Panel
- **Pins & Standoffs**：Crane Hook / Truss Connector / Ball Pin Bushing 应有新名
- **Gears & Motion**：应出现 Idler Pulley / Cam Follower
- **Miscellaneous**：2x Spool 应有新名

### 2. VEX_IQ_te_shu B/C类件（待处理）
- **B类（描述行只有SKU，6件）**：1381/1704/1705/2161/2248/178 — 需开文件确认几何
- **C类（描述行空，7件）**：1407/1727/1728/1548/1953/1958/1411 — 需逐一检查
- **D类（跳过）**：1702/4x12薄片/shou_bing_0.2.ldr/276-prefix件/中文名件

### 3. 特形梁（beams_baseline.md 中29件）
- 来源：Lib2 或 te_shu（需确认），原来定为"下阶段"
- 注意：te_shu 里已有部分特形梁（1110/1485 已处理）

## pbg 规则说明（踩坑记录）
- LDCad 的 `<items>` 列表不是独立于规则的——规则不匹配则 items 也不显示
- VEXbeams 用 `include description magikid` + `exclude !description beam`（双重过滤）
- 其他 pbg 用原始 VGR 规则：`include description vex` + `exclude !description -vgrN-`
- 新件要进哪个类目，描述行里必须带对应 VGR 标记

## 关键路径
- te_shu 来源：`C:\LDCad-1-7-Alpha-2a-Win-IQ\ling_jian_ku\VEX_IQ_te_shu\parts\`
- 测试库：`C:\LDCad-1-7-Alpha-2a-Win-IQ - 副本\vex-iq-parts-v2\parts\`
- 基准清单：`~/vex-iq-parts-lib/plans/beams_baseline.md`
- pbg 位置：`C:\LDCad-1-7-Alpha-2a-Win-IQ - 副本\partBin\default\sorted\`
