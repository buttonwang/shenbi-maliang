# 现代企业级 Dashboard 布局参考

> 本文档定义了业务助手智能体生成网页应用的标准布局模板。
> 基于 Tailwind CSS 设计体系 + Manrope 字体。
> 2026年最新版本（v3.0）

---

## 一、基础配置

### 1.1 必需资源

```html
<!-- Manrope 字体 -->
<link href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700&display=swap" rel="stylesheet">

<!-- Tailwind CSS -->
<script src="https://cdn.tailwindcss.com"></script>

<script>
tailwind.config = {
  theme: {
    extend: {
      fontFamily: { sans: ['Manrope', 'system-ui', 'sans-serif'] }
    }
  }
}
</script>
```

### 1.2 设计原则

1. **CSS 变量主题系统** — 支持深浅色切换
2. **Manrope 字体** — 现代几何无衬线字体
3. **左侧蓝色边框指示器** — 替代纯背景色，更精致
4. **大圆角** — 使用 `rounded-xl` / `rounded-2xl`
5. **多层阴影** — 悬停时有上浮 + 阴影加深效果
6. **语义化状态** — 状态徽章使用语义色

---

## 二、CSS 变量主题系统

### 2.1 完整 CSS 样式

```css
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #f8fafc;
  --bg-tertiary: #f1f5f9;
  --text-primary: #0f172a;
  --text-secondary: #334155;
  --text-muted: #64748b;
  --border-color: #e2e8f0;
  --accent-primary: #3b82f6;
  --accent-secondary: #2563eb;
  --accent-bg: #eff6ff;
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -4px rgba(0, 0, 0, 0.1);
}

[data-theme="dark"] {
  --bg-primary: #0f172a;
  --bg-secondary: #1e293b;
  --bg-tertiary: #334155;
  --text-primary: #f8fafc;
  --text-secondary: #e2e8f0;
  --text-muted: #94a3b8;
  --border-color: #334155;
  --accent-primary: #60a5fa;
  --accent-secondary: #3b82f6;
  --accent-bg: #1e3a5f;
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.3);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.4), 0 2px 4px -2px rgba(0, 0, 0, 0.3);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.4), 0 4px 6px -4px rgba(0, 0, 0, 0.3);
}

* {
  font-family: 'Manrope', system-ui, sans-serif;
}

body, aside, header, main, .card, .stat-card, .sidebar-nav-item, input, select, textarea {
  transition: background-color 0.3s ease, border-color 0.3s ease, color 0.3s ease;
}
```

### 2.2 主题切换 JavaScript

```javascript
function initTheme() {
  const savedTheme = localStorage.getItem('ba_theme') || 'light';
  document.documentElement.setAttribute('data-theme', savedTheme);
}

function toggleTheme() {
  const current = document.documentElement.getAttribute('data-theme');
  const newTheme = current === 'dark' ? 'light' : 'dark';
  document.documentElement.setAttribute('data-theme', newTheme);
  localStorage.setItem('ba_theme', newTheme);
}
```

---

## 三、侧边栏 (Sidebar) 组件

### 3.1 完整侧边栏 HTML

```html
<!-- 移动端遮罩层 -->
<div id="mobile-overlay" class="fixed inset-0 z-30 bg-black/40 hidden lg:hidden" onclick="toggleSidebar()"></div>

<!-- 侧边栏容器 -->
<aside id="sidebar" class="fixed inset-y-0 left-0 z-40 w-[260px] 
     bg-gradient-to-b from-slate-50 via-white to-slate-50 
     border-r border-slate-200/60 transform transition-all duration-300
     lg:translate-x-0 -translate-x-full shadow-2xl shadow-slate-900/5">
  
  <!-- Logo 区域 -->
  <div class="relative h-[68px] flex items-center px-5 border-b border-slate-200/50 bg-white/60">
    <a href="#" class="flex items-center gap-3.5 group">
      <div class="w-10 h-10 rounded-2xl bg-gradient-to-br from-blue-600 to-indigo-600 
                  flex items-center justify-center shadow-lg shadow-blue-500/25
                  group-hover:shadow-xl group-hover:scale-105 transition-all">
        <svg class="w-5 h-5 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/>
        </svg>
      </div>
      <div class="flex flex-col">
        <span class="text-[15px] font-bold text-slate-900 leading-tight tracking-tight">应用名称</span>
        <span class="text-[10px] text-slate-400 font-medium">App Name</span>
      </div>
    </a>
  </div>

  <!-- 导航菜单 -->
  <nav class="flex-1 px-3 pt-4 pb-2 overflow-y-auto" style="max-height: calc(100vh - 68px - 73px);">
    
    <!-- 分组 -->
    <div class="mb-6">
      <div class="flex items-center gap-2 px-3 mb-2">
        <span class="text-[11px] font-semibold text-slate-400 uppercase tracking-wider">主导航</span>
        <div class="flex-1 h-px bg-slate-200"></div>
      </div>
      
      <!-- 菜单项 -->
      <a onclick="navigateTo('dashboard')" id="nav-dashboard" class="sidebar-nav-item active">
        <span class="nav-icon">
          <svg fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6z"/>
          </svg>
        </span>
        <span class="nav-text">仪表盘</span>
        <span class="nav-badge">首页</span>
      </a>

      <a onclick="navigateTo('list')" id="nav-list" class="sidebar-nav-item">
        <span class="nav-icon">
          <svg fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M4 6h16M4 10h16M4 14h16M4 18h16"/>
          </svg>
        </span>
        <span class="nav-text">列表页</span>
      </a>
    </div>

    <!-- 更多分组... -->
  </nav>

  <!-- 底部用户信息 -->
  <div class="p-3 border-t border-slate-200/50">
    <div class="flex items-center gap-3 p-2.5 rounded-xl hover:bg-slate-100/80 transition-all cursor-pointer group">
      <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-blue-500 to-indigo-600 
                  flex items-center justify-center shadow-md shadow-blue-500/20 flex-shrink-0
                  group-hover:shadow-lg group-hover:scale-105 transition-all">
        <span class="text-sm font-bold text-white">张</span>
      </div>
      <div class="flex-1 min-w-0">
        <p class="text-sm font-semibold text-slate-900 truncate">用户名</p>
        <p class="text-xs text-slate-500 truncate">角色 ·状态</p>
      </div>
    </div>
  </div>
</aside>
```

### 3.2 侧边栏 CSS

```css
/* 侧边栏样式 */
aside {
  background: var(--bg-primary) !important;
  border-color: var(--border-color) !important;
}

/* 菜单项 - 确保一行显示 */
.sidebar-nav-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  width: 100%;
  padding: 10px 12px;
  border-radius: 10px;
  font-size: 14px;
  font-weight: 500;
  color: var(--text-muted);
  transition: all 0.2s ease;
  cursor: pointer;
  position: relative;
  white-space: nowrap;
  overflow: hidden;
  flex-shrink: 0;
}

.sidebar-nav-item .nav-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}
.sidebar-nav-item .nav-icon svg {
  width: 20px;
  height: 20px;
}

.sidebar-nav-item .nav-text {
  flex: 1;
  margin-left: 12px;
  overflow: hidden;
  text-overflow: ellipsis;
  color: var(--text-secondary);
}

.sidebar-nav-item:hover {
  background: var(--bg-tertiary);
  color: var(--text-primary);
}
.sidebar-nav-item:hover .nav-text {
  color: var(--text-primary);
}

/* 激活状态 */
.sidebar-nav-item.active {
  background: var(--accent-bg) !important;
  color: var(--accent-primary) !important;
}
.sidebar-nav-item.active .nav-text {
  color: var(--accent-primary) !important;
}
.sidebar-nav-item.active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 3px;
  height: 20px;
  background: var(--accent-primary);
  border-radius: 0 3px 3px 0;
}

/* 徽章 */
.sidebar-nav-item .nav-badge {
  flex-shrink: 0;
  padding: 2px 8px;
  font-size: 10px;
  font-weight: 600;
  background: var(--accent-bg);
  color: var(--accent-primary);
  border-radius: 9999px;
  margin-left: 8px;
}
```

---

## 四、顶部导航 (Header)

```html
<header class="sticky top-0 z-20 h-[68px] bg-white/95 backdrop-blur-xl border-b border-slate-200/60 shadow-sm">
  <div class="flex items-center justify-between h-full px-5 lg:px-6">
    
    <!-- 左侧 -->
    <div class="flex items-center gap-4">
      <button onclick="toggleSidebar()" class="lg:hidden p-2.5 rounded-xl hover:bg-slate-100 text-slate-600">
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/>
        </svg>
      </button>
      <nav class="hidden sm:flex items-center gap-2.5 text-sm">
        <a href="#" class="text-slate-500 hover:text-slate-700">首页</a>
        <svg class="w-4 h-4 text-slate-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/>
        </svg>
        <span id="breadcrumb-current" class="text-slate-900 font-semibold">当前页</span>
      </nav>
    </div>

    <!-- 右侧 -->
    <div class="flex items-center gap-2.5">
      <!-- 搜索框 -->
      <div class="hidden md:block relative">
        <input type="text" placeholder="搜索..." 
               class="w-56 lg:w-64 pl-10 pr-4 py-2.5 text-sm border border-slate-200 rounded-2xl 
                      bg-slate-50/80 focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20">
        <svg class="absolute left-3.5 top-1/2 -translate-y-1/2 w-4 h-4 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/>
        </svg>
      </div>

      <!-- 通知 -->
      <button class="relative p-2.5 rounded-xl hover:bg-slate-100 text-slate-600">
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"/>
        </svg>
        <span class="absolute top-1.5 right-1.5 w-2 h-2 bg-red-500 rounded-full border-2 border-white"></span>
      </button>
    </div>
  </div>
</header>
```

---

## 五、卡片组件

### 5.1 基础卡片

```css
.card {
  background: var(--bg-primary);
  border: 1px solid var(--border-color);
  border-radius: 16px;
  transition: all 0.3s ease;
}
.card:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-lg);
}
```

### 5.2 统计卡片

```css
.stat-card {
  background: var(--bg-primary);
  border: 1px solid var(--border-color);
  border-radius: 16px;
  padding: 20px;
  transition: all 0.3s ease;
}
.stat-card:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-lg);
  border-color: var(--accent-primary);
}
.stat-card .stat-icon {
  width: 48px;
  height: 48px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

---

## 六、按钮样式

```css
.btn-primary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 10px 18px;
  background: var(--accent-primary);
  color: #ffffff;
  font-size: 14px;
  font-weight: 600;
  border-radius: 10px;
  transition: all 0.2s ease;
  box-shadow: 0 2px 8px -2px rgba(59, 130, 246, 0.4);
}
.btn-primary:hover {
  background: var(--accent-secondary);
  transform: translateY(-1px);
  box-shadow: 0 4px 12px -2px rgba(59, 130, 246, 0.5);
}

.btn-secondary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 10px 18px;
  background: var(--bg-tertiary);
  color: var(--text-secondary);
  font-size: 14px;
  font-weight: 500;
  border-radius: 10px;
  border: 1px solid var(--border-color);
  transition: all 0.2s ease;
}
.btn-secondary:hover {
  background: var(--border-color);
  transform: translateY(-1px);
}

.btn-danger {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 10px 18px;
  background: #ef4444;
  color: #ffffff;
  font-size: 14px;
  font-weight: 600;
  border-radius: 10px;
  transition: all 0.2s ease;
}
.btn-danger:hover {
  background: #dc2626;
  transform: translateY(-1px);
}

.btn-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 8px;
  border-radius: 8px;
  color: var(--text-muted);
  transition: all 0.2s ease;
}
.btn-icon:hover {
  background: var(--bg-tertiary);
  color: var(--text-primary);
}
```

---

## 七、表单输入框

```css
.input-field {
  width: 100%;
  padding: 10px 14px;
  font-size: 14px;
  border: 1px solid var(--border-color);
  border-radius: 10px;
  background: var(--bg-primary);
  color: var(--text-primary);
  transition: all 0.2s ease;
}
.input-field::placeholder {
  color: var(--text-muted);
}
.input-field:focus {
  outline: none;
  border-color: var(--accent-primary);
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.15);
}
```

---

## 八、表格样式

```css
.data-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
}
.data-table th {
  padding: 12px 16px;
  text-align: left;
  font-size: 11px;
  font-weight: 600;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  background: var(--bg-secondary);
  border-bottom: 1px solid var(--border-color);
}
.data-table td {
  padding: 14px 16px;
  font-size: 14px;
  color: var(--text-secondary);
  border-bottom: 1px solid var(--border-color);
}
.data-table tbody tr:hover {
  background: var(--bg-secondary);
}
```

---

## 九、状态徽章

```css
.badge {
  display: inline-flex;
  align-items: center;
  padding: 4px 10px;
  border-radius: 9999px;
  font-size: 11px;
  font-weight: 600;
}
.badge-pending { background: #fef3c7; color: #b45309; }
.badge-following { background: var(--accent-bg); color: var(--accent-primary); }
.badge-dealed { background: #d1fae5; color: #059669; }
.badge-abandoned { background: var(--bg-tertiary); color: var(--text-muted); }
```

---

## 十、弹窗/模态框

```html
<div id="dialog" class="fixed inset-0 z-50 hidden">
  <!-- 遮罩层 -->
  <div class="absolute inset-0 bg-black/70"></div>
  <!-- 弹窗内容 -->
  <div class="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 w-full max-w-lg"
       style="animation: modalSlideUp 0.2s ease-out;">
    <div class="overflow-hidden" style="background: var(--bg-primary); border: 1px solid var(--border-color); border-radius: 20px; box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.4);">
      <!-- 标题栏 -->
      <div class="flex items-center justify-between px-5 py-4" style="border-bottom: 1px solid var(--border-color);">
        <h2 class="text-lg font-semibold" style="color: var(--text-primary);">标题</h2>
        <button onclick="closeDialog()" class="p-2 rounded-lg hover:bg-gray-100" style="color: var(--text-muted);">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
          </svg>
        </button>
      </div>
      <!-- 内容区 -->
      <div class="p-5">
        <!-- 表单内容 -->
      </div>
    </div>
  </div>
</div>
```

```css
@keyframes modalSlideUp {
  from { opacity: 0; transform: translate(-50%, -48%) scale(0.96); }
  to { opacity: 1; transform: translate(-50%, -50%) scale(1); }
}
```

---

## 十一、主题切换开关

```html
<button onclick="toggleTheme()" id="theme-toggle-btn" class="theme-toggle" title="切换主题"></button>
```

```css
.theme-toggle {
  position: relative;
  width: 44px;
  height: 24px;
  background: var(--bg-tertiary);
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
  border: 1px solid var(--border-color);
}
.theme-toggle::before {
  content: '';
  position: absolute;
  top: 2px;
  left: 2px;
  width: 18px;
  height: 18px;
  background: var(--accent-primary);
  border-radius: 50%;
  transition: all 0.3s ease;
  box-shadow: 0 2px 4px rgba(0,0,0,0.2);
}
.theme-toggle.active {
  background: var(--accent-primary);
  border-color: var(--accent-primary);
}
.theme-toggle.active::before {
  left: 22px;
  background: white;
}
```

---

## 十二、布局尺寸规范

| 元素 | 桌面端 | 移动端 |
|------|--------|--------|
| 侧边栏宽度 | 260px (w-[260px]) | 隐藏（抽屉式） |
| 主内容区内边距 | 24px (p-6) | 16px (p-4) |
| 顶部导航高度 | 68px (h-[68px]) | 68px |
| 卡片圆角 | 16px (rounded-2xl) | 12px (rounded-xl) |
| 按钮圆角 | 10px (rounded-xl) | - |
| 输入框圆角 | 10px | - |

---

## 十三、按钮组件变体

### 13.1 完整按钮系统

```html
<!-- 主要按钮 -->
<button class="btn-primary">主要按钮</button>

<!-- 次要按钮 -->
<button class="btn-secondary">次要按钮</button>

<!-- 危险按钮 -->
<button class="btn-danger">危险按钮</button>

<!-- 幽灵按钮 -->
<button class="btn-ghost">幽灵按钮</button>

<!-- 尺寸变体 -->
<button class="btn-primary btn-sm">小按钮</button>
<button class="btn-primary">默认</button>
<button class="btn-primary btn-lg">大按钮</button>

<!-- 纯图标按钮 -->
<button class="btn-icon-only" title="编辑">
  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z"/>
  </svg>
</button>
<button class="btn-icon-only primary" title="收藏">
  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
  </svg>
</button>
<button class="btn-icon-only danger" title="删除">
  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/>
  </svg>
</button>
```

### 13.2 CSS 样式

```css
/* 按钮基础样式 */
.btn-primary, .btn-secondary, .btn-danger, .btn-ghost {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 10px 18px;
  font-size: 14px;
  font-weight: 600;
  border-radius: 8px;
  transition: all 0.2s ease;
  cursor: pointer;
  border: none;
}

/* 尺寸变体 */
.btn-sm { padding: 6px 12px; font-size: 12px; }
.btn-lg { padding: 12px 24px; font-size: 16px; }

/* 纯图标按钮 */
.btn-icon-only {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  padding: 0;
  border-radius: 8px;
}
.btn-icon-only.primary { background: #dbeafe; color: #2563eb; }
.btn-icon-only.danger { background: #fee2e2; color: #dc2626; }

/* 暗色模式 */
.dark .btn-icon-only {
  background: #334155;
  color: #94a3b8;
}
```

---

## 十四、布局模板

### 14.1 表单页模板

```html
<div class="mb-6">
  <h1 class="text-2xl font-bold text-slate-900 dark:text-white">页面标题</h1>
  <p class="mt-1 text-sm text-slate-500 dark:text-slate-400">页面描述</p>
</div>

<div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
  <!-- 主表单区域 -->
  <div class="lg:col-span-2">
    <div class="bg-white dark:bg-slate-800 rounded-xl border border-slate-200 dark:border-slate-700 overflow-hidden">
      <div class="px-6 py-4 border-b border-slate-200 dark:border-slate-700">
        <h2 class="text-lg font-semibold text-slate-900 dark:text-white">表单标题</h2>
      </div>
      <form class="p-6 space-y-4">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="form-label">字段标签 <span class="required">*</span></label>
            <input type="text" class="input-field" placeholder="请输入..." required>
          </div>
          <div>
            <label class="form-label">下拉选择</label>
            <select class="input-field">
              <option>选项一</option>
              <option>选项二</option>
            </select>
          </div>
        </div>
        <div>
          <label class="form-label">备注</label>
          <textarea class="input-field" rows="4" placeholder="请输入..."></textarea>
        </div>
        <div class="flex justify-end gap-3 pt-4">
          <button type="button" class="btn-secondary">取消</button>
          <button type="submit" class="btn-primary">保存</button>
        </div>
      </form>
    </div>
  </div>
  
  <!-- 侧边信息 -->
  <div class="space-y-6">
    <div class="bg-white dark:bg-slate-800 rounded-xl border border-slate-200 dark:border-slate-700 p-5">
      <h3 class="text-base font-semibold text-slate-900 dark:text-white mb-4">提示信息</h3>
      <ul class="space-y-2 text-sm text-slate-500 dark:text-slate-400">
        <li>• 请确保填写所有必填项</li>
        <li>• 数据将在提交后自动保存</li>
        <li>• 如有问题请联系管理员</li>
      </ul>
    </div>
  </div>
</div>
```

### 14.2 列表页模板

```html
<div class="mb-6 flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
  <div>
    <h1 class="text-2xl font-bold text-slate-900 dark:text-white">列表标题</h1>
    <p class="mt-1 text-sm text-slate-500 dark:text-slate-400">列表描述</p>
  </div>
  <button class="btn-primary">
    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/>
    </svg>
    新建
  </button>
</div>

<!-- 筛选栏 -->
<div class="bg-white dark:bg-slate-800 rounded-xl border border-slate-200 dark:border-slate-700 overflow-hidden mb-6">
  <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4 p-4 border-b border-slate-200 dark:border-slate-700">
    <input type="text" placeholder="搜索..." class="input-field max-w-sm">
    <select class="input-field w-auto">
      <option value="all">全部</option>
      <option value="active">启用</option>
      <option value="disabled">禁用</option>
    </select>
  </div>
  
  <!-- 表格 -->
  <div class="table-responsive">
    <table class="w-full data-table">
      <thead>
        <tr>
          <th>名称</th>
          <th>状态</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <!-- 数据行 -->
      </tbody>
    </table>
  </div>
  
  <!-- 分页 -->
  <div class="flex items-center justify-between px-4 py-3 border-t border-slate-200 dark:border-slate-700 bg-slate-50 dark:bg-slate-800/50">
    <p class="text-sm text-slate-500 dark:text-slate-400">共 0 条记录</p>
    <div class="flex gap-2">
      <button class="btn-secondary btn-sm">上一页</button>
      <button class="btn-secondary btn-sm">下一页</button>
    </div>
  </div>
</div>
```

### 14.3 详情页模板

```html
<div class="mb-6 flex items-center gap-4">
  <button onclick="history.back()" class="btn-icon-only">
    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"/>
    </svg>
  </button>
  <div>
    <h1 class="text-2xl font-bold text-slate-900 dark:text-white">详情标题</h1>
    <p class="mt-1 text-sm text-slate-500 dark:text-slate-400">ID: #12345</p>
  </div>
</div>

<div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
  <!-- 主内容 -->
  <div class="lg:col-span-2 space-y-6">
    <!-- 基本信息 -->
    <div class="bg-white dark:bg-slate-800 rounded-xl border border-slate-200 dark:border-slate-700 overflow-hidden">
      <div class="flex items-center justify-between px-6 py-4 border-b border-slate-200 dark:border-slate-700">
        <h2 class="text-lg font-semibold text-slate-900 dark:text-white">基本信息</h2>
        <button class="btn-secondary btn-sm">编辑</button>
      </div>
      <div class="p-6">
        <dl class="grid grid-cols-2 gap-x-8 gap-y-4">
          <div>
            <dt class="text-sm text-slate-500 dark:text-slate-400">字段一</dt>
            <dd class="mt-1 text-sm font-medium text-slate-900 dark:text-white">值一</dd>
          </div>
          <div>
            <dt class="text-sm text-slate-500 dark:text-slate-400">字段二</dt>
            <dd class="mt-1 text-sm font-medium text-slate-900 dark:text-white">值二</dd>
          </div>
        </dl>
      </div>
    </div>
  </div>
  
  <!-- 侧边栏 -->
  <div class="space-y-6">
    <!-- 操作日志 -->
    <div class="bg-white dark:bg-slate-800 rounded-xl border border-slate-200 dark:border-slate-700 p-5">
      <h3 class="text-base font-semibold text-slate-900 dark:text-white mb-4">操作日志</h3>
      <div class="space-y-4">
        <div class="flex gap-3">
          <div class="w-2 h-2 rounded-full bg-emerald-500 mt-2"></div>
          <div>
            <p class="text-sm text-slate-900 dark:text-white">创建于 2024-01-15</p>
            <p class="text-xs text-slate-500 dark:text-slate-400">管理员</p>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 快捷操作 -->
    <div class="bg-white dark:bg-slate-800 rounded-xl border border-slate-200 dark:border-slate-700 p-5">
      <h3 class="text-base font-semibold text-slate-900 dark:text-white mb-4">快捷操作</h3>
      <div class="space-y-2">
        <button class="btn-secondary w-full justify-start">导出数据</button>
        <button class="btn-ghost w-full justify-start">打印</button>
      </div>
    </div>
  </div>
</div>
```

---

## 十五、移动端底部导航

### 15.1 HTML 结构

```html
<!-- 移动端底部导航（仅在小屏幕显示） -->
<nav class="fixed bottom-0 left-0 right-0 z-30 lg:hidden bg-white dark:bg-slate-900 border-t border-slate-200 dark:border-slate-700 shadow-lg">
  <div class="flex items-center justify-around px-2 py-1">
    <button onclick="navigateTo('dashboard')" class="mobile-nav-item active flex flex-col items-center py-2 px-4 rounded-lg" data-page="dashboard">
      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6z"/>
      </svg>
      <span class="text-[10px] mt-1 font-medium">首页</span>
    </button>
    <!-- 更多导航项... -->
  </div>
</nav>
```

### 15.2 CSS 样式

```css
/* 移动端底部导航样式 */
.mobile-nav-item {
  color: #64748b;
}
.mobile-nav-item:hover, .mobile-nav-item.active {
  color: #3b82f6;
}
.mobile-nav-item.active svg { color: #3b82f6; }
.dark .mobile-nav-item { color: #94a3b8; }
.dark .mobile-nav-item:hover, .dark .mobile-nav-item.active { color: #60a5fa; }
.dark .mobile-nav-item.active svg { color: #60a5fa; }

/* 主内容区域留出底部导航空间 */
main {
  padding-bottom: 80px; /* 为底部导航留空间 */
}
@media (min-width: 1024px) {
  main { padding-bottom: 24px; } /* 大屏幕恢复正常 */
}
```

### 15.3 JavaScript 切换逻辑

```javascript
function updateMobileNavState(page) {
  document.querySelectorAll('.mobile-nav-item').forEach(item => {
    item.classList.toggle('active', item.dataset.page === page);
  });
}

function navigateTo(page) {
  // ... 其他逻辑
  updateMobileNavState(page);
}
```

---

## 十六、响应式表格

### 16.1 横向滚动容器

```html
<div class="table-responsive">
  <table class="w-full data-table">
    <!-- 表格内容 -->
  </table>
</div>
```

### 16.2 CSS 样式

```css
.table-responsive {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch; /* iOS 平滑滚动 */
}
.table-responsive::-webkit-scrollbar {
  height: 6px;
}
.table-responsive::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 3px;
}
.dark .table-responsive::-webkit-scrollbar-thumb {
  background: #475569;
}
```

### 16.3 隐藏列策略

```html
<table class="w-full data-table">
  <thead>
    <tr>
      <th>姓名</th> <!-- 始终显示 -->
      <th class="hidden sm:table-cell">电话</th> <!-- sm 以上显示 -->
      <th class="hidden md:table-cell">邮箱</th> <!-- md 以上显示 -->
      <th class="hidden lg:table-cell">地址</th> <!-- lg 以上显示 -->
      <th>操作</th> <!-- 始终显示 -->
    </tr>
  </thead>
</table>
```

---

## 十七、Chart.js 图表集成

### 17.1 引入库

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
```

### 17.2 柱状图示例

```javascript
const ctx = document.getElementById('myChart');
new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['一月', '二月', '三月'],
    datasets: [{
      label: '销售额',
      data: [12500, 19200, 15800],
      backgroundColor: '#3b82f6',
      borderRadius: 6
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    plugins: { legend: { display: false } },
    scales: {
      y: { ticks: { color: '#64748b' }, grid: { color: '#e5e7eb' } },
      x: { ticks: { color: '#64748b' }, grid: { display: false } }
    }
  }
});
```

### 17.3 饼图/环形图示例

```javascript
new Chart(document.getElementById('pieChart'), {
  type: 'doughnut', // 或 'pie'
  data: {
    labels: ['类别A', '类别B', '类别C'],
    datasets: [{
      data: [35, 45, 20],
      backgroundColor: ['#3b82f6', '#10b981', '#f59e0b'],
      borderWidth: 0
    }]
  },
  options: {
    plugins: {
      legend: {
        position: 'right',
        labels: { color: '#64748b', padding: 15, usePointStyle: true }
      }
    }
  }
});
```

### 17.4 暗色模式支持

```javascript
function initCharts() {
  const isDark = document.documentElement.classList.contains('dark');
  const textColor = isDark ? '#94a3b8' : '#64748b';
  const gridColor = isDark ? '#334155' : '#e5e7eb';
  
  // 创建图表时使用主题颜色
  new Chart(ctx, {
    options: {
      scales: {
        y: { 
          ticks: { color: textColor },
          grid: { color: gridColor }
        },
        x: { 
          ticks: { color: textColor },
          grid: { display: false }
        }
      }
    }
  });
}

// 主题切换时重新初始化
function toggleTheme() {
  document.documentElement.classList.toggle('dark');
  // ... 其他逻辑
  initCharts(); // 重新渲染图表
}
```

---

## 十八、快速生成提示词示例

### 18.1 客户管理页面

```
生成一个客户管理页面，包含：
- 顶部标题和添加按钮
- 搜索框和状态筛选下拉
- 客户列表表格（姓名、电话、状态、操作列）
- 添加/编辑客户弹窗表单
- 删除确认弹窗
- 使用 LocalStorage 持久化数据
```

### 18.2 数据统计页面

```
生成一个数据统计页面，包含：
- 4个统计概览卡片（带图标和趋势指示）
- 柱状图展示月度趋势
- 饼图展示数据分布
- 折线图展示时间对比
- 数据表格展示明细
- 使用 Chart.js 绑定图表
- 支持暗色模式
```

### 18.3 系统设置页面

```
生成一个系统设置页面，包含：
- 个人信息编辑区（头像、姓名、邮箱等）
- 开关设置列表（深色模式、通知等）
- 按钮组件展示区
- 安全设置区
- 危险操作区（注销账户）
- 表单验证和提交处理
```

### 18.4 订单管理页面

```
生成一个订单管理页面，包含：
- 订单统计卡片（总数、待处理、已完成、总金额）
- 订单号、客户、金额、状态、日期、操作为列的表格
- 搜索和状态筛选
- 创建/编辑订单弹窗
- 删除确认弹窗
- LocalStorage 持久化
```
