---
created: 2025-02-21T10:58
updated: 2025-02-21T11:42
---
# 5_BuJo - Weekly Log 使用说明

## 1. 文件结构
- **YAML Front Matter**: 包含模板的元数据（标签、日期、创建时间等）
- **周回顾**: 包含日历视图和习惯追踪
- **长期主义**: 展示过去一周的运动和阅读数据
- **精神食粮**: 记录阅读、播客、文章等输入内容
- **金句漫游**: 收集本周的精彩语录
- **重点行动项**: 使用Tasks插件管理任务
- **收获与思考**: 进行周总结和反思
- **下周计划**: 制定下周目标
- **知识库维护**: 自动统计本周创建/修改的笔记

## 2. 使用步骤

### 2.1 初始化
1. 复制模板文件
2. 修改YAML中的`start_date`和`end_date`为本周的起止日期

### 2.2 每日记录
- 在`0_Bullet Journal/Daily Notes`文件夹中创建每日笔记
- 使用以下字段记录数据：
  ```markdown
  stretch: true/false  # 是否完成拉伸
  journal: true/false  # 是否写日记
  notes: true/false    # 是否整理笔记
  running: true/false  # 是否跑步
  reading: true/false  # 是否阅读
  stoic: "内容"        # 斯多葛笔记
  mood: -5到5          # 心情指数
  Distance_km:         # 跑步距离
  Pace_per_km:         # 配速
  Heart_Rate_BPM:      # 心率
  Reading_min:         # 阅读时长
  book: "书名"         # 阅读书籍
  podcast: "播客名"    # 收听播客
  quote: "语录"        # 金句
  ```
注：按自己的需求修改，但记得在对应调用属性代码块中也及时调整！
### 2.3 每周回顾
1. **习惯追踪**：自动生成本周习惯完成情况
2. **长期主义**：查看运动数据和阅读时长
3. **精神食粮**：回顾本周的输入内容
4. **任务管理**：检查本周任务完成情况
5. **总结反思**：填写收获与思考部分
6. **计划制定**：为下周设定目标

## 3. 注意事项
- 确保每日笔记的文件名格式为`YYYY-MM-DD 星期`（如`2024-01-01 周一`）
- 习惯追踪依赖于每日笔记中的特定字段，请确保字段名称正确
- 长期主义部分的数据会自动从每日笔记中提取
- 知识库维护部分会自动统计本周创建/修改的笔记，排除模板文件夹

## 4. 自定义建议
- 可根据个人需求调整习惯列表
- 可修改日历和习惯追踪的样式
- 可添加更多自定义字段来扩展功能

**注意**： 这个模板还有许多值得优化的地方，比如可以进一步简化操作流程、增加更多自动化功能，或者根据个人需求调整数据展示方式。欢迎在使用过程中提出建议！
但请**破除“完美主义”**：允许知识库存在合理混沌，**开始比完美更重要‼️**

希望你也可以通过这个模板系统性地追踪个人成长，培养良好习惯，并进行有效的周回顾与规划。
**“未经审视的生活不值得过”————苏格拉底**
**每一次记录都是对自我成长的见证，每一份坚持都会在未来开花结果。不要害怕开始，也不要因为偶尔的懈怠而自责。持续行动，你会遇见更好的自己！🌟**

## 📎附件
- **插件说明**
    - [Multi-Column Markdown](https://github.com/ckRobinson/multi-column-markdown?tab=readme-ov-file) 支持在Obsidian中创建多列布局
    - [Dataview](https://blacksmithgu.github.io/obsidian-dataview/) 动态查询和展示笔记数据
    - [Tasks](https://publish.obsidian.md/tasks/Introduction) 任务管理和追踪工具
    - [Hover Editor](https://github.com/nothingislost/obsidian-hover-editor) 悬浮编辑窗口，方便快速查看和编辑笔记
    - [Update time on edit](https://github.com/beaussan/update-time-on-edit-obsidian) 自动更新笔记的编辑时间
    
- **参考资料**
    - [Bulletjournal官网](https://bulletjournal.com/)
    - [How to Bullet Journal](https://bulletjournal.com/blogs/faq)
    - [卢曼卡片盒笔记法介绍 (Introduction to the Zettelkasten Method)](https://zettelkasten.de/introduction/zh/)
    - [Obsidian-Zettelkasten-Starter-Kit](https://github.com/groepl/Obsidian-Zettelkasten-Starter-Kit)
    - [Obsidian-Templates for Zettelkasten](https://github.com/groepl/Obsidian-Templates)
    - 《[卡片笔记写作法](https://book.douban.com/subject/35503571/)》简单了解一下这个方法，不需要深入阅读
    - [make_useful_notes_v0-18-0_2023-07-24.pdf](app://obsidian.md/make_useful_notes_v0-18-0_2023-07-24.pdf)
    - [Zettelkasten Iceberg Model v7.pdf](app://obsidian.md/Zettelkasten%20Iceberg%20Model%20v7.pdf)