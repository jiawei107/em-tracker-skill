---
name: em-tracker
description: |
  Academic journal manuscript status tracker for Editorial Manager system.
  
  Use this skill when the user wants to:
  (1) Check the status of their submitted manuscripts/papers
  (2) Track multiple journal submissions across different journals
  (3) Get notifications when manuscript status changes
  (4) Query submission history for a specific paper
  (5) Manage multiple journal accounts for status tracking
  
  Keywords: 论文追踪, 稿件状态, Editorial Manager, EM, manuscript, submission, journal, 投稿, 期刊, 审稿状态, Gastroenterology, Lancet, AJG, academic publishing.
  
  Supports all journals using Editorial Manager system (1000+ journals including Gastroenterology, Gut, The Lancet series, American Journal of Gastroenterology, etc.).
---

# Editorial Manager Tracker

自动追踪学术期刊稿件状态的技能，支持所有使用 Editorial Manager (EM) 系统的期刊。

## 快速开始

### 1. 安装依赖

```bash
pip install -r requirements.txt
```

### 2. 配置账户

复制配置模板并填入账户信息：

```bash
cp scripts/config_template.py scripts/config.py
# 编辑 config.py，填入您的期刊账户信息
```

或使用交互式配置工具：

```bash
cd scripts
python add_config.py
```

### 3. 运行追踪

```bash
cd scripts
python main.py
```

## 工作流程

1. **登录验证** - 使用配置的账户登录 EM 系统
2. **获取稿件列表** - 自动遍历主菜单，钻取所有稿件详情
3. **状态比对** - 与历史记录比对，检测状态变化
4. **更新记录** - 生成/更新 Excel 历史文件
5. **推送通知** - (可选) 通过 Server酱 发送状态变更通知

## 配置说明

### 账户配置 (config.py)

```python
ACCOUNTS = [
    {
        'journal_short_name': 'GASTRO',           # 期刊简称 (URL中使用的)
        'journal_full_name': 'Gastroenterology',  # 期刊全名 (显示用)
        'username': 'your_username',              # EM账户用户名
        'password': 'your_password'               # EM账户密码
    },
]
```

### 期刊简称查找

期刊简称可在 EM 投稿页面 URL 中找到：
- `https://www.editorialmanager.com/gastro/` → `GASTRO`
- `https://www.editorialmanager.com/ibd/` → `IBD`
- `https://www.editorialmanager.com/eye/` → `EYE`

### 推送通知 (可选)

1. 访问 https://sct.ftqq.com/ 获取 SendKey
2. 在 config.py 中设置 `SERVERCHAN_SENDKEY`

## 输出格式

### Excel 文件

每个稿件生成独立的 Excel 文件，采用学术三线表格式：

| Timestamp | SubmissionBeganDate | StatusDate | Status | ManuscriptNumber | Title |
|-----------|---------------------|------------|--------|------------------|-------|
| 2024-01-15 10:30 | 2024-01-10 | 2024-01-15 | Under Review | MS2024-001 | ... |

### 文件组织

```
data/
└── user@email.com/
    ├── MS2024-001_Paper-Title/
    │   └── Paper-Title_投稿追踪.xlsx
    └── MS2024-002_Another-Paper/
        └── Another-Paper_投稿追踪.xlsx
```

## 支持的期刊

理论上支持所有使用 Editorial Manager 系统的期刊（1000+），已验证的包括：

| 学科领域 | 期刊名称 | 简称 |
|:--------:|:--------|:----:|
| 消化内科 | Gastroenterology | GASTRO |
| 消化内科 | Gut | GUT |
| 消化内科 | American Journal of Gastroenterology | AJG |
| 消化内科 | Inflammatory Bowel Diseases | IBD |
| 综合医学 | The Lancet | LANCET |
| 综合医学 | The Lancet Gastroenterology & Hepatology | LANGAS |
| 眼科学 | Eye and Vision | EYE |

## 注意事项

1. **隐私安全** - 所有数据存储在本地，账户信息仅用于登录 EM 系统
2. **请求频率** - 程序会在多个账户查询间自动延迟，避免请求过于频繁
3. **文件占用** - 若 Excel 文件被占用，程序会暂停等待关闭后重试
4. **网络超时** - 默认超时 20 秒，可在 config.py 中调整

## 脚本说明

| 脚本 | 用途 |
|------|------|
| `main.py` | 主程序，执行追踪查询 |
| `add_config.py` | 交互式账户配置工具 |
| `config_template.py` | 配置文件模板 |
| `requirements.txt` | Python 依赖列表 |
