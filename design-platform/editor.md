### 组件编辑器定制
#### 基本介绍

开发店铺装修模版编辑器

#### 文件目录

```bash
├──src                      src目录
  ├── common                公共组件
  ├── editors               开发者开发的组件编辑器代码，**开发者在这里开发**
  ├── utils                 相关的工具方法
├── build                   webpack打包配置
├── node_modules            node_modules
└── dist                    本地打包文件
```

编辑器组件创建后会在editors目录下生成对应目录
```bash
├── ...                      
├── editors               开发者开发的组件编辑器代码，**开发者在这里开发**
    ├── extension-xxx     组件目录
        ├── editor.js     **编辑器代码文件**
        └── index.js      入口文件   
└── ...
```
#### 本地开发

- npm install 安装依赖
- npm run dev 开启本地编辑器开发
- npm run build 打包编辑器组件代码

#### 如何开发一个编辑器组件

**Editor 需要继承 `src/common/editor-base`，子类需要重写一些方法和属性：**

- render 实例方法
- info 静态属性，这是组件的基本信息，必须要有的字段有：
  - type: 组件类型，PC端会根据这个type来渲染对应的editor
  - name: 组件名字
  - icon: 组件图标
  - maxNum: 组件可以使用的最大个数
  - extensionImage: 给预览占位的图片
- getInitialValue 静态方法，创建一个新组件实例时的默认值。
  - 【注意】必须要有type值，并且要和info属性里的type一致！
- validate 静态方法，对表单做校验，返回一个 Promise，将所有错误的一个 `map` resolve 出来，没有错就返回一个空对象。
- type的值在创建组件时已生成，请勿改动，与文件名保持一致

**`src/common/editor-base`基类提供了一些基本方法：**
- `onInputChange` 
  - 封装了处理标准 Input 组件 onChange 事件的回调；
  - 使用时需要确保 onChange 抛出来的 `Event` 对象上有 `targte.name`, `target.value` 以及 `preventDefault` 和 `stopPropagation`。
  - 使用示例
  ``` jsx
  <Input
    name="content"
    value={value.content}
    onChange={this.onInputChange}
    onBlur={this.onInputBlur}
  />
  ```
- `onCustomInputChange` 
  - 提供了更加基础的 onChange 事件处理功能，适用于那些非标准 Input 组件。
  - 使用示例
  ``` jsx
  // this.onCustomInputChange('goods'), goods字段将作为商品数据的key
  <GoodsSelector
    onCustomInputChange={this.onCustomInputChange('goods')}
    value={value}
    globalConfig={globalConfig}
    showError={showError}
    validation={validation}
  />
  ```

- `onInputBlur` 和 `onCustomInputBlur` 提供了处理 blur 事件的功能。

**Editor 有如下几个重要 props：**
- value，实例当前的值
- validation，当前的错误信息，是个对象，key 对应表单里的 Input name。
- showError，是否强制显示所有错误，如果为 `true`，Editor 必须把当前所有错误显示出来。这个一般是传给editor-common里面的ControlGroup组件。具体用法可以参考demo示例
- onChange，用于回写 value，一般用不到，已经封装好了，建议使用 `onInputChange`/`onCustomInputChange`。

#### 编辑选择器

除了暴露出来的`editor-common`中的基础编辑器UI，我们还暴露了三个基本的编辑选择器`editorSelectors`：

- CouponSelector    -- 优惠券选择器
- GoodsSelector     -- 商品选择器
- GoodsTagSelector  -- 商品分组选择器

#### 原则

**开发原则和要点，开发者要特别关注！！**

* 样式支持sass，一般会写.scss
* 原则上除了lodash以外不需要在dependencies上增加额外的依赖，使用`zent`和`editor-common`和提供的选择器API就可以满足编辑器的开发工作。增加额外的依赖会使打包出来的js文件变得很大。
* 项目工程集成了eslint校验, 会在git提交的时候校验js语法规范，如果实在不需要的话，可以使用`git commit 'xxx' -n`来忽略校验报错。建议还是使用eslint校验。
* 在执行`npm run build`时，在打包编辑器代码的时候，会监测打包的代码是否用了es6的高级语法，比如`let, const`等，会有报错提示，务必重视。千万不要把含有高级预发的js文件上传上去，否则会有兼容性的问题。一般的，我们会对项目下的编辑器代码进行babel转码，因此是不会出现高级语法。出现高级预发的唯一可能是用的npm包里面有高级语法。因此还是建议不要lodash以外不需要在dependencies上增加额外的依赖。

#### 开发示例
1. 基础开发示例，可参考自动生成组件文件中示例
2. 选择器使用示例
``` javascript
import React from 'react';
import { DesignEditor } from '../../common/editor-base';
import { ComponentTitle } from 'editor-common';
// 引入商品选择器组件
import GoodsSelector from '../../common/goods-selector';

export default class Editor extends DesignEditor {
  render() {
    const { value, globalConfig, showError, validation } = this.props;

    return (
      <div className="decorate-third-goods-editor">
        <ComponentTitle
          name="商品"
          url="https://help.youzan.com/qa?cat_sys=K#/menu/2211/detail/11788?_k=3mic0j"
        />
        {/* 使用商品选择器组件 */}
        <GoodsSelector
          onCustomInputChange={this.onCustomInputChange('goods')}
          value={value}
          globalConfig={globalConfig}
          showError={showError}
          validation={validation}
        />
      </div>
    );
  }

  // 组件信息
  static info = {
    type: 'extension-demo-test',
    name: '测试',
    description: '',
    icon: 'https://img.yzcdn.cn/public_files/2019/02/12/c52262f634e5fbba92f69abb8d7134ee.png',
    maxNum: 20,
    usedNum: 0,
    status: '',
    extensionImage:
      'https://img.yzcdn.cn/public_files/2019/08/27/b73cb3ec7dec0b327b411975e8ca0029.png',
  };

  static getInitialValue() {
    return {
      type: 'extension-demo-test',
    };
  }

  static validate(value) {
    return new Promise(resolve => {
      const errors = {};

      resolve(errors);
    });
  }
}
```

