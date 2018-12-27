##  问题描述：
IOS系统，h5页面，当div处于编辑状态时，弹出ios系统自带的软键盘，点击键盘的换行按键，会在光标下方出现空白行，而光标位置不变，软键盘消失后光标位置也不能移动。
## 解决思路：
禁止IOS键盘换行功能，捕获点击换行按钮事件，然后自己处理换行操作
## 具体操作：

```

<template>
    <div contenteditable="true"  @keydown="keydownHandle($event)"></div>
</template>

export default{
    methods:{
        keydownHandle(e) {
            //判断没有按alt键 且 按钮为换行键
            if (!e.altKey && e.keyCode === 13) {
             //判断系统是否为ios
              if (getSys() === 'ios') {
                e.preventDefault(); //阻止原生端的默认换行事件
                let selection = window.getSelection(); // 获取光标的位置
                let range = selection.getRangeAt(0); // 获取选中的内容
                if (!!range.toString()) {
                  // 有选中内容，换行是删除
                  range.deleteContents();
                }
                let br = document.createTextNode('\n');
                range.insertNode(br); // 插入换行
                restoreSelection(range, false);
              }
            }
        }
    }
    function restoreSelection(range, type) {
      if (typeof type === 'boolean') {
        range.collapse(type); //将选中的内容光标折叠，参数为true则光标移至选中内容最前端，否则移至尾部
      }
      let selection = window.getSelection();
      if (selection) {
        try {
          selection.removeAllRanges(); // 清空所有Range
        } catch (error) {
          document.body.createTextRange().select();
          document.selection.empty();
        }
        // 将处理好的range对象添加至selection对象中
        selection.addRange(range)
      }
    },
    function getSys(){
      let u = navigator.userAgent;
      let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Adr') > -1; //android终端
      let isiOS = /iphone/gi.test(navigator.userAgent); //ios终端
      if(isAndroid){
        return 'android';
      }
      if(isiOS){
        return "ios";
      }
    },
}
```
