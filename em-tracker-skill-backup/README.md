# 📄 Editorial Manager 论文投稿状态追踪器

自动追踪学术期刊稿件状态，支持所有使用 Editorial Manager (EM) 系统的期刊。

## ✨ 功能特点

- 🔐 **自动登录** - 一键登录 EM 系统，无需手动操作
- 📊 **状态追踪** - 自动获取所有投稿状态，记录到 Excel
- 🔄 **多账户支持** - 可配置多个期刊账户，批量查询
- 📬 **微信推送** - 状态变化时可选微信通知（需配置 Server酱）
- 📁 **历史记录** - 每篇稿件独立 Excel 文件，方便追踪

## 📚 支持期刊

理论上支持所有使用 Editorial Manager 系统的期刊（1000+），已验证包括：

| 学科领域 | 期刊名称 | 简称 |
|:--------:|:--------|:----:|
| 消化内科 | Gastroenterology | GASTRO |
| 消化内科 | Gut | GUT |
| 消化内科 | American Journal of Gastroenterology | AJG |
| 消化内科 | Inflammatory Bowel Diseases | IBD |
| 综合医学 | The Lancet | LANCET |
| 综合医学 | The Lancet Gastroenterology & Hepatology | LANGAS |
| 仪器测量 | IEEE Transactions on Instrumentation & Measurement | TIM |
| 眼科学 | Eye and Vision | EYE |

## 🚀 快速开始

### 1. 安装依赖

```bash
pip install -r scripts/requirements.txt
```

### 2. 配置账户

复制配置模板并填入账户信息：

```bash
cp scripts/config_template.py scripts/config.py
```

编辑 `config.py`，填入您的期刊账户：

```python
ACCOUNTS = [
    {
        'journal_short_name': 'GASTRO',           # 期刊简称
        'journal_full_name': 'Gastroenterology',  # 期刊全名
        'username': 'your_username',              # 用户名
        'password': 'your_password'               # 密码
    },
]
```

**如何找到期刊简称？**
从投稿网站 URL 中获取，例如：
- `https://www.editorialmanager.com/gastro/` → `GASTRO`
- `https://www.editorialmanager.com/tim/` → `TIM`

### 3. 运行查询

```bash
cd scripts
python main.py
```

## 📁 输出格式

每篇稿件生成独立的 Excel 文件，采用学术三线表格式：

| Timestamp | SubmissionBeganDate | StatusDate | Status | ManuscriptNumber | Title |
|-----------|---------------------|------------|--------|------------------|-------|
| 2024-01-15 10:30 | 2024-01-10 | 2024-01-15 | Under Review | MS2024-001 | ... |

文件组织结构：

```
data/
└── your_username/
    └── MS2024-001_Paper-Title/
        └── Paper-Title_投稿追踪.xlsx
```

## 🔔 微信推送（可选）

1. 访问 [Server酱](https://sct.ftqq.com/) 获取 SendKey
2. 在 `config.py` 中设置：

```python
SERVERCHAN_SENDKEY = "your_sendkey_here"
```

状态变化时会自动推送微信通知。

## ⚠️ 注意事项

- **隐私安全** - 所有数据存储在本地，账户信息仅用于登录 EM 系统
- **请求频率** - 程序会在多次查询间自动延迟，避免请求过于频繁
- **网络超时** - 默认超时 20 秒，可在 `config.py` 中调整

## 📄 许可证

MIT License

---

**作者**: [你的名字]
**联系方式**: [你的联系方式]

如果这个工具对你有帮助，欢迎 ⭐ Star 支持！
