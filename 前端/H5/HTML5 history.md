[popstate](https://developer.mozilla.org/zh-CN/docs/Web/Events/popstate)

##HTML5 history
>DOM中的window对象通过window.history方法提供了对浏览器历史记录的读取，让你可以在用户的访问记录中前进和后退。

>在javascript中使用window.history.back(),window.history.forward(),和window.history.go()方法可以在用户的历史记录中前进和后退，这些方法和直接点击浏览器前进后退按钮的效果是一样的。

>HTML5中加入了两个新的方法：```window.history.pushState(state,null,new_url)```和```window.history.replaceState(state,null,new_url)```，前者会将当前页面作为历史页面并转到新的页面，新的页面URL为new_url，内容与原来的一致，这时候点击返回键，内容不会变，URL会变为旧的url，可以通过事件来实现无刷新的页面跳转。replaceState和与pushState类似，该方法和字面一样，不会创建新的记录，只是把当前页面的内容改变一下。