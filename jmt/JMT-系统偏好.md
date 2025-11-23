# 晶贸通项目 - 用户偏好规范

> 本文档记录了晶贸通项目开发过程中沉淀的用户偏好规范，所有开发工作应严格遵循这些规范。

## 📋 目录

- [UI/UX 设计偏好](#uiux-设计偏好)
- [数据展示规范](#数据展示规范)
- [布局与排版](#布局与排版)

---

## UI/UX 设计偏好

### 单选按钮布局偏好

**适用场景**：是否类选择项、审核结果选择等二选一或多选一场景

**设计规范**：
- ✅ 使用平铺的 radio button 展示
- ✅ 采用卡片式设计
- ✅ 支持点击整个卡片区域进行选择
- ✅ 桌面端：横向排列
- ✅ 移动端：纵向排列

**示例代码**：

```html
<!-- 桌面端 - 横向排列 -->
<div class="radio-group">
    <label class="radio-option selected" onclick="selectOption(this, 'yes')">
        <input type="radio" name="choice" value="yes" checked>
        <span class="radio-label">✓ 是</span>
    </label>
    <label class="radio-option" onclick="selectOption(this, 'no')">
        <input type="radio" name="choice" value="no">
        <span class="radio-label">✗ 否</span>
    </label>
</div>

<style>
.radio-group {
    display: flex;
    gap: 16px;
}
.radio-option {
    border: 2px solid #d9d9d9;
    border-radius: 8px;
    padding: 14px 16px;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    align-items: center;
    gap: 12px;
    flex: 1;
}
.radio-option:hover {
    border-color: #1890FF;
    background: #f0f5ff;
}
.radio-option.selected {
    border-color: #1890FF;
    background: #e6f7ff;
}
</style>
```

```html
<!-- 移动端 - 纵向排列 -->
<style>
.radio-group {
    display: flex;
    flex-direction: column;
    gap: 12px;
}
</style>
```

---

### 流程步骤精简偏好

**原则**：简化业务流程展示步骤，避免冗余环节

**实施要点**：
- ❌ 删除非必要或可合并的流程节点
- ❌ 避免展示中间过渡步骤
- ✅ 只保留关键业务节点
- ✅ 保持流程清晰简洁

**典型案例**：

退税流程优化：
```
原流程：申报 → 审核 → 退库 → 到账
优化后：申报 → 平台初审 → 到账
```

主体登记流程：
```
简化流程：材料填报 → 材料初审 → 现场核验 → 核准通过
```

---

## 数据展示规范

### 参考数据展示规范

**适用场景**：展示基于非实时或非官方数据计算的结果时

**强制要求**：
- ⚠️ 必须明确标注数据来源性质
- ⚠️ 必须添加免责提示
- ⚠️ 必须使用醒目的视觉标识

**标准提示语模板**：
- "基于网络数据计算"
- "仅供参考"
- "不代表实际结果"
- "不代表实际到账金额"

**示例代码**：

```html
<div class="form-row" id="referenceRow">
    <div class="form-item">
        <div class="form-label">参考到账金额(人民币)</div>
        <div class="input-group">
            <input type="text" class="form-input" disabled id="referenceCnyAmount">
            <span class="input-suffix">CNY</span>
        </div>
        <div class="hint-text" style="color:#fa8c16">
            ⚠️ 基于网络数据计算(参考汇率7.2385),不代表实际结汇到账金额,仅供参考
        </div>
    </div>
</div>
```

**颜色规范**：
- 使用橙色 `#fa8c16` 作为警告提示色
- 使用 ⚠️ 图标增强视觉识别

---

## 布局与排版

### 图表布局偏好

**原则**：图表采用纵向布局

**理由**：
- 便于在 A4 纸上查看
- 便于打印输出
- 提升可读性

**适用范围**：
- 时间线组件
- 流程图
- 进度展示
- 审核历史

**示例**：

```html
<!-- 纵向时间线 -->
<div class="timeline">
    <div class="timeline-item completed">
        <div class="timeline-dot">✓</div>
        <div class="timeline-title">步骤一</div>
        <div class="timeline-desc">描述信息</div>
        <div class="timeline-time">2025-10-25 10:30</div>
    </div>
    <div class="timeline-item current">
        <div class="timeline-dot">⏳</div>
        <div class="timeline-title">步骤二</div>
        <div class="timeline-desc">描述信息</div>
        <div class="timeline-time">预计完成: 2025-10-26</div>
    </div>
</div>

<style>
.timeline {
    position: relative;
    padding-left: 32px;
}
.timeline::before {
    content: '';
    position: absolute;
    left: 11px;
    top: 0;
    bottom: 0;
    width: 2px;
    background: #e8e8e8;
}
.timeline-item {
    position: relative;
    padding-bottom: 24px;
}
</style>
```

---

## 📌 开发注意事项

1. **优先级**：所有偏好规范均为强制要求，不可擅自修改
2. **一致性**：新增功能必须遵循已有偏好规范
3. **扩展性**：发现新的偏好模式应及时更新本文档
4. **审核要求**：代码审核时需检查是否符合用户偏好规范

---

## 🔄 更新日志

- **2025-11-03**：初始版本，整理所有已沉淀的用户偏好规范

---

> **维护说明**：本文档由 AI 辅助生成，基于项目开发过程中的实际偏好沉淀。如有更新需求，请及时修改本文档。
