# 晶贸通前端组件化产品需求文档

> **版本**: v1.0  
> **更新时间**: 2025-11-03  
> **文档目标**: 将项目中重复出现的页面模式抽象为可复用的前后端组件

---

## 📋 目录

1. [组件概述](#组件概述)
2. [列表组件 (List Component)](#列表组件-list-component)
3. [详情组件 (Detail Component)](#详情组件-detail-component)
4. [表单编辑组件 (Form Edit Component)](#表单编辑组件-form-edit-component)
5. [进度时间线组件 (Progress Timeline Component)](#进度时间线组件-progress-timeline-component)
6. [审核操作组件 (Audit Action Component)](#审核操作组件-audit-action-component)
7. [状态徽章组件 (Status Badge Component)](#状态徽章组件-status-badge-component)
8. [数据卡片组件 (Data Card Component)](#数据卡片组件-data-card-component)
9. [文件上传组件 (File Upload Component)](#文件上传组件-file-upload-component)
10. [分页组件 (Pagination Component)](#分页组件-pagination-component)
11. [技术实现方案](#技术实现方案)

---

## 组件概述

### 业务场景分析

通过对项目中 **180+ HTML 页面** 的分析，识别出以下高频重复模式：

| 组件类型 | 出现频率 | 典型页面 |
|---------|---------|---------|
| 列表组件 | 40+ 页面 | admin-*-list.html, *-list.html |
| 详情组件 | 45+ 页面 | admin-*-detail.html, *-detail.html |
| 表单编辑组件 | 38+ 页面 | admin-*-edit.html, *-form.html |
| 进度时间线组件 | 25+ 页面 | *-progress.html |
| 审核操作组件 | 12+ 页面 | admin-*-edit.html (审核页) |
| 状态徽章组件 | 60+ 页面 | 几乎所有列表和详情页 |
| 数据卡片组件 | 35+ 页面 | dashboard, home 页面 |
| 文件上传组件 | 20+ 页面 | 各类表单页 |
| 分页组件 | 40+ 页面 | 所有列表页 |

### 组件化收益预估

- **代码复用率**: 预计提升 **60%+**
- **开发效率**: 新页面开发时间减少 **50%+**
- **维护成本**: UI 统一调整工作量减少 **80%+**
- **一致性**: 用户体验一致性提升至 **95%+**

---

## 列表组件 (List Component)

### 业务需求

提供统一的数据列表展示能力，支持筛选、搜索、排序、分页等标准功能。

### 功能特性

#### 核心功能
- ✅ 表格数据展示
- ✅ 筛选条件栏
- ✅ 搜索功能
- ✅ 分页导航
- ✅ 批量操作（可选）
- ✅ 列排序（可选）
- ✅ 数据导出（可选）

#### 交互规则
- 表格行悬停高亮
- 支持行点击跳转详情
- 操作按钮支持权限控制
- 空数据状态展示

### 组件参数

```javascript
{
  // 数据配置
  dataSource: Array,           // 数据源
  columns: Array,              // 列配置
  
  // 筛选配置
  filters: [
    {
      label: String,           // 筛选项标签
      type: 'input|select|date', // 筛选类型
      field: String,           // 字段名
      options: Array,          // 下拉选项（type=select时）
      placeholder: String      // 占位文本
    }
  ],
  
  // 操作配置
  actions: [
    {
      label: String,           // 按钮文字
      type: 'link|button',     // 操作类型
      href: String,            // 跳转链接（type=link时）
      onClick: Function,       // 点击回调（type=button时）
      permission: String       // 权限标识（可选）
    }
  ],
  
  // 分页配置
  pagination: {
    current: Number,           // 当前页码
    pageSize: Number,          // 每页条数
    total: Number,             // 总条数
    onChange: Function         // 页码变化回调
  },
  
  // UI配置
  rowKey: String,              // 行唯一标识字段
  loading: Boolean,            // 加载状态
  emptyText: String            // 空数据提示文字
}
```

### 使用示例

**场景**: 商户列表页

```javascript
// 桌面端
<jmt-list-component
  :data-source="merchantList"
  :columns="[
    { title: '商户ID', field: 'id', width: 100 },
    { title: '商户名称', field: 'name', width: 200 },
    { title: '联系电话', field: 'phone', width: 150 },
    { 
      title: '商户状态', 
      field: 'status', 
      width: 100,
      render: (value) => <StatusBadge status={value} />
    },
    { 
      title: '操作', 
      type: 'actions',
      actions: [
        { label: '查看', href: 'detail.html?id={id}' },
        { label: '编辑', href: 'edit.html?id={id}' }
      ]
    }
  ]"
  :filters="[
    { label: '商户名称', type: 'input', field: 'name' },
    { 
      label: '商户状态', 
      type: 'select', 
      field: 'status',
      options: [
        { label: '全部', value: '' },
        { label: '正常', value: 'active' },
        { label: '冻结', value: 'frozen' }
      ]
    }
  ]"
  :pagination="{
    current: 1,
    pageSize: 20,
    total: 127
  }"
/>
```

### 样式规范

```css
/* 遵循用户偏好 */
.list-component {
  background: #fff;
  border-radius: 8px;
}

.filter-bar {
  padding: 16px 24px;
  display: flex;
  gap: 16px;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 12px 16px;
  text-align: left;
  border-bottom: 1px solid #f0f0f0;
}

th {
  background: #fafafa;
  font-weight: 600;
}

tr:hover {
  background: #fafafa;
}
```

### 响应式适配

**移动端**:
- 表格转为卡片列表
- 筛选条件收起为抽屉
- 操作按钮简化为图标

---

## 详情组件 (Detail Component)

### 业务需求

提供统一的数据详情展示能力，支持信息卡片分组、文件预览、操作按钮等。

### 功能特性

#### 核心功能
- ✅ 信息卡片分组展示
- ✅ 字段标签和值对展示
- ✅ 状态徽章集成
- ✅ 文件/图片预览
- ✅ 操作按钮栏
- ✅ 关联数据展示（可选）
- ✅ 审核历史/时间线（可选）

#### 交互规则
- 卡片可折叠（可选）
- 图片点击放大预览
- 文件点击下载
- 操作按钮支持权限控制

### 组件参数

```javascript
{
  // 数据配置
  sections: [
    {
      title: String,           // 卡片标题
      fields: [
        {
          label: String,       // 字段标签
          value: String|Number|Component, // 字段值
          span: Number,        // 栅格占比 (1-2)
          render: Function     // 自定义渲染（可选）
        }
      ],
      collapsible: Boolean     // 是否可折叠
    }
  ],
  
  // 状态配置
  status: {
    type: String,              // 状态类型
    text: String               // 状态文本
  },
  
  // 操作配置
  actions: [
    {
      label: String,           // 按钮文字
      type: 'primary|default|danger', // 按钮类型
      onClick: Function        // 点击回调
    }
  ],
  
  // 面包屑配置
  breadcrumb: [
    { label: String, href: String }
  ]
}
```

### 使用示例

**场景**: 商户详情页

```javascript
<jmt-detail-component
  :breadcrumb="[
    { label: '商户管理', href: 'list.html' },
    { label: '详情', href: '#' }
  ]"
  :status="{ type: 'active', text: '正常' }"
  :sections="[
    {
      title: '基本信息',
      fields: [
        { label: '商户ID', value: 'M001', span: 1 },
        { label: '商户名称', value: '深圳市晶贸通科技有限公司', span: 1 },
        { label: '统一社会信用代码', value: '91440300MA5XXXXX1X', span: 1 },
        { label: '注册资本', value: '500万元', span: 1 }
      ]
    },
    {
      title: '联系信息',
      fields: [
        { label: '法定代表人', value: '张三', span: 1 },
        { label: '联系电话', value: '138****8888', span: 1 }
      ]
    }
  ]"
  :actions="[
    { label: '编辑', type: 'primary', onClick: goToEdit },
    { label: '返回', type: 'default', onClick: goBack }
  ]"
/>
```

### 样式规范

```css
.detail-component {
  max-width: 1200px;
  margin: 0 auto;
}

.info-card {
  background: #fff;
  border-radius: 8px;
  padding: 24px;
  margin-bottom: 16px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.06);
}

.card-title {
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 20px;
  padding-bottom: 12px;
  border-bottom: 1px solid #f0f0f0;
}

.info-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}

.info-item {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.info-label {
  font-size: 13px;
  color: #999;
}

.info-value {
  font-size: 14px;
  color: #262626;
  font-weight: 500;
}
```

---

## 表单编辑组件 (Form Edit Component)

### 业务需求

提供统一的表单编辑能力，支持多种字段类型、表单验证、分步提交等。

### 功能特性

#### 核心功能
- ✅ 多种字段类型（input/select/textarea/date/upload等）
- ✅ 表单验证（必填/格式/自定义规则）
- ✅ 表单分组/分步（可选）
- ✅ 自动保存草稿（可选）
- ✅ 提交确认
- ✅ 错误提示

#### 交互规则
- 必填字段标红星
- 实时验证反馈
- 提交前整体验证
- 提交成功跳转/提示

### 组件参数

```javascript
{
  // 表单配置
  formGroups: [
    {
      title: String,           // 分组标题
      fields: [
        {
          label: String,       // 字段标签
          field: String,       // 字段名
          type: 'input|select|textarea|date|upload|radio',
          required: Boolean,   // 是否必填
          placeholder: String, // 占位文本
          options: Array,      // 下拉/单选选项
          rules: Array,        // 验证规则
          span: Number,        // 栅格占比
          disabled: Boolean    // 是否禁用
        }
      ]
    }
  ],
  
  // 数据绑定
  modelValue: Object,          // 表单数据
  
  // 提交配置
  onSubmit: Function,          // 提交回调
  submitText: String,          // 提交按钮文字
  showCancel: Boolean,         // 是否显示取消按钮
  onCancel: Function           // 取消回调
}
```

### 使用示例

**场景**: 商户编辑页

```javascript
<jmt-form-edit-component
  :form-groups="[
    {
      title: '基本信息',
      fields: [
        { 
          label: '商户名称', 
          field: 'name', 
          type: 'input',
          required: true,
          span: 2
        },
        { 
          label: '商户类型', 
          field: 'type', 
          type: 'select',
          required: true,
          options: [
            { label: '企业', value: 'company' },
            { label: '个体工商户', value: 'individual' }
          ],
          span: 1
        }
      ]
    }
  ]"
  v-model="formData"
  :on-submit="handleSubmit"
  submit-text="保存"
  :show-cancel="true"
  :on-cancel="handleCancel"
/>
```

### 样式规范

```css
.form-edit-component {
  max-width: 1200px;
  margin: 0 auto;
}

.form-card {
  background: #fff;
  border-radius: 8px;
  padding: 24px;
  margin-bottom: 16px;
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}

.form-item {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-label {
  font-size: 14px;
  color: #666;
}

.form-label.required::before {
  content: '*';
  color: #ff4d4f;
  margin-right: 4px;
}

.form-input,
.form-select {
  height: 40px;
  padding: 0 12px;
  border: 1px solid #d9d9d9;
  border-radius: 4px;
  font-size: 14px;
  width: 100%;
}

.form-input:focus,
.form-select:focus {
  outline: none;
  border-color: #1890FF;
  box-shadow: 0 0 0 2px rgba(24,144,255,0.1);
}
```

---

## 进度时间线组件 (Progress Timeline Component)

### 业务需求

提供统一的流程进度展示能力，支持纵向时间线布局（符合用户偏好）。

### 功能特性

#### 核心功能
- ✅ 纵向时间线布局
- ✅ 步骤状态展示（已完成/进行中/未开始）
- ✅ 每步骤标题、描述、时间
- ✅ 自定义图标/颜色
- ✅ 响应式适配

#### 交互规则
- 已完成步骤显示绿色✓
- 当前步骤显示蓝色⏳
- 未开始步骤显示灰色数字
- 步骤可点击查看详情（可选）

### 组件参数

```javascript
{
  // 步骤配置
  steps: [
    {
      title: String,           // 步骤标题
      description: String,     // 步骤描述
      time: String,            // 时间戳
      status: 'completed|current|pending', // 步骤状态
      icon: String,            // 自定义图标（可选）
      details: Object          // 详细信息（可选）
    }
  ],
  
  // UI配置
  clickable: Boolean,          // 步骤是否可点击
  onStepClick: Function        // 步骤点击回调
}
```

### 使用示例

**场景**: 主体登记进度页

```javascript
<jmt-timeline-component
  :steps="[
    {
      title: '材料填报',
      description: '商户已提交主体登记材料',
      time: '2025-10-25 10:30',
      status: 'completed'
    },
    {
      title: '材料初审',
      description: '等待平台审核材料',
      time: '预计: 2025-10-26 18:00',
      status: 'current'
    },
    {
      title: '现场核验',
      description: '材料审核通过后进行现场核验',
      time: '',
      status: 'pending'
    },
    {
      title: '核准通过',
      description: '完成所有流程,主体登记成功',
      time: '',
      status: 'pending'
    }
  ]"
/>
```

### 样式规范

```css
/* 遵循用户偏好：纵向布局 */
.timeline {
  position: relative;
  padding-left: 32px;
}

/* 连接线 */
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

.timeline-item:last-child {
  padding-bottom: 0;
}

/* 步骤圆点 */
.timeline-dot {
  position: absolute;
  left: -32px;
  top: 0;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: #fff;
  border: 2px solid #e8e8e8;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  z-index: 1;
}

/* 已完成步骤 */
.timeline-item.completed .timeline-dot {
  background: #52c41a;
  border-color: #52c41a;
  color: white;
}

/* 当前步骤 */
.timeline-item.current .timeline-dot {
  background: #1890ff;
  border-color: #1890ff;
  color: white;
}

.timeline-title {
  font-size: 15px;
  font-weight: 600;
  margin-bottom: 6px;
}

.timeline-desc {
  font-size: 13px;
  color: #666;
  margin-bottom: 4px;
}

.timeline-time {
  font-size: 12px;
  color: #999;
}
```

---

## 审核操作组件 (Audit Action Component)

### 业务需求

提供统一的审核操作界面，支持通过/驳回选择、驳回原因填写等（遵循用户偏好：平铺 radio button）。

### 功能特性

#### 核心功能
- ✅ 平铺单选按钮（通过/驳回）
- ✅ 卡片式设计，可点击整个区域
- ✅ 驳回原因输入框（条件显示）
- ✅ 审核备注（可选）
- ✅ 提交确认

#### 交互规则
- 选择"驳回"时显示原因输入框
- 驳回原因必填验证
- 提交前二次确认
- 桌面端横向排列，移动端纵向排列（遵循用户偏好）

### 组件参数

```javascript
{
  // 审核配置
  options: [
    {
      label: String,           // 选项标签
      value: String,           // 选项值
      icon: String,            // 图标（可选）
      requireReason: Boolean   // 是否需要填写原因
    }
  ],
  
  // 数据绑定
  modelValue: Object,          // { result: String, reason: String, note: String }
  
  // 提交配置
  onSubmit: Function,          // 提交回调
  submitText: String,          // 提交按钮文字
  showNote: Boolean,           // 是否显示备注输入
  
  // UI配置
  layout: 'horizontal|vertical' // 布局方向（响应式自动切换）
}
```

### 使用示例

**场景**: 补充信息审核页

```javascript
<jmt-audit-action-component
  :options="[
    { 
      label: '✓ 审核通过', 
      value: 'approved',
      icon: '✓',
      requireReason: false 
    },
    { 
      label: '✗ 驳回申请', 
      value: 'rejected',
      icon: '✗',
      requireReason: true 
    }
  ]"
  v-model="auditData"
  :on-submit="handleAudit"
  submit-text="提交审核"
  :show-note="true"
/>
```

### 样式规范

```css
/* 遵循用户偏好：平铺 radio button，卡片式设计 */

/* 桌面端 - 横向排列 */
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

.radio-option input[type="radio"] {
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.radio-option.selected {
  border-color: #1890FF;
  background: #e6f7ff;
}

.radio-label {
  font-size: 14px;
  font-weight: 500;
  color: #333;
  flex: 1;
}

/* 移动端 - 纵向排列 */
@media (max-width: 768px) {
  .radio-group {
    flex-direction: column;
    gap: 12px;
  }
}
```

---

## 状态徽章组件 (Status Badge Component)

### 业务需求

提供统一的状态标签展示，支持多种预设状态类型和自定义颜色。

### 功能特性

#### 核心功能
- ✅ 预设状态类型（待审核/审核中/已通过/已驳回/正常/冻结等）
- ✅ 自定义状态文字和颜色
- ✅ 图标支持（可选）
- ✅ 尺寸变体（small/default/large）

### 组件参数

```javascript
{
  // 状态配置
  status: String,              // 状态类型（预设）
  text: String,                // 状态文字（自定义）
  color: String,               // 自定义颜色
  
  // UI配置
  icon: String,                // 图标（可选）
  size: 'small|default|large'  // 尺寸
}
```

### 预设状态类型

| 状态类型 | 显示文字 | 颜色 | 使用场景 |
|---------|---------|------|---------|
| `pending` | 待审核 | 橙色 `#fa8c16` | 审核类业务 |
| `reviewing` | 审核中 | 蓝色 `#1890ff` | 审核类业务 |
| `approved` | 已通过 | 绿色 `#52c41a` | 审核类业务 |
| `rejected` | 已驳回 | 红色 `#ff4d4f` | 审核类业务 |
| `active` | 正常 | 绿色 `#52c41a` | 商户状态 |
| `frozen` | 冻结 | 橙色 `#fa8c16` | 商户状态 |
| `inactive` | 已注销 | 灰色 `#999` | 商户状态 |

### 使用示例

```javascript
<!-- 预设状态 -->
<jmt-status-badge status="approved" />
<!-- 输出: <span class="status-badge status-approved">已通过</span> -->

<!-- 自定义状态 -->
<jmt-status-badge text="处理中" color="#1890ff" icon="⏳" />
```

### 样式规范

```css
.status-badge {
  display: inline-block;
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: 500;
}

/* 预设状态样式 */
.status-pending {
  background: #fff7e6;
  color: #fa8c16;
}

.status-reviewing {
  background: #e6f7ff;
  color: #1890ff;
}

.status-approved {
  background: #f6ffed;
  color: #52c41a;
}

.status-rejected {
  background: #fff1f0;
  color: #ff4d4f;
}

.status-active {
  background: #f6ffed;
  color: #52c41a;
}

.status-frozen {
  background: #fff7e6;
  color: #fa8c16;
}

.status-inactive {
  background: #f5f5f5;
  color: #999;
}

/* 尺寸变体 */
.status-badge.small {
  padding: 1px 6px;
  font-size: 11px;
}

.status-badge.large {
  padding: 4px 12px;
  font-size: 14px;
}
```

---

## 数据卡片组件 (Data Card Component)

### 业务需求

提供统一的数据统计卡片，支持数字展示、趋势图标、跳转链接等。

### 功能特性

#### 核心功能
- ✅ 大号数字展示
- ✅ 标签文字
- ✅ 趋势指示（上升/下降/持平）
- ✅ 自定义颜色
- ✅ 点击跳转（可选）
- ✅ 加载骨架屏

### 组件参数

```javascript
{
  // 数据配置
  value: Number|String,        // 主数据
  label: String,               // 标签文字
  
  // 趋势配置
  trend: {
    type: 'up|down|flat',      // 趋势类型
    value: String,             // 趋势值
    text: String               // 趋势说明
  },
  
  // UI配置
  color: String,               // 主色调
  icon: String,                // 图标（可选）
  clickable: Boolean,          // 是否可点击
  href: String,                // 跳转链接
  
  // 状态
  loading: Boolean             // 加载状态
}
```

### 使用示例

**场景**: 数据概览页

```javascript
<jmt-data-card-component
  value="127"
  label="商户总数"
  color="#1890ff"
  icon="🏢"
  :trend="{ type: 'up', value: '+12', text: '较上月' }"
  clickable
  href="merchant-list.html"
/>
```

### 样式规范

```css
.data-card {
  background: #fff;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.06);
  cursor: pointer;
  transition: all 0.3s;
}

.data-card:hover {
  box-shadow: 0 4px 16px rgba(0,0,0,0.12);
  transform: translateY(-2px);
}

.card-value {
  font-size: 28px;
  font-weight: 600;
  margin-bottom: 8px;
}

.card-label {
  font-size: 14px;
  color: #666;
}

.card-trend {
  font-size: 12px;
  margin-top: 8px;
}

.trend-up {
  color: #52c41a;
}

.trend-down {
  color: #ff4d4f;
}

.trend-flat {
  color: #999;
}
```

---

## 文件上传组件 (File Upload Component)

### 业务需求

提供统一的文件上传能力，支持拖拽、多文件、图片预览等。

### 功能特性

#### 核心功能
- ✅ 点击选择文件
- ✅ 拖拽上传
- ✅ 多文件上传
- ✅ 文件类型限制
- ✅ 文件大小限制
- ✅ 上传进度显示
- ✅ 图片预览
- ✅ 文件列表管理（删除）

### 组件参数

```javascript
{
  // 上传配置
  action: String,              // 上传接口地址
  accept: String,              // 接受的文件类型
  maxSize: Number,             // 最大文件大小(MB)
  maxCount: Number,            // 最大文件数量
  multiple: Boolean,           // 是否支持多文件
  
  // 数据绑定
  modelValue: Array,           // 文件列表
  
  // 回调配置
  onUpload: Function,          // 上传回调
  onRemove: Function,          // 删除回调
  onPreview: Function,         // 预览回调
  
  // UI配置
  listType: 'text|picture|picture-card', // 列表类型
  showProgress: Boolean        // 显示上传进度
}
```

### 使用示例

```javascript
<jmt-file-upload-component
  accept=".jpg,.png,.pdf"
  :max-size="10"
  :max-count="5"
  multiple
  v-model="fileList"
  list-type="picture-card"
  :on-upload="handleUpload"
/>
```

### 样式规范

```css
.upload-area {
  border: 2px dashed #d9d9d9;
  border-radius: 8px;
  padding: 40px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s;
}

.upload-area:hover {
  border-color: #1890FF;
  background: #fafafa;
}

.upload-icon {
  font-size: 48px;
  color: #d9d9d9;
  margin-bottom: 12px;
}

.upload-text {
  font-size: 14px;
  color: #666;
}

.file-list {
  margin-top: 16px;
}

.file-item {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  border: 1px solid #e8e8e8;
  border-radius: 4px;
  margin-bottom: 8px;
}

.file-name {
  flex: 1;
  font-size: 14px;
  color: #333;
}

.file-actions {
  display: flex;
  gap: 8px;
}
```

---

## 分页组件 (Pagination Component)

### 业务需求

提供统一的分页导航，支持页码跳转、每页条数切换等。

### 功能特性

#### 核心功能
- ✅ 上一页/下一页
- ✅ 页码列表
- ✅ 快速跳转
- ✅ 每页条数切换
- ✅ 总条数显示
- ✅ 简洁模式（移动端）

### 组件参数

```javascript
{
  // 分页配置
  current: Number,             // 当前页码
  pageSize: Number,            // 每页条数
  total: Number,               // 总条数
  
  // 回调
  onChange: Function,          // 页码变化回调
  onShowSizeChange: Function,  // 每页条数变化回调
  
  // UI配置
  showTotal: Boolean,          // 显示总条数
  showSizeChanger: Boolean,    // 显示每页条数切换器
  pageSizeOptions: Array,      // 每页条数选项
  simple: Boolean              // 简洁模式（移动端）
}
```

### 使用示例

```javascript
<jmt-pagination-component
  :current="1"
  :page-size="20"
  :total="127"
  :show-total="true"
  :show-size-changer="true"
  :page-size-options="[10, 20, 50, 100]"
  @change="handlePageChange"
/>
```

### 样式规范

```css
.pagination {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding: 16px 24px;
  gap: 8px;
}

.pagination-total {
  padding: 4px 12px;
  color: #666;
  font-size: 14px;
}

.page-btn {
  padding: 4px 12px;
  border: 1px solid #d9d9d9;
  background: #fff;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.3s;
}

.page-btn:hover {
  border-color: #1890FF;
  color: #1890FF;
}

.page-btn.active {
  background: #1890FF;
  color: #fff;
  border-color: #1890FF;
}

.page-btn:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

/* 移动端简洁模式 */
@media (max-width: 768px) {
  .pagination.simple {
    justify-content: center;
  }
  
  .pagination.simple .page-btn:not(.prev):not(.next):not(.active) {
    display: none;
  }
}
```

---

## 技术实现方案

### 技术选型

#### 前端组件框架
**推荐**: Web Components（原生）

**优势**:
- ✅ 无框架依赖，可在任何项目中使用
- ✅ 原生浏览器支持，性能优秀
- ✅ 封装性强，样式隔离
- ✅ 符合当前项目纯HTML架构

**示例**:
```javascript
class JmtListComponent extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
  }
  
  connectedCallback() {
    this.render();
  }
  
  render() {
    this.shadowRoot.innerHTML = `
      <style>
        /* 组件样式 */
      </style>
      <div class="list-component">
        <!-- 组件模板 -->
      </div>
    `;
  }
}

customElements.define('jmt-list-component', JmtListComponent);
```

#### 备选方案
如需引入框架，推荐：
- **Vue 3**: 组件化开发体验好，生态成熟
- **React**: 社区强大，组件库丰富
- **Svelte**: 编译时优化，体积小

### 后端API设计

#### RESTful API 规范

```javascript
// 列表查询
GET /api/{resource}?page=1&pageSize=20&status=active

// 详情查询
GET /api/{resource}/{id}

// 创建
POST /api/{resource}

// 更新
PUT /api/{resource}/{id}

// 删除
DELETE /api/{resource}/{id}

// 审核操作
POST /api/{resource}/{id}/audit
{
  "result": "approved|rejected",
  "reason": "驳回原因",
  "note": "备注"
}
```

#### 统一响应格式

```javascript
{
  "code": 200,              // 状态码
  "message": "success",     // 消息
  "data": {                 // 数据
    "list": [],             // 列表数据
    "total": 127,           // 总条数
    "current": 1,           // 当前页
    "pageSize": 20          // 每页条数
  }
}
```

### 开发计划

#### Phase 1: 基础组件开发（2周）
- Week 1: 状态徽章、数据卡片、分页组件
- Week 2: 文件上传、进度时间线组件

#### Phase 2: 复杂组件开发（3周）
- Week 1: 列表组件
- Week 2: 详情组件、表单编辑组件
- Week 3: 审核操作组件

#### Phase 3: 集成与测试（2周）
- Week 1: 组件库文档、Storybook 示例
- Week 2: 现有页面迁移、测试优化

#### Phase 4: 推广与维护（持续）
- 组件使用培训
- 收集反馈优化
- 版本迭代

### 目录结构设计

```
jmt/
├── frontend/
│   ├── components/              # 组件库
│   │   ├── jmt-list/           # 列表组件
│   │   │   ├── index.js
│   │   │   ├── index.css
│   │   │   └── README.md
│   │   ├── jmt-detail/         # 详情组件
│   │   ├── jmt-form-edit/      # 表单编辑组件
│   │   ├── jmt-timeline/       # 时间线组件
│   │   ├── jmt-audit-action/   # 审核操作组件
│   │   ├── jmt-status-badge/   # 状态徽章组件
│   │   ├── jmt-data-card/      # 数据卡片组件
│   │   ├── jmt-file-upload/    # 文件上传组件
│   │   └── jmt-pagination/     # 分页组件
│   ├── lib/                    # 组件库打包文件
│   │   ├── jmt-components.js
│   │   └── jmt-components.css
│   └── pages/                  # 业务页面（原有页面）
├── docs/                       # 组件文档
│   └── components/
│       ├── list.md
│       ├── detail.md
│       └── ...
└── examples/                   # 示例代码
    └── ...
```

---

## 📊 组件成熟度评估

| 组件      | 优先级 | 复杂度 | 预计工时 | 复用频率 |
| ------- | --- | --- | ---- | ---- |
| 状态徽章组件  | P0  | 低   | 1天   | 极高   |
| 分页组件    | P0  | 低   | 1天   | 极高   |
| 数据卡片组件  | P0  | 低   | 2天   | 高    |
| 进度时间线组件 | P1  | 中   | 3天   | 高    |
| 审核操作组件  | P1  | 中   | 3天   | 中    |
| 文件上传组件  | P1  | 中   | 4天   | 中    |
| 列表组件    | P2  | 高   | 5天   | 极高   |
| 详情组件    | P2  | 高   | 5天   | 极高   |
| 表单编辑组件  | P2  | 高   | 6天   | 极高   |

**总工时预估**: 30人天（约6周，1人全职开发）


---

## 附录

### 参考资料

- [Web Components MDN](https://developer.mozilla.org/zh-CN/docs/Web/Web_Components)
- [Ant Design 组件库](https://ant.design/components/overview-cn)
- [Element Plus 组件库](https://element-plus.org/zh-CN/component/overview.html)

### 联系方式

如有问题或建议，请联系开发团队。

---

> **文档维护**: 本文档需随着组件开发进度定期更新
> **版本历史**: v1.0 - 2025-11-03 - 初始版本
