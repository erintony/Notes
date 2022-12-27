# 环境
vue项目，页面有搜索、筛选项等。
# 需求
页面跳转，切换或者刷新，希望可以记住用户在页面的筛选状态
# 方案v1
vue有提供一种缓存组件的解决方案 — keep-alive。
缓存不活动的组件实例，而不是销毁它们。
```
    <keep-alive>
      <router-view  v-if="$route.meta.keepAlive" class="app-middle-content"/>
    </keep-alive>
    <router-view  v-if="!$route.meta.keepAlive" class="app-middle-content"/>
```
我们可以使用keep-alive包括路由组件，去缓存页面状态。
但是，这样并不能满足刷新页面依旧可以记住页面状态的需求。因为刷新浏览器页面的时候，等于是刷新了整个vue实例应用，所有vue缓存的数据都会丢失。

# 方案V2
使用History API。当页面查询条件改变时，将页面条件更新到当前历史状态。

```js
// stateObj: 页面条件状态对象, title: 标题
history.replaceState(stateObj, "title");
```
然后，在页面加载后，获取当前历史state状态，并做为初始参数
```
export default {
  data () {
    params: {}
  },
  created() {
    this.params = history.state;
  }
}
```