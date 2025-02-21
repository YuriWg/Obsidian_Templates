---
tags:
  - type/structure
  - structure/bujo/weekly
start_date: 
end_date: ""
template: "[[5_BuJo - Weekly Log]]"
created: 2024-12-29T03:48
updated: 2025-02-20T23:34
---
# 📅 一周回顾 WEEKLY REVIEW 

--- start-multi-column: WeeklyStats

```column-settings
number of columns: 2
border: off
column-gap: 10px
shadow: off
Column Size: [35%, 65%]
scroll bar: off
```

```dataviewjs
// 获取当前日期和时间
const now = new Date(dv.current().end_date);
// 获取当前年份和月份
const currentYear = now.getFullYear();
const currentMonth = now.getMonth();
 // Start of Selection
const currentWeek = Math.ceil((now - new Date(now.getFullYear(), 0, 1)) / (1000 * 60 * 60 * 24 * 7)); 

 // Start of Selection
// 初始化月历的HTML字符串，设置显示格式为网格布局
let monthCalendarHTML = `
<div style="
  display: flex; 
  flex-direction: column;  
  background-color: hsla(var(--interactive-accent-hsl), 0.05); 
  border-radius: 12px; 
  padding: 1.5rem; 
  height: 350px;
  min-width: 300px; 
  width: 100%;
  box-shadow: 0 2px 8px rgba(0,0,0,0.05);  
">
  <div style="
    color: var(--interactive-accent);
    font-weight: 600; 
    font-size: 18px;  
    padding: 1rem 1rem;
    border-bottom: 1px solid hsla(var(--interactive-accent-hsl), 0.2); 
  ">${currentYear} ${["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"][currentMonth]} W${currentWeek}</div>
  <div style="
    display: grid; 
    grid-template-columns: repeat(7, 1fr); 
    gap: 0.2rem;  
    padding: 1.2rem 0.5rem;
  ">`;

 // Start of Selection
// 添加星期标题
monthCalendarHTML += ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
  .map(day => `<div style="
    text-align: center; 
    font-weight: bold; 
    font-size: 14px; 
    color: var(--interactive-accent);
    padding: 1rem 0rem;
    background-color: hsla(var(--interactive-accent-hsl), 0.05);  
    border-radius: 2rem;
  ">${day}</div>`)
  .join("");

// 获取本周最后一天
const endDate = new Date(dv.current().end_date);


// 这个函数用于判断给定的日期是否在当前周内，并设置高亮样式
const isThisWeek = (date) => {
    // 创建一个新的日期对象，表示本周的结束日期
    const startOfWeek = new Date();
    // 计算本周的开始日期（周一）
    startOfWeek.setDate(endDate.getDate() - endDate.getDay() + 1); // 周一
    // 返回给定日期是否在本周的范围内
    return date >= startOfWeek && date <= endDate; // 结束日期为本周最后一天
};

// 获取当前月的第一天
const firstDay = new Date(currentYear, currentMonth, 1);
// 获取当前月的最后一天
const lastDay = new Date(currentYear, currentMonth + 1, 0);
// 获取该月的总天数
const totalDays = lastDay.getDate();
// 计算当前月第一天是周几，将周一作为一周的第一天
const startDayOfWeek = (firstDay.getDay() + 6) % 7;

// 填充空白天数
let currentDay = 1;
for (let row = 0; row < 6; row++) { // 最多6行
    for (let col = 0; col < 7; col++) {
        if (row === 0 && col < startDayOfWeek) {
            // 在月历的开始部分填充空白
            monthCalendarHTML += `<div></div>`;
        } else if (currentDay <= totalDays) {
            // 构建日期字符串
            const dateStr = `${currentYear}-${String(currentMonth + 1).padStart(2, '0')}-${String(currentDay).padStart(2, '0')}`;
            // 检查是否有对应的日记
            const page = dv.page(dateStr);
            // 设置日期的默认样式
            let dayStyle = `
              text-align: center;  
              border-radius: 50%;  
              font-size: 14px; 
              display: flex; 
              justify-content: center; 
              align-items: center; 
              color: var(--text-muted);
              width: 30px;  
              height: 30px;
              transition: all 0.2s ease;  
            `;
            // 如果是这周，设置高亮样式
            const isThisWeek = (date) => {
                const startOfWeek = new Date(endDate);
                startOfWeek.setDate(endDate.getDate() - 6); // 从endDate往前倒推7天
                return date >= startOfWeek && date <= endDate; // 结束日期为本周最后一天
            };

            if (isThisWeek(new Date(currentYear, currentMonth, currentDay))) {
                dayStyle += `
                  background-color: var(--interactive-accent); 
                  color: white; 
                  box-shadow: 0 2px 6px rgba(var(--interactive-accent-rgb), 0.3);  
                `;
            } else if (page) {
                // 如果该日期有对应的日记，设置另一种样式
                dayStyle += `
                  background-color: hsla(var(--interactive-accent-hsl), 0.15); 
                  color: var(--interactive-accent);
                  font-weight: 500;
                `;
            }
            // 添加日期元素
            monthCalendarHTML += `<div style="${dayStyle}">${currentDay}</div>`;
            currentDay++;
        }
    }
}

// 结束HTML
monthCalendarHTML += `
  </div>
  <div style="
    width: 100%;
    height: 1px;
    background-color: hsla(var(--interactive-accent-hsl), 0.2);
    margin: 0.3rem 0rem;
    border-radius: 2px;
  "></div>
</div>`;

// 输出月历
dv.paragraph(monthCalendarHTML);
```

--- end-column ---


```dataviewjs
// 创建容器样式（与月历样式保持一致）
const containerStyle = `  
  display: flex; 
  background-color: hsla(var(--interactive-accent-hsl), 0.05); 
  border-radius: 12px; 
  padding: 1.2rem; 
  height: 350px; 
  box-shadow: 0 2px 8px rgba(0,0,0,0.05);  
  scroll: off;
`;

// 创建标题和网格布局
let habitHTML = `
<div style="${containerStyle}">
  <div style="
    display: grid; 
    grid-template-columns: 80px repeat(7, 1fr);  /* 缩小网格列宽 */
    gap: 0.5rem; 
    padding: 1.5rem;  
    margin-left: 0.2rem;
    width: 100%;
  ">  
    <div style="
      text-align: left; 
      font-weight: bold;
      font-size: 14px;  
      color: var(--interactive-accent); 
    "></div>
    ${["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"].map(day => `
        <div style="
          align-items: center;
          font-weight: bold; 
          font-size: 15px;  
          color: var(--interactive-accent);
          padding: 0rem 0rem;  
          margin: 0.5rem 0rem;
          border-radius: 20px;
        ">${day}</div>
    `).join('')}
`;

// 根据模板的end_date属性计算本周范围
const endDate = dv.date(dv.current().end_date);
const startDate = dv.date(dv.current().start_date);


// 获取本周数据
const pages = dv.pages('"0_Bullet Journal/Daily Notes"') // 从指定路径获取日记页面
  .where(p => {
    const noteDate = dv.date(p.file.name.split(" ")[0]); // 提取文件名中的日期部分并转换为日期对象
    return noteDate >= startDate && noteDate <= endDate; // 过滤出在开始日期和结束日期之间的页面
  })
  .sort(p => p.file.name, 'asc'); // 按文件名升序排序

// 生成每个日期的习惯卡片（调整内边距和字体大小与月历对齐）
// 定义习惯名称
const habits = ['🧘 拉伸', '📓 日记', '💻 笔记', '🏃 慢跑', '📖 阅读', '🤔 Stoic', '💢 心情'];  

// 遍历每个习惯
habits.forEach(habit => {
  // 显示习惯名称
  habitHTML += `
  <div style="text-align: left; 
  font-size: 14px; 
  font-weight: bold; 
  margin: 0.1rem 0.3rem; 
  height: 0.8rem;
  color: var(--interactive-accent)">${habit}</div>`;  

  // 遍历每个页面
  pages.forEach(page => {
    // 提取日期和星期
    const dateStr = page.file.name.split(" ")[0];
    const dayOfWeek = page.file.name.split(" ")[1];

    // 修复习惯映射并处理未定义值
    const isHabitDone = {
      '🧘 拉伸': page?.stretch || false,
      '📓 日记': page?.journal || false,
      '💻 笔记': page?.notes || false,
      '🏃 慢跑': page?.running || false,
      '📖 阅读': page?.reading || false,
      '🤔 Stoic': page?.stoic || "",
      '💢 心情': page?.mood ?? null
    }[habit];

    // 根据心情值显示不同表情
    const moodEmoji = {
      [-5]: '😱', // 极度痛苦
      [-4]: '😖', // 非常糟糕
      [-3]: '😟', // 糟糕
      [-2]: '😔', // 不太好 
      [-1]: '😕', // 稍差
      0: '😐',    // 中性
      1: '🙂',    // 尚可
      2: '😊',    // 不错
      3: '😄',    // 良好
      4: '🥰',    // 很好
      5: '😇'     // 完美
    }[isHabitDone !== null && isHabitDone !== undefined ? isHabitDone : ''] ?? '';  // 确保isHabitDone不为null或undefined


    habitHTML += `
    <div style="
      background-color: ${habit === '💢 心情' ? 'transparent' : (isHabitDone ? 'var(--interactive-accent)' : 'transparent')};
      border: 1.5px dotted ${habit === '💢 心情' ? 'transparent' : (isHabitDone ? 'hsla(var(--interactive-accent-hsl), 0)' : 'hsla(var(--interactive-accent-hsl), 0.8)')};
      border-radius: 50%;  
      width: 100%;
      aspect-ratio: 1; 
      display: flex;
      height: 1.5rem;
      width: 1.5rem;
      margin: 0.2rem 0.2rem;
      align-items: center;
      justify-content: center;
      transition: all 0.2s ease;
      ${isHabitDone ? 'box-shadow: 0 2px 6px rgba(var(--interactive-accent-rgb), 0.5);' : ''}
    ">
      <span style="color: ${habit === '💢 心情' ? 'var(--interactive-accent)' : 'white'}; font-size: ${habit === '💢 心情' ? '1.5rem' : '0.8rem'};">
        ${habit === '💢 心情' ? moodEmoji : (isHabitDone ? '✓' : '')}
      </span>  
    </div>`;
  });
});

// 闭合容器
habitHTML += `</div></div>`;  // 结束网格布局

// 输出结果
dv.paragraph(habitHTML);  // 输出生成的HTML
```

--- end-multi-column

---  
# ✊ 长期主义 Longtermism

```dataviewjs
// 获取符合条件的文件并按日期升序排序
const pages = dv.pages('"0_Bullet Journal/Daily Notes"')
    .where(p => dv.date(p.file.name.split(" ")[0]) > dv.date(dv.current().end_date) - dv.duration("7 days") &&
                dv.date(p.file.name.split(" ")[0]) <= dv.date(dv.current().end_date))
    .sort(p => dv.date(p.file.name.split(" ")[0]), 'asc');  // 新增排序逻辑

// 创建表格头
dv.table(["📅 日期", "🌟 星期", "📏 距离(km)", "💨 配速(min/km)", "🫀 心率(BPM)", "📖 阅读时长(min)", "💢 心情指数"],
    pages.map(p => [
        p.file.name.split(" ")[0],  // 日期
        p.file.name.split(" ")[1],  // 星期
        p.Distance_km,              // 距离
        p.Pace_per_km,              // 配速
        p.Heart_Rate_BPM,           // 心率
        p.Reading_min,              // 阅读时长
        p.mood                      // 心情指数
    ])
);
```

--- 
# 🍚 精神食粮 Spiritual Nourishment
### 🤔 每日斯多葛 Stoic
```dataview
LIST without id Stoic
FROM "0_Bullet Journal/Daily Notes"
WHERE date(split(file.name, " ")[0]) > date(this.end_date) - dur(7 days)
    AND date(split(file.name, " ")[0]) <= date(this.end_date)
    AND Stoic
```
### 📖 书籍 Book
```dataview
LIST without id "《" + book + "》" + " 本周共计📖时长" + sum(rows.Reading_min) + "min " 
FROM "0_Bullet Journal/Daily Notes"
WHERE date(split(file.name, " ")[0]) > date(this.end_date) - dur(7 days)
    AND date(split(file.name, " ")[0]) <= date(this.end_date)
    AND book
GROUP BY book
```
### 👂播客 Podcast
```dataview
LIST without id podcast
FROM "0_Bullet Journal/Daily Notes"
	WHERE date(split(file.name, " ")[0]) > date(this.end_date) - dur(7 days)
	AND date(split(file.name, " ")[0]) <= date(this.end_date)
	AND podcast != null
```
### 📑 文章 Airticle
```dataview
LIST 
FROM "2_Literature notes/2_1_Article"
WHERE file.ctime > date(this.end_date) - dur(7 days) AND file.ctime <= date(this.end_date)
```
### 🎬 电影电视 Movie & TV
```dataview
LIST
FROM "2_Literature notes/2_4_Movie TV"
WHERE file.ctime > date(this.end_date) - dur(7 days) AND file.ctime <= date(this.end_date)
```
### 📱 视频 Vedio
```dataview
LIST 
FROM "2_Literature notes/2_5_Vedio"
WHERE file.ctime > date(this.end_date) - dur(7 days) AND file.ctime <= date(this.end_date)
```
### 🎵 音乐 Music
- 
---
# 💬 金句漫游 Quotes Tour
```dataview
LIST quote
FROM "0_Bullet Journal/Daily Notes"
	WHERE date(split(file.name, " ")[0]) > date(this.end_date) - dur(7 days)
	AND date(split(file.name, " ")[0]) <= date(this.end_date)
	AND quote != null
```
---
# 🧰 重点行动项 Key Tasks
<!-- 本周截止的任务 -->
```tasks
due on this week
sort by due reverse
short
```
### ✅ 本周已完成项 Done
<!-- 本周已完成的任务 -->
```tasks
done on this week
sort by done reverse
short
```

### ⌛️ 进行中的项目 Underway
<!-- 本周进行中的任务 -->
```tasks
not done
sort by due reverse
group by priority
hide tags
short
```
---
# 🤔 收获与思考 Harvest & Thinking
### 🏆 本周亮点 Weekly Highlights
<!-- What important tasks did you complete? What progress is worth celebrating? -->
- 
### 🧗 克服的挑战 Challenges Overcome
<!-- What difficulties or obstacles did you encounter? How did you address these challenges? -->
- 
### 💡 关键洞察 Key Insights
<!-- What new knowledge or skills did you learn? What new inspirations or ideas did you have? -->
- 
### 🛠️ 改进空间 Areas for Improvement
<!-- What could be improved today? How can you do better tomorrow? -->
- 

### 🌱 成长轨迹 Growth Trajectory
<!-- This week's progress in relation to long-term goals -->
- 

---
# 🎯 下周计划与目标 Plans & Goals for Next Week

- 继续坚持长期主义，良好的习惯继续坚持✊
- 

---
# 💻 知识库维护 Knowledge System
### ➕ 本周创建的笔记 Created
```dataview
TABLE without id file.cday AS Date, file.link AS Name, file.folder AS Folder
WHERE file.cday > date(this.end_date) - dur(7 days) 
	AND file.cday <= date(this.end_date)
	AND contains(file.folder,"Templates") = False 
	AND contains(file.folder,"5") = False
SORT file.cday DESC,file.folder ASC
//LIMIT 20
```

### 🔧 本周修改的笔记 Updated
```dataview
TABLE without id file.mday AS Date, file.link AS Name, file.folder AS Folder
WHERE file.mday > date(this.end_date) - dur(7 days)
	AND file.mday <= date(this.end_date)
	AND contains(file.folder,"Templates") = False 
	AND contains(file.folder,"5") = False
SORT file.mday DESC,file.folder ASC
```

