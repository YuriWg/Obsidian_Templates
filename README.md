# Obsidian_Bujo_Templates

这次单独优化了Dailylog、Weeklylog和Monthlylog文件中日历和打卡部分的视图模版
[视图文件包](https://github.com/YuriWg/Obsidian_Bujo_Templates/blob/main/bujo_calender_views%20V1.0.zip)
![资料清单](https://github.com/YuriWg/Obsidian_Bujo_Templates/blob/main/%E6%9B%B4%E6%96%B0%E8%B5%84%E6%96%99%E6%B8%85%E5%8D%95.png)

使用说明详见[使用指南](https://github.com/YuriWg/Obsidian_Bujo_Templates/blob/main/%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97.pdf)

# 更新说明
- 优化系统架构
- 改进性能表现
- 增强用户体验
- 完善文档说明

## 1 整体架构优化
- CSS样式完全分离：所有样式都统一在 `calendar.css` 中管理
- 统一容器类名：使用 `calendar-container` 实现样式共享
- 代码结构优化：提高可维护性和扩展性
- 统一设计语言：保持视觉风格一致性

## 2 日视图优化
- 改进日期获取逻辑：支持从文件名和created属性获取日期
- 优化周数计算：采用 ISO 周数标准
- 改进引用区块：优化显示位置和样式
- 增强日期单元格：提供更好的视觉反馈

## 3 周视图优化
- 改进日期范围计算
- 优化习惯追踪器显示
- 提升页面查询性能
- 添加错误处理机制
- 支持跨月份周视图

## 4 月视图优化
- 添加年度概览功能
- 优化习惯统计算法
- 改进数据可视化展示
- 增加月度数据分析

## 5 功能增强
- 完善心情追踪系统
  - 使用表情符号直观显示
  - 优化统计算法
- 添加习惯完成率统计
- 优化 Stoic 思考记录
- 增加数据验证机制

## 6 性能优化
- 优化数据查询逻辑
- 减少DOM操作
- 改进日期计算方法
- 添加空值保护机制



