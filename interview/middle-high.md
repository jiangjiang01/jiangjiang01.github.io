# 前端开发工程师面试题（中高级）

## 题目

### HTML 和 CSS 基础

1. 请详细说明 HTML 中图片（img）标签的优化方案，包括懒加载、响应式图片、图片格式选择等方面？
2. CSS 中如何处理动画性能优化？请详细说明 CSS 动画的性能优化方案，以及如何选择合适的动画实现方式？
3. 请解释一下 BFC（块级格式化上下文）的概念，以及如何创建 BFC？BFC 有什么作用？

### JavaScript 基础

1. 请解释一下事件循环（Event Loop）机制，以及宏任务和微任务的区别？
2. 请解释一下 Promise 的原理，以及 async/await 的实现机制？

### Vue 相关

1. Vue3 相比 Vue2 有哪些重要的改进？Composition API 的优势是什么？
2. Vue 的响应式原理是什么？Vue3 的响应式系统是如何实现的？
3. Vuex 和 Pinia 的区别是什么？为什么 Vue3 推荐使用 Pinia？

### React 相关

1. React Hooks 的优势是什么？与类组件相比有什么不同？
2. 解释一下 React 的虚拟 DOM 和 Diff 算法的工作原理？
3. React 的性能优化方案有哪些？如何避免不必要的重渲染？
4. React 的 Fiber 架构是什么？它解决了什么问题？

### 网络相关

1. 请详细解释一下 HTTP 缓存机制？
2. 跨域问题有哪些解决方案？它们的原理是什么？
3. HTTPS 的工作原理是什么？与 HTTP 相比有什么优势？
4. 请解释一下 TCP 三次握手和四次挥手的过程？
5. 什么是 XSS 和 CSRF 攻击？如何防范？

### 算法相关

1. 如何判断两个对象是否相等？
2. 实现一个函数，将数组中的元素按照出现频率排序？

### 项目经验

1. 请详细描述一下你做过的最有挑战性的项目？遇到了哪些技术难点？如何解决的？
2. 在项目中你是如何进行性能优化的？取得了什么效果？
3. 项目中是如何处理前端错误的？有什么监控和报警机制？
4. 在团队协作中，你是如何保证代码质量的？
5. 项目中使用了哪些工程化工具？为什么要选择这些工具？

### 场景题

### H5 抽奖活动页面开发方案

实现以下的需求：

<img width="399" alt="Image" src="https://github.com/user-attachments/assets/96712a86-347c-4be6-bcfe-e0d23a08712d" />

假设你作为前端负责人，需要开发一个 H5 抽奖活动页面。页面主要包含活动介绍文案和抽奖按钮。现在 PM 和后端 RD 都是实习生，技术和业务都不熟练，你需要从 0 开发这个页面。请从以下维度详细说明你的解决方案：

1. 需求分析和接口设计
2. 技术方案设计
3. 项目管理建议
4. 可能遇到的问题及解决方案

## 解答

### 一、HTML 和 CSS 基础

1. 请详细说明 HTML 中图片（img）标签的优化方案，包括懒加载、响应式图片、图片格式选择等方面？

答：图片优化方案：

- 懒加载实现：

  ```html
  <!-- 原生懒加载 -->
  <img src="placeholder.jpg" data-src="actual-image.jpg" loading="lazy" />

  <!-- Intersection Observer实现 -->
  <script>
    const observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const img = entry.target;
          img.src = img.dataset.src;
          observer.unobserve(img);
        }
      });
    });
  </script>
  ```

- 响应式图片：

  ```html
  <!-- srcset和sizes -->
  <img
    src="small.jpg"
    srcset="small.jpg 300w, medium.jpg 600w, large.jpg 900w"
    sizes="(max-width: 320px) 280px, (max-width: 640px) 580px, 800px"
  />

  <!-- picture元素 -->
  <picture>
    <source media="(min-width: 800px)" srcset="large.jpg" />
    <source media="(min-width: 400px)" srcset="medium.jpg" />
    <img src="small.jpg" alt="响应式图片" />
  </picture>
  ```

- 图片格式选择：
  - WebP：更好的压缩比，支持透明度
  - AVIF：新一代图片格式，更好的压缩效果
  - 根据浏览器支持情况提供降级方案

2. CSS 中如何处理动画性能优化？请详细说明 CSS 动画的性能优化方案，以及如何选择合适的动画实现方式？

答：CSS 动画性能优化：

- 性能优化原则：

  ```css
  /* 使用transform和opacity进行动画 */
  .animate {
    /* 好的做法 */
    transform: translateX(100px);
    opacity: 0;

    /* 避免使用 */
    left: 100px;
    margin-left: 100px;
  }

  /* 使用will-change提示浏览器 */
  .will-animate {
    will-change: transform, opacity;
  }

  /* 使用GPU加速 */
  .gpu-accelerated {
    transform: translateZ(0);
    /* 或 */
    backface-visibility: hidden;
  }
  ```

- 动画实现方式选择：
  - CSS animation：简单动画，性能好
  - CSS transition：状态变化过渡
  - JavaScript requestAnimationFrame：复杂动画
  - Web Animations API：需要精确控制
- 性能优化建议：
  - 避免频繁重排
  - 使用 transform 代替位置属性
  - 合理使用 will-change
  - 控制动画帧数
  - 避免同时动画过多元素

3. 请解释一下 BFC（块级格式化上下文）的概念，以及如何创建 BFC？BFC 有什么作用？

答：BFC（块级格式化上下文）是一个独立的渲染区域，在这个区域中，块级元素的布局与渲染不会影响到区域外部的布局与渲染。创建 BFC 的方法包括：

- 根元素
- float 属性不为 none
- position 为 absolute 或 fixed
- display 为 inline-block, table-cell, table-caption, flex, 或 inline-flex
- overflow 不为 visible

BFC 的作用包括：

- 清除浮动
- 防止 margin 重叠
- 自适应两栏布局

### 二、JavaScript 基础

1. 请解释一下事件循环（Event Loop）机制，以及宏任务和微任务的区别？

答：事件循环是 JS 处理异步操作的机制。执行顺序：同步代码 → 微任务队列 → 宏任务队列。微任务（Promise.then、MutationObserver）优先级高于宏任务（setTimeout、setInterval、I/O）。每个宏任务之后都会清空微任务队列。

2. 请解释一下 Promise 的原理，以及 async/await 的实现机制？

答：Promise 是异步编程的解决方案，有 pending、fulfilled、rejected 三种状态，状态改变不可逆。async/await 是 Promise 的语法糖，async 函数返回 Promise，await 暂停函数执行等待 Promise 完成。底层基于 Generator 实现。

### 三、Vue 相关

1. Vue3 相比 Vue2 有哪些重要的改进？Composition API 的优势是什么？

答：主要改进：响应式系统用 Proxy 重写、TreeShaking 支持、Composition API 引入。Composition API 优势：更好的代码组织、逻辑复用、更好的 TypeScript 支持、避免 this 指向问题。

2. Vue 的响应式原理是什么？Vue3 的响应式系统是如何实现的？

答：Vue2 使用 Object.defineProperty 劫持对象属性，Vue3 使用 Proxy 代理整个对象。Proxy 的优势：可以监听数组变化、动态属性添加、更好的性能、可以监听 Map/Set。

3. Vuex 和 Pinia 的区别是什么？为什么 Vue3 推荐使用 Pinia？

答：Pinia 优势：更好的 TypeScript 支持、无需嵌套模块、更简洁的 API、无需命名空间、支持多个 Store、体积更小。完全符合 Vue3 的 Composition API 风格。

### 四、React 相关

1. React Hooks 的优势是什么？与类组件相比有什么不同？

答：Hooks 优势：逻辑复用更简单、避免 this 问题、代码更简洁、更好的类型推导。函数组件无需生命周期方法，使用 useEffect 统一处理副作用。

2. 解释一下 React 的虚拟 DOM 和 Diff 算法的工作原理？

答：虚拟 DOM 是真实 DOM 的 JS 对象表示。Diff 算法基于三个策略：同层比较、类型不同直接替换、key 标识节点。时间复杂度从 O(n^3)优化到 O(n)。

3. React 的性能优化方案有哪些？如何避免不必要的重渲染？

答：使用 React.memo/useMemo/useCallback 避免不必要渲染，合理使用 key，避免内联对象/函数，使用虚拟列表处理大数据，代码分割和懒加载。

4. React 的 Fiber 架构是什么？它解决了什么问题？

答：Fiber 是 React16 引入的新协调引擎，将渲染工作分割成小单元，可中断和恢复，实现时间切片，解决大量同步计算导致的页面卡顿问题。

### 五、网络相关

1. 请详细解释一下 HTTP 缓存机制？

答：分强缓存（Expires、Cache-Control）和协商缓存（Last-Modified/If-Modified-Since、ETag/If-None-Match）。强缓存直接使用本地缓存，协商缓存需要服务器验证。

2. 跨域问题有哪些解决方案？它们的原理是什么？

答：CORS（服务端配置）、JSONP（script 标签）、代理服务器、postMessage、WebSocket。最常用 CORS，通过 HTTP 头控制跨域访问权限。

3. HTTPS 的工作原理是什么？与 HTTP 相比有什么优势？

答：HTTPS = HTTP + SSL/TLS，使用非对称加密传输密钥，对称加密传输数据。优势：加密传输、防篡改、身份认证。

4. 请解释一下 TCP 三次握手和四次挥手的过程？

答：三次握手：SYN→SYN+ACK→ACK。四次挥手：FIN→ACK→FIN→ACK。三次握手建立连接，四次挥手断开连接，确保可靠传输。

5. 什么是 XSS 和 CSRF 攻击？如何防范？

答：XSS 是注入恶意脚本，防范：转义输出、CSP。CSRF 是伪造用户请求，防范：Token 验证、SameSite Cookie、验证码。

### 六、算法相关

1. 如何判断两个对象是否相等？

答：浅比较：Object.keys 比较长度和属性值。深比较：递归比较所有层级，注意循环引用。也可用 JSON.stringify 比较（有局限性）。

2. 实现一个函数，将数组中的元素按照出现频率排序？

答：使用 Map 统计频率，转数组排序。时间复杂度 O(nlogn)，空间复杂度 O(n)。

### 七、项目经验

1. 请详细描述一下你做过的最有挑战性的项目？遇到了哪些技术难点？如何解决的？

2. 在项目中你是如何进行性能优化的？取得了什么效果？

3. 项目中是如何处理前端错误的？有什么监控和报警机制？

4. 在团队协作中，你是如何保证代码质量的？

5. 项目中使用了哪些工程化工具？为什么要选择这些工具？

### 八、场景题

实现以下的需求：

<img width="399" alt="Image" src="https://github.com/user-attachments/assets/96712a86-347c-4be6-bcfe-e0d23a08712d" />

假设你作为前端负责人，需要开发一个 H5 抽奖活动页面。页面主要包含活动介绍文案和抽奖按钮。现在 PM 和后端 RD 都是实习生，技术和业务都不熟练，你需要从 0 开发这个页面。请从以下维度详细说明你的解决方案：

1. 需求分析和接口设计
2. 技术方案设计
3. 项目管理建议
4. 可能遇到的问题及解决方案

#### 1. 需求分析和接口设计

##### 活动信息相关：

- 需要后端提供活动基础信息接口：
  ```json
  {
    "activityId": "string", // 活动ID
    "title": "string", // 活动标题
    "content": "string", // 活动介绍文案
    "startTime": "timestamp", // 活动开始时间
    "endTime": "timestamp", // 活动结束时间
    "status": "number", // 活动状态：0-未开始，1-进行中，2-已结束
    "bannerUrl": "string", // 活动banner图片URL（可选）
    "shareConfig": {
      // 分享配置
      "title": "string",
      "desc": "string",
      "imgUrl": "string"
    }
  }
  ```

##### 用户信息相关：

- 登录状态检查接口
- 用户信息获取接口：
  ```json
  {
    "userId": "string",
    "isNewUser": "boolean", // 是否新用户
    "phone": "string", // 手机号（可能需要脱敏）
    "drawCount": "number" // 剩余抽奖次数
  }
  ```
- 手机号绑定接口（如需）：
  ```json
  POST /api/bind-phone
  {
    "phone": "string",
    "verifyCode": "string"
  }
  ```

##### 抽奖相关：

- 抽奖接口：
  ```json
  POST /api/draw
  Response: {
    "prizeId": "string",      // 奖品ID
    "prizeName": "string",    // 奖品名称
    "prizeType": "number",    // 奖品类型：1-实物，2-虚拟物品，3-优惠券
    "prizeImg": "string"      // 奖品图片
  }
  ```
- 获取抽奖记录接口：
  ```json
  GET /api/draw-records
  Response: {
    "list": [{
      "prizeId": "string",
      "prizeName": "string",
      "drawTime": "timestamp",
      "status": "number"      // 0-未领取，1-已领取
    }]
  }
  ```

##### 数据统计相关：

- 前端需要埋点的事件：
  1. 页面访问（PV/UV）
  2. 抽奖按钮点击
  3. 分享按钮点击
  4. 登录/注册行为
  5. 奖品领取行为

#### 2. 技术方案设计

1. 前端架构：

   - 使用 Vue3 + Vite 搭建项目
   - 使用 TypeScript 确保代码质量
   - 使用 Pinia 管理状态
   - 使用 VueUse 处理常用逻辑

2. 性能优化：

   - 图片资源 CDN + 预加载
   - 路由懒加载
   - 组件按需加载
   - Service Worker 缓存静态资源

3. 安全考虑：

   - 防止重复抽奖（前端节流 + 后端幂等）
   - XSS 防护
   - 点击劫持防护
   - 敏感信息加密传输

4. 容错处理：
   - 网络异常重试机制
   - 登录态失效处理
   - 活动状态异常处理
   - 降级方案准备

#### 3. 项目管理建议

1. 与后端约定接口规范：

   - RESTful API 设计
   - 统一的错误码规范
   - 统一的响应格式

2. 测试策略：

   - 单元测试覆盖核心逻辑
   - E2E 测试覆盖关键路径
   - 性能测试和压力测试

3. 文档输出：

   - 接口文档（Swagger）
   - 技术方案文档
   - 测试用例文档
   - 上线检查清单

4. 监控告警：
   - 前端错误监控
   - 性能监控
   - 业务指标监控
   - 用户行为分析

#### 4. 可能遇到的问题及解决方案

1. 实习生 RD 经验不足：

   - 提供详细的接口文档和示例
   - 做好接口 Mock
   - 定期 Code Review
   - 技术分享和指导

2. 活动运营风险：

   - 设置抽奖频率限制
   - 增加用户验证机制
   - 设置奖品库存告警
   - 准备降级方案

3. 性能问题：

   - 首屏加载优化
   - 动画性能优化
   - 网络请求优化
   - 缓存策略优化

4. 兼容性问题：
   - 浏览器兼容性测试
   - 不同机型适配测试
   - 网络环境测试
   - 降级方案准备
