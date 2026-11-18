# 定时检查任务：必须做的验证（用户要求）

用户要求：**不能只看 `check` 的输出就下结论，每次运行都要独立验证。** 即使 `candidates` 为空也要做。

在 `check` 之后、`commit` 之前，做以下核对：

1. 运行 `python3 -I greylock_jobs.py audit --state state`（只读，不改状态文件）。
   - 看 `new_since_seen_any_type`：逐个看这次新出现的全部岗位标题，确认没有因为标题关键词太窄而漏掉的学生岗（例如 "Associate"、"Junior"、"Summer 2027"、"Residency"）。
   - 看 `per_company_counts`：某家公司岗位数为 0 或比平时骤降，说明抓取可能失效，要在总结里说明。
2. 用 `get_values` 读表格 G 列，和 `live_studentish_us_or_unknown` 比对。表格里没有、但看起来像学生岗的，打开链接读 JD，判断该不该加（规则同步骤 2：美国、不是只招 PhD、确实面向学生）。该加的就加。
   - 已经确认过不加的（截至 2026-11-18）：Baseten「Base Labs Fellowship」、Cogent「Cogent AI Fellowship」、Snorkel「Research Fellow」（都只招 PhD 或博士后）；Axiamatic「Agentic Delivery Analyst Intern」（只写 Remote，确认不了是美国）。除非 JD 有变化，否则不用重复报告。
3. 用 WebSearch 搜一两次（例如 "<公司> intern 2027"、"<公司> new grad 2027"），看有没有发在招聘页以外的学生岗。
4. 总结里写明用了哪些验证方法、各自的结果；拿不准的岗位要列出来并给链接，不要静默丢掉。
