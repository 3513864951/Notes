这是CSS中的两个属性，用于控制flex容器中的项目在主轴和交叉轴上的对齐方式。

**justify-content: center;** 属性用于控制flex容器中的项目在主轴上的对齐方式，将项目居中对齐。主轴是flex容器的水平方向或垂直方向，取决于flex容器的flex-direction属性设置。

**align-items: center;** 属性用于控制flex容器中的项目在交叉轴上的对齐方式，将项目居中对齐。交叉轴是与主轴垂直的方向。

通过这两个属性的设置，可以实现将flex容器中的项目在水平和垂直方向上都居中对齐的效果。

------------------

`flex-direction` 属性定义容器要在哪个方向上堆叠 flex 项目。

`column` 值设置垂直堆叠 flex 项目（从上到下）

`row` 值水平堆叠 flex 项目（从左到右）

------------------

box-shadow

![image-20231214171143210](C:\Users\liukai\AppData\Roaming\Typora\typora-user-images\image-20231214171143210.png)

```csharp
  box-shadow: 30px 10px;水平 垂直 模糊 阴影的模糊半径 
```

------------------------

position

[CSS 布局 - position 属性 (w3school.com.cn)](https://www.w3school.com.cn/css/css_positioning.asp)

------------------------

![image-20231214173637086](C:\Users\liukai\AppData\Roaming\Typora\typora-user-images\image-20231214173637086.png)

```
overflow: visible;鼠标放到上面是否显示
```



------------------------

![image-20231214173922611](C:\Users\liukai\AppData\Roaming\Typora\typora-user-images\image-20231214173922611.png)


------------------

1. **LocalStorage:**
   - **特点：** 提供了在浏览器中存储键值对数据的简单方式，数据存储在客户端，并且相对于会话级别的 sessionStorage，数据可以长期保留。
   - **容量：** 通常限制在 5MB 左右。
   - **用途：** 用于存储持久性的用户设置、缓存数据等。
2. **SessionStorage:**
   - **特点：** 与 LocalStorage 类似，也是用于在浏览器中存储键值对数据，但数据在用户会话结束时被清除（当用户关闭浏览器窗口或标签页时）。
   - **容量：** 通常也限制在 5MB 左右。
   - **用途：** 适合存储临时性的会话数据，如页面之间的数据传递、表单数据暂存等。
3. **IndexedDB:**
   - **特点：** 一种低级API，用于在客户端存储大量结构化数据。提供了比 LocalStorage 和 SessionStorage 更强大和灵活的存储功能，可以存储大容量的数据、支持索引和事务。
   - **容量：** 通常比 LocalStorage 和 SessionStorage 允许存储更大量级的数据。
   - **用途：** 适合需要存储大量结构化数据、如离线应用、大型数据集的缓存等场景。
4. **Cookies:**
   - **特点：** 存储在用户本地的小型文本文件，被用于跟踪、会话管理和存储少量数据。
   - **容量：** 每个 Cookie 通常限制在几KB。
   - **用途：** 主要用于存储少量的认证信息、跟踪标识符等。但由于容量小且每次 HTTP 请求都会携带，不适合存储大量数据。
5. **Web Storage API:**
   - **特点：** 包括 LocalStorage 和 SessionStorage，提供了一种简单的键值对存储方式。
   - **用途：** 适用于需要在客户端存储少量结构简单的数据，比如用户设置、应用状态等。

`position: fixed;` 的元素是相对于**视口**定位的

`position: relative;` 的元素**相对于其正常位置进行定位**。

position: static; 的元素不会以任何特殊方式定位；它始终**根据页面的正常流进行定位：**

`position: absolute;` 的元素相对于最近的定位**祖先元素**进行定位（而不是相对于视口定位，如 fixed）。

`position: sticky;` 的元素根据用户的滚动位置进行定位。当界面滑动到指定位置再固定成fixed















