## JS SDK 接入

为了提升开发者的接入体验，减少复杂的开发工作，快速实现前端页面定制，有赞云提供了前端页面的youzanyun SDK，封装了相关的数据、事件和流程，后续会逐步开放和完善能力。
youzanyun SDK的使用依赖前端扩展点开发，仅支持组件定制开发。

⚠️注：本地开发预览时sdk数据属于mock数据，正式发布后数据会生效

### SDK使用示例

```javascript
// 以商详页为例
Component({
  behaviors: [],
  properties: {
    goodsInfo: Object
  },
  data: {},
  ready: function ready() {
    const yunSdk = this.getYunSdk();
    console.log('goodsItem: ', yunSdk.page.goodsItem);
    yunSdk.page.events.beforeCartSubmit.on('listenBeforeCartSubmit', (args) => {
      console.log('beforeCartSubmit', args);
      // todo
      // ------
      /* 异步方法reject的方式可以终止后续事件执行
      * 即此时return Promise.reject()，则不会加入购物车数据
      */
      return Promise.reject()
    });
  },
  methods: {
    // 获取youzanyunSdk
    getYunSdk() {
      return getApp().getYouZanYunSdk();
    },
    showSKU() {
      const showSKU = this.getYunSdk().page.getProcess('showSKU');
      showSKU('selectSku').then(() => {
        console.log('已经弹起');
      });
    }
  }
});
```

### SDK基础方法
```javascript
getYunSdk() {
  return getApp().getYouZanYunSdk();
}
const yunSdk = this.getYunSdk();
```
以下`yunSdk`皆为通过上述方式获取

#### 1. yunSdk.app[key]
- 获取全局数据
- key: 字段名
- 示例：
```javascript
const user = yunSdk.app.user; // 获取用户信息
```

#### 2. yunSdk.nagivate(pageName, args, type='navigateTo')
- 页面跳转
- 参数：
  - pageName: String 页面名称;
  - args: Object 页面参数;
  - type: 跳转类型, 可选值：'navigateTo' || 'redirectTo' || 'navigateBack' || 'reLaunch'，含义同微信小程序API
- 示例：
```javascript
// 跳转到商品详情页，保留当前页面，跳转到应用内的某个页面。但是不能跳到 tabbar 页面
yunSdk.navigate('goodsDetail', { alias: 'xxxxx' }, 'navidateTo');
```

#### 3. yunSdk.page[key]
- 获取页面数据
- key: 字段名
- 示例：
  1. 静态获取
  ```javascript
    const goodsItem = yunSdk.page.goodsItem
  ```
  2. watch方式订阅数据
  ```javascript
    const unwatchGoodsItemFn = yunSdk.page.$watch('goodsItem', funtion (newVal, oldVal) {
      console.log('goodsItem changed:', newVal);
    });
    // 取消订阅
    unwatchGoodsItemFn();
  ```

#### 4. yunSdk.page.events[key]
- 页面事件扩展。在官方页面中，我们提供了一系列可扩展的事件，通过示例中方式可以在特定时机触发开发者自定义的事件扩展。
- 事件类型有： 同步 和 异步
- 示例：
```javascript
  /* 订阅商品详情页加入购物车之前的事件
   * 在商详页SKU弹窗中点击加入购物车后，保存购物车数据之前会先执行该自定义事件
   * 该方法为异步方法
   */
  yunSdk.page.events.beforeCartSubmit.on('listenBeforeCartSubmit', (args) => {
    console.log('beforeCartSubmit', args);
    // todo
    // ------
    /* 异步方法reject的方式可以终止后续事件执行
     * 即此时return Promise.reject()，则不会加入购物车数据
     */
    return Promise.reject()
  });
```
 
#### 5. yunSdk.page.getProcess(key)
- 页面流程。有赞提供了部分核心流程，方便开发者直接调用即可完成相应功能，而不需要关心内部实现。
- 示例：
```javascript
  // 弹起sku流程 
  const showSKU = yunSdk.page.getProcess('showSKU');
  showSKU('selectSku').then(() => {
    console.log('已经弹起');
  });
```
