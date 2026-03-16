# F1 Historical Data

F1 历史数据存储仓库，包含 2023-2026 年 F1 大奖赛的完整数据，由 Open F1 API 采集生成。

## 目录结构

```
F1_Historical_Data/
├── meetings/{year}.json           # 年度赛事列表
├── sessions/{year}.json           # 年度所有会话
├── session_data/{session_key}/    # 单场详情数据
│   ├── session.json               # 会话信息
│   ├── drivers.json               # 车手成绩
│   ├── laps.json                  # 圈速数据
│   ├── position.json              # 位置数据
│   ├── intervals.json             # 间隔数据
│   ├── pit.json                   # 进站数据
│   ├── stints.json                # 轮胎数据
│   ├── car_data.json              # 遥测数据（油门/刹车/挡位等）
│   └── location.json              # 位置追踪数据
└── metadata/                      # 元数据
    ├── race_podiums_2023.json     # 2023 年正赛前三名
    ├── race_podiums_2024.json     # 2024 年正赛前三名
    ├── race_podiums_2025.json     # 2025 年正赛前三名
    ├── race_podiums_2026.json     # 2026 年正赛前三名
    └── race_podiums_2023_2026.json # 四年汇总
```

## 数据采集

使用 `scripts/f1_data_collector/collect_f1_data.py` 脚本从 Open F1 API 采集数据：

```bash
cd scripts/f1_data_collector
pip install -r requirements.txt
python collect_f1_data.py -o /path/to/F1_Historical_Data -y 2023,2024 --git-push
```

## 领奖台元数据生成

使用 `generate_podiums.py` 脚本生成正赛前三名元数据：

```bash
cd scripts/f1_data_collector
python generate_podiums.py -o /path/to/F1_Historical_Data -y 2023,2024,2025,2026
```

生成的文件：
- `metadata/race_podiums_{year}.json` - 各年独立文件
- `metadata/race_podiums_2023_2026.json` - 四年汇总文件

## 数据更新与推送

### 查看当前状态

```bash
cd /path/to/F1_Historical_Data
git status
```

### 添加所有改动

```bash
git add .
```

或者只添加特定文件：

```bash
# 添加某个年份的领奖台数据
git add metadata/race_podiums_2025.json

# 添加所有元数据文件
git add metadata/

# 添加所有 session 数据
git add session_data/
```

### 提交更改

```bash
git commit -m "你的提交信息"
```

**提交信息示例**：
```bash
# 更新领奖台数据
git commit -m "Update 2025 podium data for British Grand Prix"

# 添加新的 session 数据
git commit -m "Add session data for 2025 Round 10"

# 批量更新
git commit -m "Update 2025 data: add podiums and session details"
```

### 推送到 GitHub

```bash
git push origin main
```

### 完整推送流程示例

```bash
# 1. 进入仓库目录
cd D:\Coding Demo\202603_F1_Data\F1_Historical_Data

# 2. 查看改动
git status

# 3. 添加所有改动
git add .

# 4. 提交更改
git commit -m "Update metadata and session data"

# 5. 推送到 GitHub
git push origin main
```

### 从 GitHub 拉取最新数据

```bash
# 拉取远程分支并合并到本地
git pull origin main
```

### 解决冲突（如有必要）

```bash
# 1. 拉取时如有冲突，先编辑解决冲突文件
# 2. 标记冲突已解决
git add <冲突文件>

# 3. 继续完成合并
git commit -m "Resolve merge conflicts"

# 4. 推送
git push origin main
```

## 数据来源

- **Open F1 API**: https://api.openf1.org/
- 数据通过 Python 脚本定期采集并推送到本仓库

## 使用方式

本仓库的数据主要供 F1 Dashboard Android 应用使用，通过 GitHub Raw URL 直接读取 JSON 文件。

示例 URL：
```
https://raw.githubusercontent.com/lpz7777777/F1_Historical_Data/main/meetings/2025.json
https://raw.githubusercontent.com/lpz7777777/F1_Historical_Data/main/metadata/race_podiums_2025.json
```

## 注意事项

1. **文件大小限制**: GitHub 单个文件限制为 100 MB，`car_data.json` 和 `location.json` 可能较大，需要注意降采样
2. **Git LFS**: 如需要存储大文件，建议配置 Git LFS
3. **数据验证**: 推送前建议验证 JSON 格式正确性
4. **提交频率**: 建议批量更新后再推送，避免频繁的小提交

## 许可证

数据来源于 Open F1 API，本仓库仅用于存储和历史数据整理。
