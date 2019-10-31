## 商品详情页 sdk

### 页面数据(yunSdk.page[key])

```javascript
{
  // 商品信息
  goodsItem: {
    alias,     // 商品别名
    title,    // 商品名称
    picture,  // 商品头图，用于sku展示
    sellPoint,  // 商品卖点，页面子标题内容
    origin,  // 划线价
    isVirtual,  // 是否是虚拟商品，包含 虚拟商品和电子卡券
    isVirtualTicket,  // 是否是电子卡券
    isDisplay,    // 是否上架
    limitBuy,     // 是否仅限特定会员购买
    waitToSoldTime,     // 定时开售时间
    buyWay,     // 购买方式 0：外购买商品 1：非外部购买商品
    buyUrl,     // 外链商品购买链接
    forbidBuyReason,    // 不可购买原因
    isSupportFCode,     // 商品是否参与f码活动
    isGoodsCollected,     // 商品是否被收藏
    isInstallment,    // 是否支持分期支付
    risk：{ // 店铺风险提示
      match,  // 是否命中，
      note，//  说明文字
    }
  },
  // 退款模型
  refund: {
    isSupport,  // 是否支持退款（包含虚拟商品和电子卡券）
    type,  // 退款方式
    interval,  // 退款时间区间
  },
  // 多网点信息
  multistore: {
    name
  },
  // 店铺信息
  shop: {
    logo,     // 店铺logo
    name,     // 店铺名称
    url,    // 店铺跳转地址
    certType,     // 店铺认证类型:2:企业认证 3-4:个人认证 5-9:官方认证
  },
  // 店铺配置
  shopConfig: {
    isShowBuyBtn,  // 商品页是否展示立即购买按钮
    isSecuredTransactions,  // 是否加入担保交易
    showRecommendGoods,     // 是否开启推荐商品
    showBuyRecord,    // 是否开启销量与成交记录
    showCustomerReviews,    // 是否开启商品评价
    supportFreightInsurance,     // 是否支持运费险
    hideShoppingCart,      // 是否隐藏购物车按钮
    hasPhysicalStore,     // 是否有线下门店
  },
  // 店铺担保配置
  guarantee: {
    on,  // 是否加入有赞担保
    style,  // 担保样式
  },
  // 配送信息
  distribution: {
    postage,  // 运费
    supportExpress,  // 是否支持快递
    supportSelfFetch,   // 是否支持自提
    supportLocalDelivery,   // 是否支持同城送
    expressFee,   // 快递费用
    localDeliveryFee,   // 同城送费用
  }
}
```

### 事件(yunSdk.page.events[key])

<table>
<thead>
  <th>事件名</th>
  <th>入参</th>
  <th>事件类型</th>
  <th>事件说明</th>
</thead>
<tbody>
  <tr>
    <td>beforeCartSubmit</td>
    <td>
    <pre>
{ 
  goodsAlias: string,  // 商品别名
  skuId: number,  // skuid
  num: number,  // 商品数量
  messages：Object { 留言名：留言值 }
}
    </pre>
    </td>
    <td>异步事件</td>
    <td>sku选择后加入购物车，保存购物车数据之前触发</td>
  </tr>
  <tr>
    <td>afterBuy</td>
    <td>
    <pre>
{
  bookKey: string,
  buyUrl: string,  // 跳转下单页的url
  goodsAlias: string,  // 商品别名
  skuId: number,  // skuid
  num: number,  // 商品数量
  messages：Object { 留言名：留言值 }
}
    </pre>
    </td>
    <td>异步事件</td>
    <td>预下单（生成book_key）之后触发。</td>
  </tr>
</tbody>
</table>


### 流程(yunSdk.page.getProcess())

<table><colgroup><col style="width: 31.850534%;" /><col style="width: 68.14947%;" /></colgroup><tbody><tr><th><span>流程名</span></th><th><span>入参</span></th></tr><tr><td colspan="1"><p class="p1"><span>弹出<span class="s1">sku</span>流程(showSKU)</span></p></td><td colspan="1"><p><span>type 可选值如下</span></p><ul><li>selectSku: 有加入购物车和立即购买</li><li>addCart：只有加入购物车</li><li>buy: 只有下一步（购买）</li><li>present：只有下一步（赠品购买）</li><li>gift：只有下一步（送礼购买）</li><li>point：只有下一步（积分购买）</li><li>actBuy：营销购买</li></ul></td></tr></tbody></table>
