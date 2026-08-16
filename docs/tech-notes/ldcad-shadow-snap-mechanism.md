# LDCad Shadow 文件与吸附机制 技术手册

> 建立：2026-04-19
> 背景：梁样本验证时发现新命名件无吸附，经深度调查后整理

---

## 一、什么是 Shadow 文件

Shadow 文件是 LDCad 的**非侵入式 snap 注入机制**。
- 不修改原始 `.dat` 文件
- 通过同名文件"覆盖"零件的 snap（吸附）定义
- 零件文件内容不变，snap 行为可独立配置

---

## 二、文件格式

Shadow 文件是**纯文本**（不是二进制），后缀 `.dat`，内容为 LDCad meta 指令：

```
0 LDCad snapinfo for VEX Pin  1.5 M
0 Author: Philippe Hurbain

0 !LDCAD SNAP_CYL [gender=M] [caps=none] [secs=R 8 2.2 R 5.3 11.6 _L 5.6 4.4 R 5.3 11.6 _L 5.8 1.6] [pos=0 0 0] [ori=1 0 0 0 0 -1 0 1 0]
```

常见指令：
- `!LDCAD SNAP_CYL` — 圆柱形 snap（销/孔对接）
- `!LDCAD SNAP_GEN` — 通用 snap（异形连接）
- `gender=M` — 公头（Pin）；`gender=F` — 母头（Hole）

---

## 三、目录结构

```
shadow/
  offLib/
    offLibShadow.csl            ← ZIP 包，存标准 LDraw 图元的 snap（3675 条目）
    offLibShadow_VEX_IQ/
      parts/
        228-2500-061.dat        ← VEX Pin 1.5 M 的 snap 定义
        228-2500-062.dat
        ...（65 个文件，均为 Pin/连接件类）
        vexhconin.dat
        vexhconout.dat
        vexrj.dat
        vexrjsocket.dat
      p/
        stud.dat
    offLibShadow_VEX_EDR/
      ...
```

---

## 四、生效条件（核心）

Shadow 是否生效，**完全取决于 `main.cfg` 中库的配置**：

```
# 有 shadow → snap 生效
off-><库路径>\-><shadow目录路径>\

# 无 shadow → snap 全部失效
off-><库路径>\->none
```

**结论：`->none` 会让该库所有零件的 snap 失效，包括 Pin。**

---

## 五、VEX IQ 吸附机制拆解

### 5.1 Pin 的吸附（已确认）

- 来源：`offLibShadow_VEX_IQ/parts/228-2500-061.dat`（等 Pin 类 shadow）
- 内容：`SNAP_CYL gender=M`（公头圆柱）
- 条件：Pin 所在库必须配置了指向该 shadow 目录的路径

### 5.2 Beam Pinhole 的吸附（2026-04-19 调查结论）

- `vexpinhole.dat` 本身：纯几何（`4-4cylo.dat` + `4-4ring2.dat`），无 SNAP 指令
- `offLibShadow_VEX_IQ` 里：没有 `vexpinhole.dat` shadow
- `offLibShadow.csl`（全局）里：有 `p/beamhole.dat`（Technic 梁孔，非 VEX 专属）
- **待验证**：配置 shadow 路径后，梁的 pinhole 能否与 Pin 正常吸附

### 5.3 旧件 vs 新件吸附差异

| | 旧件（Lib2 VEX_IQ） | 新件（vex-iq-parts-v2） |
|---|---|---|
| 库配置 | `->offLibShadow_VEX_IQ\` | `->none`（修复前） |
| Pin snap | ✅ 生效 | ❌ 失效 |
| 文件内容 | 相同子件（vexpinhole等） | 相同子件 |

**原因不在文件内容，在于库的 shadow 配置。**

---

## 六、修复操作（2026-04-19 执行）

修改测试环境 `config/main.cfg`：

```
# 修复前
off->C:\LDCad-1-7-Alpha-2a-Win-IQ - 副本\vex-iq-parts-v2\->none

# 修复后
off->C:\LDCad-1-7-Alpha-2a-Win-IQ - 副本\vex-iq-parts-v2\->C:\LDCad-1-7-Alpha-2a-Win-IQ - 副本\shadow\offLib\offLibShadow_VEX_IQ\
```

**铁律：测试环境所有路径必须在 `副本` 目录内，严禁引用生产环境路径。**

---

## 七、如果未来需要为新零件创建 Shadow

当整理新类目（如梁、齿轮）时，如果发现某类零件缺少 snap：

1. 先确认该零件用了哪些子件（看 .dat 文件的 `1 <color> ... s/xxx.dat` 行）
2. 在 shadow 目录里查找对应子件是否有 shadow
3. 如果没有，参考同类零件的 shadow 格式，新建 shadow 文件
4. shadow 文件命名必须与子件文件名完全一致
5. 放入测试环境自己的 shadow 目录，验证通过后再同步思路到生产

---

## 八、待验证事项

- [ ] 修复 shadow 配置后，梁与 Pin 的吸附是否正常（需在测试 LDCad 中实测）
- [ ] 若 pinhole 仍无 snap，是否需要为 `vexpinhole.dat` 单独创建 shadow 文件
- [ ] 整理其他类目零件时，是否存在同类问题

---

## 九、关键命令速查

```bash
# 查看 shadow 目录文件数
ls "C:/LDCad-1-7-Alpha-2a-Win-IQ - 副本/shadow/offLib/offLibShadow_VEX_IQ/parts/" | wc -l

# 查看某 shadow 文件内容
python -c "
with open('shadow文件路径', 'rb') as f:
    print(f.read().decode('utf-8', errors='replace'))
"

# 查看 offLibShadow.csl 内条目
python -c "
import zipfile
with zipfile.ZipFile('offLibShadow.csl路径') as z:
    for n in z.namelist(): print(n)
"

# 验证库配置
grep "LDrawPaths" -A 20 "main.cfg路径"
```

---

## 十、LDCad pbg 过滤机制（2026-04-19 调查）

### pbg rules 只支持 description 字段

经实测，以下语法**无效**：
- `include name _`（按文件名过滤）
- `include keywords magikid`（按关键字过滤）

**唯一有效的过滤语法**：
```
include description <关键词>
exclude !description <关键词>
```

### kind=filter 的显示逻辑

- rules 是内容驱动引擎，**rules 为空 = 显示为空**，items 不会独立显示
- items 是显示顺序/置顶列表，依附于 rules 结果
- pbg 文件必须使用 **CRLF 换行**，LF 格式 LDCad 无法正确读取，会清空文件内容

### Magikid 标准零件库过滤方案

在每个新零件描述行末尾加 `Magikid`：
```
0 VEX Beam  1 x  2 -Vgr2- Magikid
```

pbg rules 配置：
```
include description magikid
```

**效果**：精准过滤 Magikid 新件，Lib2 旧件完全不显示，零重复。

### pbg 文件写入规范（重要）

禁止用 Claude Write 工具直接写 pbg 文件（输出 LF，LDCad 会清空）。
必须用 Python 以 `wb` 模式写入 CRLF：

```python
python -c "
path = 'C:/LDCad.../VEXbeams.pbg'
content = '[options]\r\nkind=filter\r\n...\r\n<rules>\r\ninclude description magikid\r\n\r\n\r\n<items>\r\n零件名.dat\r\n'
with open(path, 'wb') as f:
    f.write(content.encode('utf-8'))
"
```
