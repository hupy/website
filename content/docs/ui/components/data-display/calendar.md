+++
title = "Calendar 日历"
weight = 3
+++

# Calendar 日历

按照日历形式展示数据的容器。

## 何时使用

当数据是日期或按照日期划分时，例如日程、课表、价格日历等，农历等。目前支持年/月切换。

## 代码演示

<div class="c7n-row">
    <div class="c7n-row-12">
        <section class="code-box">
            <section class="code-box-demo"><div id="calendar-demo-basic"></div></section>
            <section class="code-box-meta">
                <div class="code-box-title"><a>基本</a></div>
                <div>
                    <p>一个通用的日历面板，支持年/月切换。</p>
                </div>
            </section>
        </section>
        <section class="code-box">
            <section class="code-box-demo"><div id="calendar-demo-notice"></div></section>
            <section class="code-box-meta">
                <div class="code-box-title"><a>通知事项日历</a></div>
                <div>
                    <p>一个复杂的应用示例，用 <code>dateCellRender</code> 和 <code>monthCellRender</code> 函数来自定义需要渲染的数据。</p>
                </div>
            </section>
        </section>
        <section class="code-box">
            <section class="code-box-demo"><div id="calendar-demo-card"></div></section>
            <section class="code-box-meta">
                <div class="code-box-title"><a>卡片模式</a></div>
                <div>
                    <p>用于嵌套在空间有限的容器中。</p>
                </div>
            </section>
        </section>
        <section class="code-box">
            <section class="code-box-demo"><div id="calendar-demo-select"></div></section>
            <section class="code-box-meta">
                <div class="code-box-title"><a>选择功能</a></div>
                <div>
                    <p>一个通用的日历面板，支持年/月切换。</p>
                </div>
            </section>
        </section>
    </div>
</div>

{{< components-calendar >}}

## API

**注意：**Calendar 部分 locale 是从 value 中读取，所以请先正确设置 moment 的 locale。

```jsx
// 默认语言为 en-US，所以如果需要使用其他语言，推荐在入口文件全局设置 locale
// import moment from 'moment';
// import 'moment/locale/zh-cn';
// moment.locale('zh-cn');

<Calendar
  dateCellRender={dateCellRender}
  monthCellRender={monthCellRender}

  onPanelChange={onPanelChange}

  onSelect={onSelect}
/>
```

| 参数 | 说明 | 类型 | 默认值 |
| --- | --- | --- | --- |
| dateCellRender | 自定义渲染日期单元格，返回内容会被追加到单元格 | function(date: moment): ReactNode | 无 |
| dateFullCellRender | 自定义渲染日期单元格，返回内容覆盖单元格 | function(date: moment): ReactNode | 无 |
| defaultValue | 默认展示的日期 | [moment](http://momentjs.com/) | 默认日期 |
| disabledDate | 不可选择的日期 | (currentDate: moment) => boolean | 无 |
| fullscreen | 是否全屏显示 | boolean | true |
| locale | 国际化配置 | object | [默认配置](https://github.com/ant-design/ant-design/blob/master/components/date-picker/locale/example.json) |
| mode | 初始模式，`month/year` | string | month |
| monthCellRender | 自定义渲染月单元格，返回内容会被追加到单元格 | function(date: moment): ReactNode | 无 |
| monthFullCellRender | 自定义渲染月单元格，返回内容覆盖单元格 | function(date: moment): ReactNode | 无 |
| validRange | 设置可以显示的日期 | \[[moment](http://momentjs.com/), [moment](http://momentjs.com/)] | 无 |
| value | 展示日期 | [moment](http://momentjs.com/) | 当前日期 |
| onPanelChange | 日期面板变化回调 | function(date: moment, mode: string) | 无 |
| onSelect | 点击选择日期回调 | function(date: moment） | 无 |