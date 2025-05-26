# Cytoscape.js 介绍

## 关于 Cytoscape.js

Cytoscape.js 是一个用 JavaScript 编写的开源图论（网络）库，专门用于图形分析和可视化。
**官方文档**: [https://js.cytoscape.org/](https://js.cytoscape.org/) 
没有提供中文，这里是参考原文档编写的中文文档，为方便使用，还提供了一些代码示例
Cytoscape.js 允许您轻松显示和操作丰富的交互式图形。由于 Cytoscape.js 允许用户与图形交互，并且库允许客户端挂接到用户事件，因此 Cytoscape.js 很容易集成到您的应用程序中，特别是因为 Cytoscape.js 同时支持桌面浏览器（如 Chrome）和移动浏览器（如 iPad）。Cytoscape.js 包含您期望的所有开箱即用的手势，包括双指缩放、框选、平移等。

Cytoscape.js 还考虑了图形分析：该库包含图论中的许多有用函数。您可以在 Node.js 上无头使用 Cytoscape.js 在终端或 Web 服务器上进行图形分析。

## 主要特性

- **纯 JavaScript 编写**：完全功能的图形库
- **开源许可证**：核心 Cytoscape.js 库和所有第一方扩展均采用宽松的开源许可证（MIT）
- **生产级应用**：在商业项目和开源项目中广泛使用
- **用户友好设计**：既适用于面向用户的应用程序用例，也适用于开发人员用例
- **高度优化**：性能卓越
- **广泛兼容性**：
  - 所有现代浏览器
  - 支持 ES5 和 canvas 的旧版浏览器
  - 多种模块系统（ES modules、UMD、CommonJS/Node.js、AMD/Require.js）
  - 多种包管理器（npm、yarn、bower）

## 核心功能

### 🎨 可视化特性
- **样式表系统**：以渲染无关的方式将表示与数据分离
- **自动布局**：使用布局自动或手动定位节点
- **动画支持**：可动画化的图形元素和视口
- **选择器系统**：用于简洁过滤和图形查询的选择器

### 🎯 交互特性
- **统一事件模型**：在熟悉的事件模型基础上抽象和统一触摸事件
- **标准手势支持**：内置支持桌面和触摸的标准手势
- **链式调用**：为方便使用提供链式调用
- **函数式编程模式**：支持函数式编程模式

### 🔬 分析功能
- **图论算法**：从 BFS 到 PageRank 的各种算法
- **集合理论操作**：支持集合理论操作
- **JSON 序列化**：完全可序列化和反序列化

## 安装

### 使用 npm
```bash
npm install cytoscape
```

### 使用 yarn
```bash
yarn add cytoscape
```

### 使用 bower
```bash
bower install cytoscape
```

## 基础使用示例

### 1. 基本图形创建

```javascript
import cytoscape from 'cytoscape';

// 创建 Cytoscape 实例
const cy = cytoscape({
  container: document.getElementById('cy'), // 容器元素

  elements: [
    // 节点
    { data: { id: 'a', label: '节点 A' } },
    { data: { id: 'b', label: '节点 B' } },
    { data: { id: 'c', label: '节点 C' } },
    
    // 边
    { data: { id: 'ab', source: 'a', target: 'b' } },
    { data: { id: 'bc', source: 'b', target: 'c' } }
  ],

  style: [
    {
      selector: 'node',
      style: {
        'background-color': '#666',
        'label': 'data(label)',
        'text-valign': 'center',
        'text-halign': 'center'
      }
    },
    {
      selector: 'edge',
      style: {
        'width': 3,
        'line-color': '#ccc',
        'target-arrow-color': '#ccc',
        'target-arrow-shape': 'triangle'
      }
    }
  ],

  layout: {
    name: 'grid',
    rows: 1
  }
});
```

### 2. 动态添加节点

```javascript
// 添加新节点
cy.add({
  group: 'nodes',
  data: { 
    id: 'd', 
    label: '新节点' 
  },
  position: { x: 200, y: 200 }
});

// 添加新边
cy.add({
  group: 'edges',
  data: {
    id: 'cd',
    source: 'c',
    target: 'd'
  }
});
```

### 3. 事件处理

```javascript
// 节点点击事件
cy.on('tap', 'node', function(evt) {
  const node = evt.target;
  console.log('点击了节点:', node.id());
});

// 边点击事件
cy.on('tap', 'edge', function(evt) {
  const edge = evt.target;
  console.log('点击了边:', edge.id());
});
```

### 4. 样式定制

```javascript
const deviceStyle = [
  {
    selector: 'node[type="server"]',
    style: {
      'background-color': '#FF6B6B',
      'shape': 'rectangle',
      'width': '60px',
      'height': '40px'
    }
  },
  {
    selector: 'node[type="router"]',
    style: {
      'background-color': '#4ECDC4',
      'shape': 'diamond',
      'width': '50px',
      'height': '50px'
    }
  },
  {
    selector: 'node[status="online"]',
    style: {
      'border-width': '3px',
      'border-color': '#4CAF50'
    }
  }
];
```

### 5. 布局配置

```javascript
// 网格布局
cy.layout({
  name: 'grid',
  rows: 3,
  cols: 3
}).run();

// 层次布局
cy.layout({
  name: 'breadthfirst',
  directed: true,
  roots: '#root'
}).run();

// 物理布局（CoSE）
cy.layout({
  name: 'cose',
  idealEdgeLength: 100,
  nodeOverlap: 20,
  refresh: 20,
  fit: true,
  padding: 30,
  randomize: false,
  componentSpacing: 100,
  nodeRepulsion: 400000,
  edgeElasticity: 100,
  nestingFactor: 5,
  gravity: 80,
  numIter: 1000,
  initialTemp: 200,
  coolingFactor: 0.95,
  minTemp: 1.0
}).run();
```

## 常见应用场景

### 🌐 网络拓扑图
- 数据中心网络拓扑
- 企业网络架构图
- 服务器集群拓扑

### 🔗 关系图谱
- 社交网络关系
- 知识图谱
- 组织架构图

### 📊 数据可视化
- 流程图
- 依赖关系图
- 数据流图

### 🧬 科学研究
- 生物网络
- 化学分子结构
- 基因关系网络

## 扩展生态

Cytoscape.js 拥有丰富的扩展生态系统：

- **布局扩展**：Cola.js、Dagre、Klay 等
- **交互扩展**：Edgehandles、Cxtmenu、Popper.js 等
- **分析扩展**：各种图论算法扩展

## 性能特点

- **高性能渲染**：支持大规模图形（数千个节点和边）
- **内存优化**：高效的内存使用
- **响应式**：流畅的用户交互体验
- **可扩展性**：模块化设计，按需加载功能

## 社区支持

- **活跃维护**：每周补丁发布、每月功能发布
- **完整文档**：包含实时代码示例的交互式文档
- **测试覆盖**：大量测试套件确保质量
- **开源社区**：GitHub 上的活跃社区支持

---

**了解更多**：访问 [Cytoscape.js 官网](https://js.cytoscape.org/) 获取完整文档和示例。 