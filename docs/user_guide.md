# RuleDoc 用户使用指南

本指南详细介绍如何使用 RuleDoc 格式化您的学术文档。

## 目录

- [快速体验](#快速体验)
- [安装与配置](#安装与配置)
- [基本概念](#基本概念)
- [命令行使用](#命令行使用)
- [输入文件准备](#输入文件准备)
- [常见问题](#常见问题)
- [故障排除](#故障排除)

## 快速体验

使用项目自带的 `test_script.md` 快速体验 RuleDoc：

```bash
# 1. 克隆仓库
git clone https://github.com/xiaoyu-778/ruledoc.git
cd ruledoc

# 2. 安装依赖
pip install -e .

# 3. 运行测试（使用 test_script.md）
ruledoc test_script.md --rule yzu_thesis
```

运行后会：
1. 加载 `yzu_thesis` 规则（扬州大学学位论文格式）
2. 生成 `test_script_formatted.docx`

**test_script.md 包含以下内容：**
- 中英文摘要
- 六级标题层级测试
- 正文段落与列表
- 图表题注（图 4-1、表 4-1）
- 数学公式
- 代码块
- 参考文献（GB/T 7714 格式）

## 安装与配置

### 系统要求

- **Python**: 3.8 或更高版本
- **操作系统**: Windows 10/11、macOS 10.15+、Linux
- **内存**: 建议 4GB 以上
- **磁盘空间**: 100MB 可用空间

### 安装步骤

#### 1. 安装 Python

如果尚未安装 Python，请从 [python.org](https://python.org) 下载并安装 3.8 或更高版本。

验证安装：
```bash
python --version
```

#### 2. 安装 RuleDoc

```bash
pip install ruledoc
```

#### 3. 验证安装

```bash
ruledoc --version
```

预期输出：
```
ruledoc 1.0.0
```

### 可选依赖

如需 Markdown 转换功能，建议安装 Pandoc：

- **Windows**: `winget install pandoc`
- **macOS**: `brew install pandoc`
- **Linux**: `sudo apt-get install pandoc`

## 基本概念

### ⚠️ 必须使用规则

**RuleDoc 必须指定格式规则才能工作**，没有默认规则。规则定义了完整的文档格式规范，包括：

- **页面设置**: 页边距、纸张大小、装订线
- **字体规范**: 正文字体、标题字体、字号
- **标题格式**: 各级标题的字体、对齐方式、字号
- **段落格式**: 行距、段前段后间距
- **特殊元素**: 图表编号、参考文献格式、页眉页脚

### 规则命名

规则名称采用 `{学校缩写}_{文档类型}` 的格式：

| 规则名称 | 学校 | 文档类型 |
|---------|------|---------|
| `yzu_thesis` | 扬州大学 (YZU) | 学位论文 |
| `yzu_design` | 扬州大学 (YZU) | 毕业设计 |

### 支持的输入格式

| 格式 | 扩展名 | 说明 |
|------|--------|------|
| Markdown | `.md`, `.markdown` | 推荐，简洁易写 |
| Word | `.docx` | 可重新格式化已有文档 |

## 命令行使用

### ⚠️ 重要：必须使用规则

**RuleDoc 必须指定格式规则才能工作**，没有默认规则。您需要：

1. 查看可用规则：`ruledoc --list-rules`
2. 使用 `--rule` 参数指定规则

### 基本命令结构

```bash
ruledoc [输入文件] --rule [规则名称] [选项]
```

### 常用命令示例

#### 1. 查看可用规则

```bash
ruledoc --list-rules
```

输出示例：
```
Available rules:
  - yzu_thesis: 扬州大学学位论文格式
  - yzu_design: 扬州大学毕业设计格式
```

#### 2. 基本格式化

```bash
ruledoc thesis.md --rule yzu_thesis
```

#### 3. 指定输出文件

```bash
ruledoc thesis.md --rule yzu_thesis -o 我的论文.docx
```

#### 4. 重新格式化 Word 文档

```bash
ruledoc draft.docx --rule yzu_thesis -o final.docx
```

### 完整参数说明

| 参数 | 简写 | 必填 | 说明 | 示例 |
|------|------|------|------|------|
| `输入文件` | - | ✅ | 要格式化的文件路径 | `thesis.md` |
| `--rule` | `-r` | ✅ | 使用的格式规则 | `--rule yzu_thesis` |
| `--output` | `-o` | - | 输出文件路径 | `-o output.docx` |
| `--list-rules` | `-l` | - | 列出所有规则 | `--list-rules` |
| `--version` | `-v` | - | 显示版本 | `--version` |
| `--help` | `-h` | - | 显示帮助 | `--help` |

### 高级用法

#### 批量处理

使用 shell 脚本批量格式化多个文件：

**Windows (PowerShell):**
```powershell
Get-ChildItem *.md | ForEach-Object {
    ruledoc $_.Name --rule yzu_thesis -o "formatted_$($_.BaseName).docx"
}
```

**Linux/macOS (Bash):**
```bash
for file in *.md; do
    ruledoc "$file" --rule yzu_thesis -o "formatted_${file%.md}.docx"
done
```

## 输入文件准备

### Markdown 格式指南

#### 1. 文档结构

```markdown
# 论文标题

## 摘要

本文研究了人工智能在医疗诊断中的应用...

**关键词：** 人工智能；医疗诊断；深度学习

## Abstract

This paper studies the application of artificial intelligence...

**Keywords:** Artificial Intelligence; Medical Diagnosis; Deep Learning

## 第一章 绪论

### 1.1 研究背景

随着人工智能技术的快速发展...

### 1.2 研究意义

本研究具有重要的理论价值和实践意义...

## 第二章 相关技术

### 2.1 深度学习

深度学习是机器学习的一个分支...

#### 2.1.1 卷积神经网络

卷积神经网络（CNN）在图像处理领域...

## 第三章 实验结果

### 3.1 数据集

本实验使用了公开的医疗影像数据集...

**图 1**  数据集样本分布

![数据集样本分布](images/dataset.png)

### 3.2 实验设置

实验环境配置如 **表 1** 所示：

| 组件 | 配置 |
|------|------|
| CPU | Intel i9-10900K |
| GPU | NVIDIA RTX 3090 |
| 内存 | 64GB DDR4 |

**表 1**  实验环境配置

## 第四章 结论

本文提出了一种新的医疗诊断方法...

## 参考文献

[1] LeCun Y, Bengio Y, Hinton G. Deep learning[J]. Nature, 2015, 521(7553): 436-444.
[2] 张三, 李四. 基于深度学习的医学影像分析[J]. 计算机学报, 2023, 46(3): 512-528.
```

#### 2. 标题层级

| Markdown | 对应样式 | 用途 |
|----------|---------|------|
| `#` | 论文大标题 | 文档主标题 |
| `##` | 一级标题 | 章节标题（第一章、第二章） |
| `###` | 二级标题 | 小节标题（1.1、1.2） |
| `####` | 三级标题 | 子节标题（1.1.1） |

#### 3. 图表标注

**图片标注:**
```markdown
**图 1**  图表标题

![替代文本](图片路径)
```

**表格标注:**
```markdown
| 列1 | 列2 |
|-----|-----|
| 数据1 | 数据2 |

**表 1**  表格标题
```

#### 4. 参考文献格式

支持 GB/T 7714 标准格式：

```markdown
## 参考文献

[1] 作者. 文章标题[J]. 期刊名, 年份, 卷(期): 页码.
[2] 作者. 书名[M]. 出版地: 出版社, 年份.
[3] 作者. 论文标题[D]. 保存地: 保存单位, 年份.
```

### Word 文档准备

如果您已有 Word 文档，建议按以下方式准备：

#### 1. 应用样式

- **论文标题**: 使用"标题"样式
- **章节标题**: 使用"标题 1"样式
- **小节标题**: 使用"标题 2"样式
- **正文**: 使用"正文"样式

#### 2. 添加题注

图表使用 Word 的题注功能：

1. 选中图表
2. 点击"引用" → "插入题注"
3. 标签选择"图"或"表"
4. 自动生成编号

## 常见问题

### Q1: 为什么必须指定规则？

**A:** RuleDoc 设计为规则驱动，不同学校有不同的格式规范。没有"通用"的默认规则，必须明确指定使用哪个学校的格式。

### Q2: 支持哪些学校格式？

目前支持：
- 扬州大学学位论文格式 (`yzu_thesis`)
- 扬州大学毕业设计格式 (`yzu_design`)

如需其他学校支持，您可以：
1. 查看 [规则开发指南](rule_development.md) 自行添加
2. 在 [Issues](https://github.com/xiaoyu-778/ruledoc/issues) 中提出需求

### Q3: 可以处理 LaTeX 文件吗？

目前不直接支持 LaTeX。建议：
1. 使用 Pandoc 将 LaTeX 转换为 Markdown：`pandoc input.tex -o output.md`
2. 再使用 RuleDoc 格式化：`ruledoc output.md --rule yzu_thesis`

### Q4: 如何自定义格式？

RuleDoc 设计为规则驱动，不支持直接自定义格式参数。如需特殊格式：
1. 创建新的规则文件（参考 [规则开发指南](rule_development.md)）
2. 在格式化后手动调整 Word 文档

### Q5: 格式化后格式不对怎么办？

1. **检查输入文件**: 确保标题层级正确
2. **检查规则选择**: 确认使用了正确的学校规则
3. **手动调整**: 部分特殊格式可能需要手动微调

### Q6: 支持双语（中英文）摘要吗？

支持。在 Markdown 中分别使用 `## 摘要` 和 `## Abstract` 标记中英文摘要。

## 故障排除

### 问题：命令未找到

**症状**: `ruledoc: command not found`

**解决方案**:

1. 确认安装成功：
   ```bash
   pip list | grep ruledoc
   ```

2. 检查 Python 脚本路径是否在 PATH 中：
   ```bash
   # Windows
   python -m site --user-site
   # 将 Scripts 目录添加到 PATH
   ```

3. 使用 Python 模块方式运行：
   ```bash
   python -m ruledoc --help
   ```

### 问题：未指定规则错误

**症状**: `错误: 规则 'None' 不存在` 或类似错误

**解决方案**:

必须使用 `--rule` 参数指定规则：
```bash
# 错误
ruledoc thesis.md

# 正确
ruledoc thesis.md --rule yzu_thesis
```

### 问题：Pandoc 转换失败

**症状**: `Error: Pandoc conversion failed`

**解决方案**:

1. 安装 Pandoc：
   ```bash
   # Windows
   winget install pandoc
   
   # macOS
   brew install pandoc
   
   # Linux
   sudo apt-get install pandoc
   ```

2. 验证 Pandoc 安装：
   ```bash
   pandoc --version
   ```

### 问题：字体显示异常

**症状**: 生成的文档中文字体显示不正确

**解决方案**:

1. 确保系统安装了所需字体（如宋体、黑体）
2. 在 Word 中手动调整字体
3. 创建自定义规则指定替代字体

### 问题：内存不足

**症状**: `MemoryError` 或程序崩溃

**解决方案**:

1. 关闭其他占用内存的程序
2. 分批处理大型文档
3. 增加系统虚拟内存

### 问题：权限错误

**症状**: `Permission denied` 或无法写入文件

**解决方案**:

1. 检查输出目录是否有写入权限
2. 确保输出文件未被其他程序打开
3. 尝试输出到其他目录

## 获取帮助

如果您遇到本指南未涵盖的问题：

1. 查看 [README.md](../README.md) 获取项目概览
2. 在 [GitHub Issues](https://github.com/xiaoyu-778/ruledoc/issues) 搜索类似问题
3. 创建新的 Issue 描述您的问题
4. 发送邮件至 3389357760@qq.com

## 更新日志

查看 [CHANGELOG.md](../CHANGELOG.md) 了解版本更新内容。

---

**最后更新**: 2026-04-20
