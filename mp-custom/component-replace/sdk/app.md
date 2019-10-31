###数据
yunSdk.app
```
{
  // 店铺信息
  shop: {
    kdtId: number, // 店铺ID
  },
  // 用户信息
  user: {
    gender:	string,	// 性别
    avatar:	string,	// 头像
    mobile:	string,	// 联系电话
    nickName:	string,	// 昵称
    openId: string,
  }
}
```

###页面导航
- 使用方法：  
yunSdk.navigate(pageName, args, type = 'navigateTo')
  - pageName: 页面英文名称
  - args: 页面参数
  - type: 跳转类型, 可选值：'navigateTo' || 'redirectTo' || 'navigateBack' || 'reLaunch'，含义同微信小程序API  
  
- 已开放跳转页面：

<table>
<tr>
  <th>页面英文名称</th>
  <th>参数</th>
  <th>页面中文名称</th>
</tr>
<tr>
  <td>login</td>
  <td>登录页</td>
  <td>{ redirectUrl: '', // 登录后跳转页面 }</td>
</tr>
<tr>
  <td>goodsDetail</td>
  <td>商品详情页</td>
  <td>{ alias: 'xxxxx' }</td>
</tr>
<tr>
  <td>buy</td>
  <td>下单页</td>
  <td>{ bookKey: 'xxxxx' }</td>
</tr>
<tr>
  <td>cart</td>
  <td>购物车页面</td>
  <td>-</td>
</tr>
<tr>
  <td>userCenter</td>
  <td>个人中心页面</td>
  <td>-</td>
</tr>
<tr>
  <td>orderList</td>
  <td>订单列表页面</td>
  <td>
<pre>
空：全部订单
{
  type: 'topay', // 待付款
  type: 'tosend', // 待发货
  type: 'send', // 已发货
  type: 'toevaluate', // 待评价
}
</pre>
  </td>
</tr>
<tr>
  <td>refundList</td>
  <td>退款/售后页面</td>
  <td>-</td>
</tr>
<tr>
  <td>cardPackages</td>
  <td>卡包页面</td>
  <td>-</td>
</tr>
<tr>
  <td>userCounponList</td>
  <td>我的券码页面</td>
  <td>-</td>
</tr>
<tr>
  <td>addressList</td>
  <td>收货地址页面</td>
  <td>-</td>
</tr>
<tr>
  <td>userSettings</td>
  <td>个人信息页面</td>
  <td>-</td>
</tr>
<tr>
  <td>accountSettings</td>
  <td>帐号设置页面</td>
  <td>-</td>
</tr>
</table>