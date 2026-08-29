# Blender 学习笔记（二）：导入 PDB 格式的小分子

> 用 Atomic Blender 插件将分子结构导入 Blender 制作科研三维示意图
> 以组氨酸残基为例，从 PyMOL 导出 → RDKit 核对 → 修改双键 → Blender 导入

**来源**：https://mp.weixin.qq.com/s/jd48A7WFG5VRfjhW9sYMtg
**日期**：2026-08-29

---

## 概述

在 Blender 中制作小分子、氨基酸残基或蛋白结构的三维示意图，可以通过 PDB 文件导入。本文以组氨酸残基为例，介绍完整操作流程。

---

## 一、在 PyMOL 中准备小分子结构

以组氨酸为例，在 PyMOL 命令行中输入：

```
fragment His                    # 生成一个组氨酸示例片段
set pdb_conect_all, on          # 在导出的PDB文件中写入原子连接信息
```

完成后，将结构导出为 PDB 文件。

---

## 二、用 RDKit 核对原子编号

PDB 文件使用数字标识每个原子。修改化学键之前，必须先确认每个编号对应哪个原子。

使用 RDKit 生成一张带原子编号的二维结构图，然后对照 PDB 文件检查：
- 哪些原子彼此连接
- 哪些位置是单键
- 哪些位置应该是双键
- PDB 文件中的编号是否与结构图一致

> ⚠️ 编号对应错误会导致 Blender 中出现错误的化学键

---

## 三、在 PDB 文件中记录双键

### 1. 找到 CONECT 记录

用纯文本编辑器打开 PDB 文件，在文件末尾附近找到以 `CONECT` 开头的记录。

例如：
```
CONECT  3  2  4
```
- 第一个数字 `3` 是中心原子编号
- 后面的 `2` 和 `4` 表示原子 3 分别与原子 2、原子 4 相连
- 普通连接通常只记录一次，导入后一般显示为单键

### 2. 用重复编号表示双键

如果原子 3 和原子 4 之间应该是双键，重复写入原子 4：
```
CONECT  3  2  4  4
```

为完整性，在另一端也写入对应记录：
```
CONECT  4  3  3
```

### 3. 修改注意事项
- 双键两端最好都记录重复编号
- 不要随意改变原有字段的排列和间距
- 修改后如 Blender 无法正确读取，首先检查原子编号和字段格式

---

## 四、在 Blender 中安装 Atomic Blender

1. 打开 Blender：`Edit > Preferences > Get Extensions`
2. 搜索：`Atomic Blender`
3. 安装：`Atomic Blender PDB/XYZ`
4. 确认插件已启用
5. 在插件设置中勾选 `Open panel`
6. 启用后，3D Viewport 右侧栏的 `Create` 标签中出现 `Atomic Blender Utilities`
   - 如右侧栏未显示，鼠标放在 3D Viewport 中按 `N` 打开

---

## 五、将 PDB 文件导入 Blender

1. `File > Import` 选择 PDB 导入选项
2. 选中修改后的 PDB 文件
3. 导入设置：
   - `Sector`：控制圆柱形化学键的分段数，数值越大化学键越光滑
   - 勾选 `bonds` 以显示键

---

## 六、调整原子和化学键

打开 `Create > Atomic Blender Utilities`：

- 先选中相应的原子/原子类型/化学键，即可调整：
  - 原子大小
  - 缩放比例
  - 化学键粗细
- 只调整某一个原子：直接选中后用坐标轴手柄移动
- Blender 基础快捷键：
  - `G`：移动
  - `R`：旋转
  - `S`：缩放

> 详细操作见 Blender 学习笔记（一）：从看懂界面到完成第一次渲染

---

## 七、整体调整整个分子

Atomic Blender 导入的分子由多个原子和化学键对象组成。直接全选可能同时选中场景中的相机和灯光。

### 1. 显示选择限制列
- 打开 Outliner 右上角筛选菜单
- 在 Restriction Toggles 中显示"可选择"限制列

### 2. 禁止选择相机和灯光
- 在 Camera 和 Light 对应行中，关闭可选择状态
- 相机和灯光仍存在，但全选时不会被选中

### 3. 选择整个分子
- 鼠标放在 3D Viewport 中，按 `A` 全选分子
- 用 `G`/`R`/`S` 移动/旋转/缩放

---

## 流程总结

```
PyMOL 生成片段 → 导出 PDB
        ↓
RDKit 生成原子编号二维图 → 核对编号
        ↓
文本编辑器修改 CONECT 记录 → 标记双键
        ↓
Blender 安装 Atomic Blender 插件
        ↓
File > Import PDB → 调整原子和化学键
        ↓
Outliner 排除相机/灯光 → 全选分子 → 整体调整
```

---

*参考：Blender 科研绘图系列教程*
