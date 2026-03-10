# 基因表达数据提取与相关性分析

## 项目简介

本项目提供了一个完整的R Markdown工作流程，用于从BrainSpan和GTEx数据库提取基因表达数据，并进行相关性分析和可视化。特别针对脑组织数据进行了详细分析，支持BrainSpan发育脑数据和GTEx成人脑数据的独立分析。

## 主要特性

- **双数据源支持**：BrainSpan（发育脑）和GTEx（成人脑）独立分析
- **GTEx亚组织分析**：自动提取并分析13个脑亚组织的表达数据
- **统一参数配置**：所有参数集中管理，便于修改
- **NPG配色方案**：采用Nature Publishing Group配色，专业美观
- **SVG矢量图输出**：高质量图像，支持无损缩放

## 数据源

### BrainSpan
- 发育脑转录组数据
- 包含多个发育时期的脑组织样本
- 数据文件：
  - `columns_metadata.csv` - 样本元数据
  - `rows_metadata.csv` - 基因元数据
  - `expression_matrix.csv` - 表达矩阵

### GTEx
- 成人组织转录组数据（v10版本）
- 从原始GCT文件提取Brain及亚组织数据
- 数据文件：
  - `GTEx_Analysis_2022-06-06_v10_RNASeQCv2.4.2_gene_tpm_non_lcm.gct` - 表达矩阵
  - `GTEx_Analysis_v10_Annotations_SampleAttributesDS.txt` - 样本属性

## 项目结构

```
基因共表达/
├── gene_expression_analysis.Rmd          # 主分析脚本
├── README.md                             # 项目说明文档
├── BrainSpan/                            # BrainSpan数据目录
│   ├── columns_metadata.csv
│   ├── rows_metadata.csv
│   └── expression_matrix.csv
└── output/                               # 输出目录
    ├── BrainSpan_expression_data.xlsx    # BrainSpan表达数据
    ├── BrainSpan_corrgram.svg            # BrainSpan相关性矩阵图
    ├── BrainSpan_scatter_*.svg           # BrainSpan散点图（3个）
    ├── correlation_summary.csv           # 所有相关性统计汇总
    └── GTEx/                             # GTEx输出目录
        ├── GTEx_expression_data.xlsx     # GTEx表达数据（14个工作表）
        ├── Brain_All/                    # 所有Brain样本（3234个）
        │   ├── Brain_All_corrgram.svg
        │   └── Brain_All_scatter_*.svg
        ├── Frontal_Cortex_BA9/           # 额叶皮层BA9（269个样本）
        ├── Cerebellar_Hemisphere/        # 小脑半球（277个样本）
        ├── Substantia_nigra/             # 黑质（183个样本）
        ├── Anterior_cingulate_cortex_BA24/ # 前扣带皮层（233个样本）
        ├── Amygdala/                     # 杏仁核（181个样本）
        ├── Caudate_basal_ganglia/        # 尾状核（300个样本）
        ├── Nucleus_accumbens_basal_ganglia/ # 伏隔核（285个样本）
        ├── Putamen_basal_ganglia/        # 壳核（254个样本）
        ├── Cortex/                       # 皮层（270个样本）
        ├── Hypothalamus/                 # 下丘脑（257个样本）
        ├── Cerebellum/                   # 小脑（266个样本）
        ├── Spinal_cord_cervical_c-1/     # 脊髓（204个样本）
        └── Hippocampus/                  # 海马（255个样本）
```

## 输出文件说明

### 数据文件

1. **BrainSpan_expression_data.xlsx**
   - BrainSpan数据库的表达数据
   - 包含样本信息、年龄、供体和基因表达值

2. **GTEx_expression_data.xlsx**
   - GTEx数据库的表达数据
   - 包含14个工作表：1个总览 + 13个亚组织

### 图像文件

每个数据集生成4个SVG图像：

1. **相关性矩阵图（Corrgram）**
   - 左下角：散点图和拟合线
   - 右上角：相关系数热图
   - 对角线：基因名称

2. **散点图（Scatter plots）**
   - 每对基因一个散点图
   - 包含R值和P值标注
   - 带有线性拟合线

### 汇总文件

**correlation_summary.csv** - 包含所有相关性分析结果
- Dataset: 数据集名称
- Gene1, Gene2: 基因对
- Correlation: 相关系数
- P_value: 显著性P值
- Method: 分析方法

## 使用方法

### 环境要求

- R >= 4.0
- 必需R包：readxl, writexl, ggplot2, svglite, data.table

### 运行步骤

1. **准备数据**
   - 将BrainSpan数据放入`BrainSpan/`目录
   - 将GTEx数据放入指定路径（需修改`gtex_data_dir`参数）

2. **修改参数**（可选）
   - 打开`gene_expression_analysis.Rmd`
   - 在"参数设置区域"修改：
     - `target_genes`: 目标基因列表
     - `gtex_data_dir`: GTEx数据路径
     - 颜色、字体、图像尺寸等

3. **运行分析**
   ```r
   # 方法1：在RStudio中
   # 打开gene_expression_analysis.Rmd，点击"Knit"

   # 方法2：命令行
   Rscript -e "rmarkdown::render('gene_expression_analysis.Rmd')"
   ```

## 参数配置

所有参数都在Rmd文件开头的"参数设置区域"定义：

```r
# 目标基因
target_genes <- c("SCN1A", "SCN2A", "SCN3A")

# 字体设置
font_family <- "Arial"
font_size_title <- 14
font_size_axis <- 12

# 图像尺寸
fig_width_corrgram <- 8    # 相关性矩阵图宽度
fig_height_corrgram <- 8   # 相关性矩阵图高度
fig_width_scatter <- 4     # 散点图宽度
fig_height_scatter <- 4    # 散点图高度

# NPG配色
point_color <- "#8491B4"   # 点颜色
line_color <- "#E64B35"    # 拟合线颜色

# 分析方法
cor_method <- "pearson"    # 可选: "pearson", "kendall", "spearman"
```

## 分析结果示例

### BrainSpan（发育脑，524个样本）
- MAPK8IP1 vs MAPK8IP2: R = 0.7142, P < 2e-16
- MAPK8IP1 vs MAPK8IP3: R = 0.5249, P < 2e-16
- MAPK8IP2 vs MAPK8IP3: R = 0.7355, P < 2e-16

### GTEx Brain_All（成人脑，3234个样本）
- MAPK8IP1 vs MAPK8IP2: R = 0.6718, P < 2e-16
- MAPK8IP1 vs MAPK8IP3: R = 0.5885, P < 2e-16
- MAPK8IP2 vs MAPK8IP3: R = 0.7566, P < 2e-16

### GTEx亚组织相关性差异
- **Cerebellar Hemisphere**: 相关性最高 (R = 0.43-0.62)
- **Substantia nigra**: 相关性最低 (R = 0.08-0.32)
- **Cerebellum**: 相关性较高 (R = 0.43-0.54)

## 技术特点

1. **高效数据读取**
   - 使用data.table包快速读取大型GCT文件
   - 按需读取目标基因，节省内存

2. **自动化分析流程**
   - 自动识别Brain亚组织
   - 批量生成所有基因对的散点图
   - 自动创建输出目录结构

3. **专业可视化**
   - NPG配色方案，符合学术出版标准
   - SVG矢量格式，支持高质量出版
   - 统一的图像风格和参数

## 注意事项

1. **数据文件体积**
   - GTEx GCT文件约7GB，首次读取需要几分钟
   - 建议使用SSD硬盘以提高读取速度

2. **内存要求**
   - 建议至少8GB内存
   - 使用data.table按行读取可降低内存占用

3. **路径设置**
   - Windows路径使用正斜杠`/`或双反斜杠`\\`
   - 确保数据文件路径正确

## 依赖包

```r
install.packages(c("readxl", "writexl", "ggplot2", "svglite", "data.table"))
```

## 数据来源

- **BrainSpan**: http://www.brainspan.org/
- **GTEx**: https://gtexportal.org/

## 许可证

本项目仅供学术研究使用。数据使用需遵守BrainSpan和GTEx的数据使用协议。

## 联系方式

如有问题或建议，请提交Issue。
