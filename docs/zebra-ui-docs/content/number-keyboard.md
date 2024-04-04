# NumberKeyboard 数字键盘

### 介绍

虚拟数字键盘，可以配合[密码输入框组件](/password-input)或自定义的输入框组件使用。

## 代码演示

### 默认样式

数字键盘提供了 `input`、`delete`、`blur` 事件，分别对应输入内容、删除内容和失去焦点的动作。

```html
<z-cell title="弹出默认键盘" is-link @click="show = true" />
<z-number-keyboard
  :show="show"
  @blur="show = false"
  @input="onInput"
  @delete="onDelete"
/>
```

```js
import { ref } from 'vue';
import { useToast } from '../../uni_modules/zebra-ui'
const toast = useToast()
const show = ref(false)
const onInput = (value: string) => toast.showToast(value)
const onDelete = () => toast.showToast('删除')
```

### 带右侧栏的键盘

将 theme 属性设置为 `custom` 来展示键盘的右侧栏，常用于输入金额的场景。

```html
<z-number-keyboard
  :show="show"
  theme="custom"
  extra-key="."
  close-button-text="完成"
  @blur="show = false"
  @input="onInput"
  @delete="onDelete"
/>
```

### 身份证号键盘

通过 `extra-key` 属性可以设置左下角按键内容，比如需要输入身份证号时，可以将 `extra-key` 设置为 `X`。

```html
<z-cell plain type="primary" @touchstart.stop="show = true">
  弹出身份证号键盘
</z-cell>
<z-number-keyboard
  :show="show"
  extra-key="X"
  close-button-text="完成"
  @blur="show = false"
  @input="onInput"
  @delete="onDelete"
/>
```

### 键盘标题

通过 `title` 属性可以设置键盘标题。

```html
<z-cell plain type="primary" @touchstart.stop="show = true">
  弹出带标题的键盘
</z-cell>
<z-number-keyboard
  :show="show"
  title="键盘标题"
  extra-key="."
  close-button-text="完成"
  @blur="show = false"
  @input="onInput"
  @delete="onDelete"
/>
```

### 配置多个按键

当 theme 为 `custom` 时，支持以数组的形式配置两个 `extra-key`。

```html
<z-cell plain type="primary" @touchstart.stop="show = true">
  弹出配置多个按键的键盘
</z-cell>
<z-number-keyboard
  :show="show"
  theme="custom"
  :extra-key="['00', '.']"
  close-button-text="完成"
  @blur="show = false"
  @input="onInput"
  @delete="onDelete"
/>
```

### 随机数字键盘

通过 `random-key-order` 属性可以随机排序数字键盘，常用于安全等级较高的场景。

```html
<z-cell @touchstart.stop="show = true"> 弹出配置随机数字的键盘 </z-cell>
<z-number-keyboard
  :show="show"
  random-key-order
  @blur="show = false"
  @input="onInput"
  @delete="onDelete"
/>
```

### 双向绑定

可以通过 `v-model` 绑定键盘当前输入值，并通过 `maxlength` 属性来限制输入长度。

```html
<z-field
  v-model="valueModel"
  label="双向绑定"
  readonly
  clickable
  @click="showValue = true"
/>
<z-number-keyboard
  v-model="valueModel"
  :show="showValue"
  :maxlength="6"
  @blur="showValue = false"
/>
```

```js
import { ref } from 'vue';
const show = ref(true);
const value = ref('');
```

## API

### Props

| 参数 | 说明 | 类型 | 默认值 |
| --- | --- | --- | --- |
| v-model | 当前输入值 | _string_ | - |
| show | 是否显示键盘 | _boolean_ | - |
| title | 键盘标题 | _string_ | - |
| theme | 样式风格，可选值为 `custom` | _string_ | `default` |
| maxlength | 输入值最大长度 | _number \| string_ | `Infinity` |
| transition | 是否开启过场动画 | _boolean_ | `true` |
| z-index | 键盘 z-index 层级 | _number \| string_ | `100` |
| extra-key | 底部额外按键的内容 | _string \| string[]_ | `''` |
| close-button-text | 关闭按钮文字，空则不展示 | _string_ | - |
| delete-button-text | 删除按钮文字，空则展示删除图标 | _string_ | - |
| close-button-loading | 是否将关闭按钮设置为加载中状态，仅在 `theme="custom"` 时有效 | _boolean_ | `false` |
| show-delete-key | 是否展示删除图标 | _boolean_ | `true` |
| blur-on-close | 是否在点击关闭按钮时触发 blur 事件 | _boolean_ | `true` |
| hide-on-click-outside | 是否在点击外部时收起键盘 | _boolean_ | `true` |
| safe-area-inset-bottom | 是否开启[底部安全区适配](/adzced-usage) | _boolean_ | `true` |
| random-key-order | 是否将通过随机顺序展示按键 | _boolean_ | `false` |

### Events

| 事件名 | 说明                           | 回调参数      |
| ------ | ------------------------------ | ------------- |
| input  | 点击按键时触发                 | _key: string_ |
| delete | 点击删除键时触发               | -             |
| close  | 点击关闭按钮时触发             | -             |
| blur   | 点击关闭按钮或非键盘区域时触发 | -             |
| show   | 键盘完全弹出时触发             | -             |
| hide   | 键盘完全收起时触发             | -             |

### Slots

| 名称       | 说明                 |
| ---------- | -------------------- |
| delete     | 自定义删除按键内容   |
| extra-key  | 自定义左下角按键内容 |
| title-left | 自定义标题栏左侧内容 |

## 主题定制

### 样式变量

组件提供了下列 CSS 变量，可用于自定义样式，使用方法请参考 [ConfigProvider 组件](/config-provider)。

| 名称                                    | 默认值                     | 描述 |
| --------------------------------------- | -------------------------- | ---- |
| --z-number-keyboard-background        | _var(--z-gray-2)_        | -    |
| --z-number-keyboard-key-height        | _96rpx_                     | -    |
| --z-number-keyboard-key-font-size     | _56rpx_                     | -    |
| --z-number-keyboard-key-active-color  | _var(--z-gray-3)_        | -    |
| --z-number-keyboard-key-background    | _var(--z-white)_         | -    |
| --z-number-keyboard-delete-font-size  | _var(--z-font-size-lg)_  | -    |
| --z-number-keyboard-title-color       | _var(--z-gray-7)_        | -    |
| --z-number-keyboard-title-height      | _68px_                     | -    |
| --z-number-keyboard-title-font-size   | _var(--z-font-size-lg)_  | -    |
| --z-number-keyboard-close-padding     | _0 var(--z-padding-md)_  | -    |
| --z-number-keyboard-close-color       | _var(--z-primary-color)_ | -    |
| --z-number-keyboard-close-font-size   | _var(--z-font-size-md)_  | -    |
| --z-number-keyboard-button-text-color | _var(--z-white)_         | -    |
| --z-number-keyboard-button-background | _var(--z-primary-color)_ | -    |
| --z-number-keyboard-z-index           | _100_                      | -    |
