---
created: 2025-03-01T22:09:00
updated: 2025-03-19T10:35
challenge_month: 3
life_stage: 0.8
---
```dataviewjs
// 在最外层添加背景容器
const mainContainer = document.createElement('div');
mainContainer.style.cssText = `
    background: linear-gradient(135deg, #f5f5f5 0%, #fafafa 100%);
    border-radius: 0px;
    padding: 40px;
    margin: 20px 0;
`;

// 生成30天极简主义游戏的日历
const currentDate = new Date();
const challengeMonth = this.challenge_month; 
const currentYear = currentDate.getFullYear();

// 添加环形进度条SVG生成函数
function generateProgressRing(score) {
    const radius = 15;
    const circumference = 2 * Math.PI * radius;
    const progress = (score / 100) * circumference;
    const dashOffset = circumference - progress;
    
    return `
    <svg width="40" height="40" viewBox="0 0 40 40" class="progress-ring">
        <circle cx="20" cy="20" r="${radius}"
            stroke="#e0e0e0"
            stroke-width="3"
            fill="transparent"
        />
        <circle cx="20" cy="20" r="${radius}"
            stroke="currentColor"
            stroke-width="3"
            stroke-dasharray="${circumference} ${circumference}"
            stroke-dashoffset="${dashOffset}"
            fill="transparent"
            transform="rotate(-90 20 20)"
        />
        <text x="50%" y="50%" 
            text-anchor="middle" 
            dy=".3em" 
            font-size="12"
            fill="currentColor">
            ${Math.round(score)}
        </text>
    </svg>`;
}

// 添加处理方式映射
const disposalMethods = {
    0: { label: "立即处理", icon: "🗑️"},
    20: { label: "推荐舍弃", icon: "📤"},
    40: { label: "灵活处理", icon: "🔄"},
    60: { label: "建议保留", icon: "📥"},
    80: { label: "必须保留", icon: "📦"}
};

// 获取处理方式
function getDisposalMethod(score) {
    const thresholds = Object.keys(disposalMethods)
        .map(Number)
        .sort((a, b) => b - a);
    
    for (const threshold of thresholds) {
        if (score >= threshold) {
            return disposalMethods[threshold];
        }
    }
    return disposalMethods[0];
}

try {
    // 1. 直接读取文件内容
    const filePath = '0_Bullet Journal/Monthly Challenge/2025 Mar 30天极简主义挑战/minimalism_items.md';
    const content = await dv.io.load(filePath);
    
    if (!content) {
        throw new Error('无法读取文件内容');
    }
    
    // 2. 定位JSON数据
    const startIndex = content.indexOf('[{');
    const endIndex = content.lastIndexOf('}]') + 2;
    
    if (startIndex === -1 || endIndex <= 1) {
        throw new Error('未找到有效的JSON数据');
    }
    
    const jsonString = content.slice(startIndex, endIndex);
    const rawData = JSON.parse(jsonString);
    const itemCategories = {
        "衣物": {color: "#B4846C", icon: "🧣", order: 1},
        "日用品": {color: "#A7B89D", icon: "🧴", order: 2},
        "食物": {color: "#E1A692", icon: "🍕", order: 3},
        "电子产品": {color: "#7895B2", icon: "📱", order: 4},
        "文具": {color: "#BBAB8C", icon: "✏️", order: 5},
        "书籍": {color: "#E5BA73", icon: "📖", order: 6},
        "装饰品": {color: "#C17C74", icon: "🎀", order: 7},
        "其它": {color: "#939B9F", icon: "🫙", order: 8},
        "default": {color: "#e8e8e8", icon: "📎", order: 9}
    };
        

    // 导出颜色映射
    const setcolors = Object.fromEntries(
        Object.entries(itemCategories).map(([key, value]) => [key, value.color])
    );

    // 导出图标映射
    const categoryIcons = Object.fromEntries(
        Object.entries(itemCategories).map(([key, value]) => [key, value.icon])
    );

    function getCategoryColor(category) {
        return itemCategories[category]?.color || itemCategories.default.color;
    }

    function getCategoryIcon(category) {
        return itemCategories[category]?.icon || itemCategories.default.icon;
    }

    function getSortedCategories() {
        return Object.entries(itemCategories)
            .sort((a, b) => a[1].order - b[1].order)
            .map(([key]) => key);
    }  

    // 3. 从JSON数据中提取表格数据
    const tableData = rawData.map(row => ({
        date: row.日期.replace(/-/g, ''),
        item: row.物品,
        category: row.分类 || 'default',
        freq: Number(row['使用频率']) || 0,
        necessity: Number(row['必要性']) || 0,
        irreplace: Number(row['不可替代性']) || 0,
        space: Number(row['空间负担']) || 0,
        multifunction: Number(row['多功能性']) || 0,
        emotion: Number(row['情感价值']) || 0,
        maintenance: Number(row['维护费用']) || 0,
        cost: Number(row['获取成本']) || 0
    }));

    // 添加生活阶段系数的计算函数
    function getLifeStageMultiplier(stage) {
        const multipliers = {
            1: 0.8,
            2: 1.0,
            3: 1.3
        };
        return multipliers[stage] || 1.0;
    }

    // 修改计算得分函数
    function calculateScore(item) {
        const scores = [
            item.freq,          
            item.necessity,     
            item.irreplace,     
            item.space,         
            item.multifunction, 
            item.emotion,       
            item.maintenance,   
            item.cost          
        ];
        
        const baseScore = scores.reduce((sum, score) => sum + score, 0);
        const lifeStageMultiplier = getLifeStageMultiplier(dv.current().life_stage);
        const finalScore = baseScore * lifeStageMultiplier;
        
        return Math.min(100, Math.max(0, finalScore));
    }

    // 添加月度统计面板
    const monthStats = tableData.reduce((stats, item) => {
        const score = calculateScore(item);
        stats.totalScore += score;
        stats.itemCount++;
        stats.categories[item.category] = (stats.categories[item.category] || 0) + 1;
        return stats;
    }, { totalScore: 0, itemCount: 0, categories: {} });

    // 修改统计面板的风格和内容
    const statsPanel = document.createElement('div');
    statsPanel.style.cssText = `
        display: flex;
        flex-direction: column;
        gap: 15px;
        margin: 20px 0;
        padding: 20px;
    `;

    // 更新统计面板的HTML结构
    statsPanel.innerHTML = `
        <div style="
            display: grid;
            grid-template-columns: 200px 1fr;
            height: 100px;
            gap: 20px;
            margin-bottom: 15px;
        ">
            <div style="
                display: flex;
                align-items: center;
                padding: 25px;
                border-radius: 12px;
            ">
                <div style="
                    width: 60px;
                    height: 60px;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    border-radius: 12px;
                    margin-right: 20px;
                ">
                    <span style="font-size: 3.5em;">📦</span>
                </div>
                <div>
                    <div style="
                        font-size: 2.2em;
                        font-weight: 600;
                        color: var(--text-normal);
                        margin-bottom: 4px;
                    ">${monthStats.itemCount}</div>
                    <div style="
                        font-size: 0.95em;
                        color: var(--text-muted);
                    ">待处理物品</div>
                </div>
            </div>

            <div style="
                display: grid;
                grid-template-columns: repeat(4, minmax(100px, 1fr)); // 设置最小宽度
                grid-template-rows: repeat(2, 30px);  // 固定行高
                gap: 25px 15px;
                min-height: 120px;
                padding: 22px;
                border-radius: 12px;
                place-content: space-between;
                align-items: center;
            ">
                ${Object.entries(monthStats.categories)
                .sort((a, b) => {
                    const orderA = itemCategories[a[0]]?.order || 999;
                    const orderB = itemCategories[b[0]]?.order || 999;
                    return orderA - orderB;
                })
                .map(([category, count]) => `
                    <div style="
                        display: flex;
                        align-items: center;
                        gap: 1rem 1.5rem;
                        padding: 0.3rem 1rem;
                        background: ${setcolors[category] || setcolors.default}99;
                        border-radius: 15px;
                        border: 1px dashed ${setcolors[category] || setcolors.default}99;
                        width: 90%;
                        height: 30px;
                        margin: auto;
                    ">
                        <span style="
                            font-size: 1.2em;
                        ">${categoryIcons[category] || categoryIcons.default}</span>
                        <div style="
                            display: flex;
                            align-items: center;
                            gap: 4px;
                        ">
                            <span style="
                                font-size: 0.85em;
                                color: var(--text-muted);
                            ">${category}</span>
                            <span style="
                                font-size: 1.1em;
                                font-weight: 500;
                                color: var(--text-normal);
                            ">${count}</span>
                        </div>
                    </div>
                `).join('')}
            </div>
        </div>
    `;

    // 修改得分指导说明部分
    const scoreGuide = document.createElement('div');
    scoreGuide.style.cssText = `
        position: relative;
        padding: 8px;
        border-radius: 12px;
        font-size: 0.9em;
        margin-bottom: -15px;
    `;

    // 更新得分指导的HTML结构
    scoreGuide.innerHTML = `
        <div style="
            display: grid;
            grid-template-columns: repeat(5, auto);  // 固定5列，每列180px
            gap: 10px;
            color: var(--text-normal);
            margin-bottom: 15px;
            overflow-x: auto;  // 添加横向滚动
            //min-width: 1000px;  // 确保最小宽度
        ">
            ${[
                {score: '80-100', label: '必须保留', icon: "🗂️"},
                {score: '60-79', label: '建议保留', icon: "📥"},
                {score: '40-59', label: '灵活处理', icon: "🔄"},
                {score: '20-39', label: '推荐舍弃', icon: "📤"},
                {score: '0-19', label: '立即处理', icon: "🗑️"}
            ].map(item => `
                <div style="
                    display: flex;
                    align-items: center;
                    gap: 10px;
                    width: 100%;
                    padding: 3px 10px;
                    border-radius: 20px;
                ">
                    <span style="
                        display: inline-block;
                        width: 8px;
                        height: 8px;
                        border-radius: 50%;
                    "></span>
                    <span>${item.icon}</span>
                    <span style="font-weight: 500;">${item.score}</span>
                    <span style="color: var(--text-muted);">${item.label}</span>
                </div>
            `).join('')}
        </div>
        <div style="
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 100%;
            height: 1px;
            background: var(--background-modifier-border);
            opacity: 0.8;
        "></div>
    `;

    statsPanel.appendChild(scoreGuide);

    // 生成表格布局
    let table = `<div class="grid-container" style=" 
        display: grid; 
        grid-template-columns: repeat(5, 1fr); 
        gap: 15px; 
        background: transparent;
    ">`;

    const totalDays = 30;

    for (let day = 1; day <= totalDays; day++) {
        const dateStr = `2025${String(3).padStart(2,'0')}${String(day).padStart(2,'0')}`;
        const dayRecord = tableData.find(item => item.date === dateStr);
        const dayLabel = `Day ${day}`;
        
        let itemInfo = dayRecord ? `
            <div class="item-card" style="
                display: flex;
                flex-direction: column;
                position: relative;
                gap: 5px;
                width: 140px;
                min-height: 120px;
                height: 100%;
                color: white;
            ">
                <div style="
                    font-size: 1em;
                    font-weight: 500;
                    font-family: 'Inter', -apple-system, sans-serif;
                    opacity: 0.8;
                    text-align: right;
                    position: absolute;
                    top: -5px;
                    right: 5px;
                ">${dayLabel}</div>
                <div style="
                    font-size: 1em;
                    line-height: 1.4;
                    position: absolute;
                    top: 40%;
                    left: 50%;
                    transform: translate(-50%, -50%);
                    text-align: center;
                    width: 98%;
                    overflow-wrap: break-word;
                ">${dayRecord.item}</div>
                <div style="
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    padding: 8px 1px;
                    border-top: 1px dashed rgba(255,255,255,0.2);
                    position: absolute;
                    bottom: 3px;
                    left: 0;
                    right: 0;
                    margin: 0 5px;
                ">
                    <span style="
                        display: flex;
                        align-items: center;
                        margin-left: 5px;
                        font-size: 1em;
                    ">${categoryIcons[dayRecord.category]}</span>
                    <span style="
                        font-size: 0.8em;
                        padding: 2px 8px;
                        border-radius: 20px;
                        background: rgba(255,255,255,0.2);
                        display: flex;
                        align-items: center;
                        gap: 4px;
                    ">
                        <div style="
                            font-weight: 500;
                            color: rgba(255,255,255,0.9);
                        ">${Math.round(calculateScore(dayRecord))}</div>
                        ${getDisposalMethod(calculateScore(dayRecord)).icon}
                    </span>
                </div>
            </div>` : '';

        const content = dayRecord ? `
            <a href="#" style="
                display: flex;
                flex-direction: column;
                align-items: center;
                justify-content: center;
                height: 100%;
                padding: 15px;
                text-align: center;
                text-decoration: none;
                color: white;
            ">${itemInfo}</a>` : `
            <div style="
                color: white;
                font-size: 1em;
                font-weight: 500;
                font-family: 'Inter', -apple-system, sans-serif;
                text-align: right;
            ">${dayLabel}</div>`;

        table += `
            <div style="
                background: ${dayRecord ? getCategoryColor(dayRecord.category) : setcolors.default};
                border: 1px dashed ${dayRecord ? getCategoryColor(dayRecord.category) : setcolors.default}99;
                width: 100%;
                height: 150px;
                transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
                position: relative;
                border-radius: 12px;
                overflow: hidden;
                padding: 10px;
                display: grid;
                align-items: center;
                justify-content: center;
                //opacity: ${dayRecord ? 1 : 1};
            ">${content}</div>`;
    }
    table += `</div>`;

    // 修改主容器的添加顺序
    mainContainer.innerHTML = `
        <div class="minimalism-title" style="
            text-align: center; 
            color: #A67F5D;
            font-family: 'Futura', 'Trebuchet MS', -apple-system;
            font-weight: 700;
            font-size: 120px;
            margin: 20px 0 15px;
            opacity: 0.85;
            letter-spacing: -1.5px;
        ">30 Days</div>
        <div class="minimalism-subtitle" style="
            text-align: center;
            color: #BEA98B;
            font-family: 'Helvetica Neue', 'Arial', -apple-system;
            font-weight: 500;
            font-size: 60px;
            margin: 0 0 30px;
            opacity: 0.75;
            letter-spacing: 0.2px;
        ">Minimalism Game</div>
    `;
    
    mainContainer.appendChild(statsPanel);
    
    const tableContainer = document.createElement('div');
    tableContainer.innerHTML = table;
    tableContainer.style.cssText = `
        width: 100%;
        //max-width: 1000px;
        margin: 30px 0px;
        padding: 0;
    `;
    
    mainContainer.appendChild(tableContainer);
    dv.container.appendChild(mainContainer);
    
} catch (error) {
    console.error('数据处理错误:', error);
    dv.paragraph(`❌ 数据加载失败: ${error.message}`);
}
```