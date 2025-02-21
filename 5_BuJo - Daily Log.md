---
tags:
  - type/structure
  - structure/bujo/daily
template_type: BuJo Daily
quote: "[[浊水变澄清，全在自流中。—— 种田山头火]]"
stretch: false
journal: false
notes: false
running: false
reading: false
Stoic: 
podcast: 
book: 
Distance_km: 0
Pace_per_km: 0'0"
Heart_Rate_BPM: 0
Reading_min: 0
mood: 0
project: 
entertainment: 
Red_fans: 
template: "[[5_BuJo - Daily Log]]"
---
```dataviewjs
// 获取当前页面的日期（假设页面文件名为 YYYY-MM-DD 格式）
const pageDate = dv.current().file.name.split(" ")[0];
const [year, month, day] = pageDate.split("-").map(Number);
const currentDate = new Date(year, month - 1, day); // 将文件名转换为日期对象

// 计算本周的起始和结束日期
const currentDayOfWeek = (currentDate.getDay() + 6) % 7; // 当前是周几，周一为第一天
const weekStart = new Date(currentDate);
weekStart.setDate(currentDate.getDate() - currentDayOfWeek); // 周一
const weekEnd = new Date(currentDate);
weekEnd.setDate(currentDate.getDate() + (6 - currentDayOfWeek)); // 周日

// 获取 ISO 周数
const getISOWeek = (date) => {
    const target = new Date(date.valueOf());
    const dayNr = (date.getDay() + 6) % 7;
    target.setDate(target.getDate() - dayNr + 3);
    const jan4 = new Date(target.getFullYear(), 0, 4);
    const dayDiff = (target - jan4) / 86400000;
    return 2 + Math.floor(dayDiff / 7);
};
const weekNumber = getISOWeek(currentDate);

// 格式化标题 
const formattedWeek = `${year} W${weekNumber}`;

// 初始化周历HTML
let weekCalendarHTML = `

<div style="background-color: hsla(var(--interactive-accent-hsl), 0.1); border-radius: var(--callout-radius); padding: 10px; text-align: left; color: var(--text-normal);">
  <div style="
    color: var(--interactive-accent); 
    font-weight: 600;
    font-size: 1.5em;
    padding-left: 30px;
    padding-top: 30px;
  ">${formattedWeek}</div>

  <!-- 新增标题下分割线 -->
  <div style="
    height: 1px;
    background:  hsla(var(--interactive-accent-hsl), 0.3);
    opacity: 0.5;
    margin-top: 20px;
    margin-left: 40px;
    margin-right: 40px;
  "></div>

  <div style="
    display: grid; 
    grid-template-columns: repeat(7, minmax(0, 1fr)); 
    gap: 0.1em; 
    padding: 0.5em 0.5em;
  ">`;

// 添加星期标题
weekCalendarHTML += ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
  .map(day => `<div style="text-align: center; font-weight: bold; font-size: 14px; color: var(--interactive-accent);padding:0.5em 0em; margin-left: 5px; ">${day}</div>`)
  .join("");

// 填充本周日期
for (let i = 0; i < 7; i++) {
    const date = new Date(weekStart);
    date.setDate(weekStart.getDate() + i); // 当前日期
    const dateStr = `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(2, '0')}-${String(date.getDate()).padStart(2, '0')}`;
    const page = dv.page(dateStr); // 检查是否有对应的日记

    // 设置默认日期样式
    let dayStyle = `
      text-align: center; padding:0.5em 0em; border-radius: 20px; font-size: 14px; 
      display: flex; justify-content: center; align-items: center; color: var(--text-muted);margin-left: 5px;`;

    if (date.toDateString() === currentDate.toDateString()) {
        // 高亮当前页面日期
        dayStyle += `
          background-color: var(--interactive-accent); 
          color: white; font-weight: bold;padding: 5px;margin-left:0.5em;margin-right: 0.5em;`;
    } else if (page) {
        // 如果有对应日记
        dayStyle += `
          background-color: hsla(var(--interactive-accent-hsl), 0.3); 
          color: var(--text-muted);`;
    }

    // 填充日期
    weekCalendarHTML += `<div style="${dayStyle}">${date.getDate()}</div>`;
}

// 结束HTML
weekCalendarHTML += `
  </div>
<!-- 移至下方的引用区块 -->
  <div style="
    border-top: 1px solid hsla(var(--interactive-accent-hsl), 0.1);
    margin-top: 0.5em;
    padding-top: 2em;
    padding-bottom: 2em;
    font-size: 1em;
    color: var(--interactive-accent);
    position: relative;
    font-weight: bold;
    line-height: 2; 
    margin-left: 40px;
    margin-right: 40px;
  ">
    <div style="
      position: absolute;
      left: -10px;
      top: 0px;
      font-size: 24px;
      color: var(--interactive-accent);
      opacity: 1;
    ">❝</div>
    ${dv.current().quote ? 
      String(dv.current().quote)
        .replace(/^"+|"+$/g, '')  // 移除首尾引号
        .replace(/(\[\[.*?\]\])|(.*\.md\|?)/g, (match) => {
          // 处理两种格式：
          // 1. Markdown链接格式 [[路径|显示文本]]
          // 2. 纯文本路径格式 路径/文件名.md|显示文本
          const parts = match.split('|');
          return parts.length > 1 ? parts.pop() : parts[0];
        })
        .replace(/\]\]/g, '') :  // 清理残留的链接符号
      '<span style="opacity:0.5">点击编辑添加每日格言</span>'}
  </div>
</div> <!-- 关闭主容器 -->`;


// 输出周历
dv.paragraph(weekCalendarHTML);
```
## 🗓️习惯打卡
```dataviewjs
const habits = [
  { key: 'stretch', label: '🧘 晨起拉伸' },
  { key: 'journal', label: '📓 晨间日记' },
  { key: 'notes', label: '💻 整理笔记' },
  { key: 'running', label: '🏃 有氧慢跑', extra: `距离${dv.current().Distance_km ?? '+'}km 配速${dv.current().Pace_per_km ?? '+'}/km` },
  { key: 'reading', label: '📖 睡前阅读', extra: `《${dv.current().book ?? '又忘记看书啦～🙃'}》${dv.current().Reading_min ?? '+'} min` },
  { key: 'podcast', label: '👂 播客', extra: `${dv.current().podcast ?? '今天啥也没听听🤪'}` },
  { key: 'Stoic', label: '🤔 每日斯多葛', extra: `${dv.current().Stoic ?? '你又偷懒咯～😮‍💨'}` }
];

habits.forEach(habit => {
  const status = dv.current()[habit.key] ? "✅" : "🔲";
  const extra = habit.extra ? ` ${habit.extra}` : '';
  dv.paragraph(`-  ${status} ${habit.label}${extra}`);
});
```

📈[[习惯追踪 Heatmap]]

--- 
## 🛠 临时任务创建区
- [ ] 

--- 
## 📋待办事项
---
### 7️⃣ 天内到期 🔝高优先级任务
```tasks
not done
(priority is high) OR (priority is highest) AND (due before in 7 days)
sort by priority
group by due
short 
```
---
### 📦 所有任务 
```tasks 
not done   
group by tags
hide tags
short
```
---
### ✅ 今日完成项
```tasks
done on {{date:YYYY-MM-DD}}
sort by done reverse
short
```
---
## 👀今日回顾
--- 
### 🌟 高光时刻 Highlights 
<!-- What important tasks did you complete? What progress is worth celebrating? -->
- 
### 🚧 突破障碍 Overcoming Obstacles
<!-- What difficulties or obstacles did you encounter? How did you address these challenges? -->
- 
### 🎁 知识获取 Knowledge Gains
<!-- What new knowledge or skills did you learn? What new inspirations or ideas did you have? -->
- 
### 😊 情绪地图 Emotional Map
<!-- How was your emotional state today? What influenced your emotions? -->
- 
### 🔧 优化空间 Room for Improvement
<!-- What could be improved today? How can you do better tomorrow? -->
- 

---
## Files Created Today
```dataview
TABLE without id file.ctime AS Time, file.link AS Name, file.folder AS Folder
where file.ctime > this.created and file.ctime < (this.created + dur(1 day))
sort file.ctime desc
```
## Files Modified Today
```dataview
TABLE without id file.mtime AS Time, file.link AS Name, file.folder AS Folder
where file.mtime > this.updated and file.mtime < (this.updated + dur(1 day))
sort file.mtime desc
```
