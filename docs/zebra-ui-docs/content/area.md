# Area 省市区选择

### 介绍

省市区三级联动选择，通常与[弹出层](/popup)组件配合使用。

## 代码演示

### 基础用法

初始化省市区组件时，需要通过 `area-list` 属性传入省市区数据。

```html
<z-area title="标题" :area-list="areaList" />
```

### areaList 格式

areaList 为对象结构，包含 `province_list`、`city_list`、`county_list` 三个 key。

每项以地区码作为 key，省市区名字作为 value。地区码为 6 位数字，前两位代表省份，中间两位代表城市，后两位代表区县，以 0 补足 6 位。比如北京的地区码为 `11`，以 0 补足 6 位，为 `110000`。

示例数据如下：

```js
const areaList = {
  province_list: {
    110000: '北京市',
    120000: '天津市',
  },
  city_list: {
    110100: '北京市',
    120100: '天津市',
  },
  county_list: {
    110101: '东城区',
    110102: '西城区',
    // ....
  },
};
```

### 中国省市区数据

zebra-ui的示例项目提供了中国省市区数据，可以复制后直接引用

```ts
import { areaList } from '../../common/js/area'
```

### 控制选中项

通过 `v-model` 绑定当前选中的地区码。

```html
<z-area v-model="valueSelect" title="标题" :area-list="areaList" />
```

```js
import { ref } from 'vue';
import { areaList } from '../../common/js/area'
const valueSelect = ref('330302')
```

### 配置显示列

可以通过 `columns-num` 属性配置省市区显示的列数，默认情况下会显示省市区，当你设置为 `2`，则只会显示省市选择。

```html
<z-area title="标题" :area-list="areaList" :columns-num="2" />
```

### 配置列占位提示文字

可以通过 `columns-placeholder` 属性配置每一列的占位提示文字。

```html
<z-area
  title="标题"
  :area-list="areaList"
  :columns-placeholder="['请选择', '请选择', '请选择']"
/>
```

## API

### Props

| 参数 | 说明 | 类型 | 默认值 |
| --- | --- | --- | --- |
| v-model | 当前选中项对应的地区码 | _string_ | - |
| title | 顶部栏标题 | _string_ | - |
| confirm-button-text | 确认按钮文字 | _string_ | `确认` |
| cancel-button-text | 取消按钮文字 | _string_ | `取消` |
| area-list | 省市区数据 | _object_ | - |
| columns-placeholder | 列占位提示文字 | _string[]_ | `[]` |
| loading | 是否显示加载状态 | _boolean_ | `false` |
| readonly | 是否为只读状态，只读状态下无法切换选项 | _boolean_ | `false` |
| option-height | 选项高度，支持 `rpx` 单位，默认 `px` | _number \| string_ | `44` |
| columns-num | 显示列数，3-省市区，2-省市，1-省 | _number \| string_ | `3` |
| visible-option-num | 可见的选项个数 | _number \| string_ | `6` |
| swipe-duration | 快速滑动时惯性滚动的时长，单位 `ms` | _number \| string_ | `1000` |

### Events

| 事件名 | 说明 | 回调参数 |
| --- | --- | --- |
| confirm | 点击完成按钮时触发 | _{ selectedValues, selectedOptions, selectedIndexes }_ |
| cancel | 点击取消按钮时触发 | _{ selectedValues, selectedOptions, selectedIndexes }_ |
| change | 选项改变时触发 | _{ selectedValues, selectedOptions, selectedIndexes, columnIndex }_ |

### Slots

| 名称           | 说明                   | 参数 |
| -------------- | ---------------------- | ---- |
| toolbar        | 自定义整个顶部栏的内容 | -    |
| title          | 自定义标题内容         | -    |
| confirm        | 自定义确认按钮内容     | -    |
| cancel         | 自定义取消按钮内容     | -    |
| columns-top    | 自定义选项上方内容     | -    |
| columns-bottom | 自定义选项下方内容     | -    |

### 方法

通过 ref 可以获取到 Area 实例并调用实例方法。

| 方法名 | 说明 | 参数 | 返回值 |
| --- | --- | --- | --- |
| confirm | 停止惯性滚动并触发 `confirm` 事件 | - | - |
| getSelectedOptions | 获取当前选中的选项 | - | _PickerOption[]_ |
