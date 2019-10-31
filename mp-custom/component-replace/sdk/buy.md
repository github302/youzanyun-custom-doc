## 下单页sdk

### 页面数据(yunSdk.page[key])

```javascript
// 当前配送信息
delivery: {
  expressType: 'express', // 配送方式
  address: {
    addressDetail: '翠苑街道黄龙万科中心', // 地址详细
    areaCode: '', // 区号
    country: '中国', // 国家
    province: '浙江省', // 省份
    city: '杭州市',  // 市
    county: '西湖区',  // 县/区
    lat: '',  // 维度
    lon: '',  // 经度
    recipients: '张某某', // 收货人
    tel: '156xxxxxxxx' // 收货人手机号
  },
  selfFetch: {
    // 自提点
    shop: {
      addressDetail: '',  // 地址详细
      area: '',
      province: '',  // 省份
      city: '',  // 市
      county: '',  // 县/区
      lat: '',  // 维度
      lng: '',  // 经度
      name: '自提点名称',  // 自提点名称
      tel: '自提点联系电话',  // 自提点联系电话
    },
    contact:  {
      tel: '', // 提货人联系电话
      recipients: '', //提货人姓名
    },
    time: '', // 自提时间
  }
},
// 订单支付数据
payment: {
  realPay: 9900, // 实付金额，单位：分
  postage: 600, // 邮费，单位：分
},
goodsList: [{
  title: '商品名称',
  imgUrl: 'https://img.yzcdn.cn/upload_files/2019/03/23/dd2c6bdab2c8e6b41a595dfcacf34511.jpg',
  alias: '', // 商品alias
  num: 2, // 数量
  payPrice: 100, // 商品价格
  goodsId: 111,
  skuId: 121
}]
```

### 事件(yunSdk.page.events[key])

<table width="1000px">
<thead>
  <th>事件名</th>
  <th>入参</th>
  <th>事件类型</th>
  <th>事件说明</th>
</thead>
<tbody>
  <tr>
    <td>beforeCreateOrderAsync</td>
    <td>
<pre>
{
  delivery: object,
  payment: object,
  goodsList: Array
}
</pre>
    </td>
    <td>异步事件</td>
    <td>提交订单之前事件触发。(reject会阻止创建订单)</td>
  </tr>
  <tr>
    <td>afterCreateOrder</td>
    <td>
    <pre>
{
  orderNo: string,// 订单号
}
    </pre>
    </td>
    <td>同步事件</td>
    <td>生成订单之后触发。</td>
  </tr>
  <tr>
    <td>onPayItemClickAsync</td>
    <td>
    <pre>
{
  payChannel: string,// 支付渠道
}
    </pre>
    </td>
    <td>异步事件</td>
    <td>当选择某种支付方式点击时触发。(reject会阻止支付)</td>
  </tr>
</tbody>
</table>

### 流程(yunSdk.page.getProcess())

<table>
<thead>
  <th>流程名</th>
  <th>入参</th>
  <th>流程说明</th>
  <th>返回信息</th>
</thead>
<tbody>
  <tr>
    <td>createOrder</td>
    <td>-</td>
    <td>创建订单</td>
    <td>Promise({
      orderNo：''
    }); </td>
  </tr>
   <tr>
    <td>getPayWays</td>
    <td>-</td>
    <td>
<ul>
  <li>获取支付方式  </li>
  <li>支付方式有(具体数据以实际获取为准)：  </li>
  <li>
<pre>
[{  
  pay_channel: "ALIPAY_WAP",  
  pay_channel_name: "支付宝支付"  
}, {  
  pay_channel: "BANK_CARD",
  pay_channel_name: "银行卡支付"
}, {
  pay_channel: "CASH_ON_DELIVERY",
  pay_channel_name: "货到付款"
}, {
  pay_channel: "WX_JS",
  pay_channel_name: "微信支付"
}]
</pre>
  </li>
  <li>注：生成订单号之后才能通过以下方法获取到支付方式</li>
<ul>
    </td>
    <td>Promise([{  
      payChannel: 'WX_APPLET',  
      payChannelName: '微信支付'  
    }]); </td>
  </tr>
   <tr>
    <td>startPay</td>
    <td>-</td>
    <td>开始支付</td>
    <td>Promise();</td>
  </tr>
</tbody>
</table>

例：

创建订单

```javascript
const createOrder = getProcess('createOrder');

createOrder().then(res => {
  // 订单创建成功后暴露的数据，目前里面只有 orderNo
  console.log(res.data);
  // 可用的支付方式列表
  // 接入支付扩展点 或 用户无需支付时，返回空数组
  console.log(res.payWays);
});
```

