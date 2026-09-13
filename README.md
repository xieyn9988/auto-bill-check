# 多数据源电商订单自动对账系统

> 基于 Python + Pandas 的多渠道订单自动对账工具，支持 MySQL / Excel / CSV 三种数据源（可扩展）混合对账。

---

## 📌 项目背景

电商业务存在多平台、多代理商台账，数据分散在数据库、Excel、CSV 导出文件中。人工核对耗时长，容易漏单、金额对不上，历史对账文件容易覆盖丢失。

本项目通过自动化对账引擎，统一接入多源数据，自动识别差异，一键生成归档报告。

---

## ✨ 实现功能

1. **多源接入**：数据源支持 MySQL / Excel / CSV 任意组合，可灵活配置渠道
2. **脏数据处理**：CSV 自动识别编码（utf-8-sig / utf-8 / gbk），自动清洗金额符号、空格、逗号
3. **容错降级**：单个渠道加载失败不中断整个任务，自动跳过并记录日志
4. **自动归档**：生成带时间戳的 Excel 报告
   - ✅ 正常：全部订单总表 + 差异明细表
   - ✅ 可用渠道不足：生成故障说明报表
5. **日志持久化**：完整记录每一次执行情况，便于排查

---

## 🏗 项目架构分层

| 层级 | 职责 |
|------|------|
| 数据接入层 | 多文件 / 数据库读取，统一输出 DataFrame |
| 对账引擎层 | 独立的多表外连接与异常判定算法 |
| 报表归档层 | Excel 多 sheet 生成、时间戳命名 |
| 调度入口层 | 任务总流程控制，分支处理成功 / 失败场景 |

---

## 🛠 技术栈

- **语言**：Python 3.x
- **数据处理**：Pandas、NumPy
- **数据库**：SQLAlchemy、PyMySQL
- **Excel 引擎**：openpyxl
- **运行环境**：Jupyter Notebook

---

## 🚀 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/xieyn9988/auto-bill-check.git
cd auto-bill-check
```

### 2. 安装依赖

```bash
pip install -r requirements.txt
```

### 3. 配置渠道

编辑 `main.ipynb` 中的 `TARGET_CHANNEL_CONFIG`，指定每个渠道的数据源类型：

```python
TARGET_CHANNEL_CONFIG = [
    {"name": "合作分销商台账", "source": "excel"},
    {"name": "平台后台账单", "source": "csv"},
    # {"name": "ERP内部数据", "source": "mysql"},
]
```

### 4. 运行对账

用 Jupyter 打开 `main.ipynb`，从头到尾运行所有 Cell。

### 5. 查看结果

- 日志：`unified_bill_log.txt`
- 报告：`output_sample/对账报告_YYYYMMDD_HHMMSS.xlsx`

---

## 📂 项目结构

```text
auto-bill-check/
├── notebooks/
│   ├── main.ipynb           # 主入口：任务调度
│   └── main2.ipynb          # 对账逻辑测试
├── config/
│   └── config_sample.txt    # 配置示例
├── output_sample/           # 输出示例
├── requirements.txt
├── .gitignore
└── README.md
```

---

## 🔮 可扩展方向

- 自动创建 report 文件夹，所有报告统一归档
- 增加定时调度（`schedule` 库），实现每日自动对账
- 增加邮件推送对账结果
- 增加数据库写入对账结果，形成对账台账
- 增加可视化统计（订单数量、差异占比）

---

## 📬 联系方式

- 邮箱：420309519@qq.com
- GitHub：[@xieyn9988](https://github.com/xieyn9988)
