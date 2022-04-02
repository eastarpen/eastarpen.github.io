## 布局容器和栅格网格系统

### 布局容器 

  1.  `.container` 类用于固定宽度并支持响应式布局的容器

     ```html
     <div class = "container"></div>
     ```

  2. `.container-fluid` 用于 100% 宽度, 占据全部视口(viewport)

     ```html
     <div class = "container-fluid"></div>
     ```

### 栅格网格系统

     ```html
     <div class="container">
        <div class="row">
            <div class="col-md-4"></div>
        </div>
     </div>
     ```

    1. 将一行分为12列, 自行选择元素宽度, 总和超出12 列则换行

    2. md -> middle 表示适配中等大小屏幕, 可替换为 xs, sm, md, lg

    3. `<div class="col-md-4 col-lg-4"></div>` 将自动计算

    4. `col-md-offset-4` 偏移4格, 超出12格换行

    5. `col-md-push-, col-md-pull` 浮动, 会被后面元素覆盖

## 常用样式

### 标题

    1. 对 html 标题标签样式优化

    2. 提供 ".h1~.h6"  类为非标题标签提供标题样式

    3. 提供副标题标签 `<small></small>`
