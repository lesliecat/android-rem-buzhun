## 部分安卓机由于rem计算不准确，导致页面在这些机型下出现了水平滚动条。
    主要解决思路是：
    1.设置1rem与px的对应关系，即html元素的font-size，我这里设置为屏幕视口宽度的1/10;
    2.然后设置body的宽度为10rem;
    3.取body的计算宽度,是否与html的宽度一致，不一致则说明rem计算有误，将html的font-size按比例缩放至正确大小

```
function fixRem () {
    var html = document.documentElement,
        body = document.body,
        bodyW = body.style.width;
    body.style.width = '10rem'; // 假如页面的视口宽度为10rem
    var bodyWidth = parseInt(window.getComputedStyle(body, null).width),
        htmlWidth = parseInt(window.getComputedStyle(html, null).width);
    if (bodyWidth != htmlWidth) {
        var size =  parseInt(html.style.fontSize)* htmlWidth/bodyWidth;
        html.style.fontSize = size + 'px';
    }
    body.style.width = bodyW;
}

```


JavaScript
这里要注意，document的DOMContentLoaded事件，在某些浏览器上触发时，html元素可能还没显示，此时去取其计算高度可能为auto，所以fixRem应当放在window.onpageshow事件回调里面。