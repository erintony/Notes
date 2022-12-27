# 异步组件
Vue 提供了 [`defineAsyncComponent`](https://cn.vuejs.org/api/general.html#defineasynccomponent) 方法,  返回一个外层包装过的组件，仅在页面需要它渲染时才会调用加载内部实际组件的函数，实现异步加载组件
## defineAsyncComponent 支持两种使用方式
1. 接收一个返回 Promise 的加载函数，该函数用于返回异步加载的组件
```js
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() => {
  return new Promise((resolve, reject) => {
    // ...从服务器获取组件
    resolve(/* 获取到的组件 */)
  })
})

// 或者结合import 组件导入
const AsyncComp = defineAsyncComponent(() =>｛
  // 其他操作..
 return import('./components/MyComponent.vue')
})
```
2.  配置对象使用方式
```
const AsyncComp = defineAsyncComponent({
  // 加载函数, 同第一种的加载函数
  loader: () => import('./Foo.vue'),

  // 加载异步组件时使用的组件
  loadingComponent: LoadingComponent,
  // 展示加载组件前的延迟时间，默认为 200ms
  delay: 200,

  // 加载失败后展示的组件
  errorComponent: ErrorComponent,
  // 如果提供了一个 timeout 时间限制，并超时了
  // 也会显示这里配置的报错组件，默认值是：Infinity
  timeout: 3000
})
```

## 如何做到异步加载？
我们来看源码如何实现
```ts
export function defineAsyncComponent<
  T extends Component = { new (): ComponentPublicInstance }
>(source: AsyncComponentLoader<T> | AsyncComponentOptions<T>): T {
  if (isFunction(source)) {
    source = { loader: source }
  }

  const {
    loader,
    loadingComponent: loadingComponent,
    errorComponent: errorComponent,
    delay = 200,
    timeout, // undefined = never times out
    suspensible = true,
    onError: userOnError
  } = source

  const load = (): Promise<ConcreteComponent> => {
    // 此处省略源码环境处理、转换等修饰代码
    return loader().then(comp => {
      resolvedComp = comp
      return comp
    })
  }

  return defineComponent({
    name: 'AsyncComponentWrapper',
    setup() {
      // already resolved
      if (resolvedComp) {
        return () => createInnerComp(resolvedComp!, instance)
      }
      load().then(() => {
        loaded.value = true
      })

      return () => {
        // 如果已经 load 加载完成， 并且
        if (loaded.value && resolvedComp) {
          return createInnerComp(resolvedComp, instance)
        }
      }
    }
  }) as any
}
```
可以清晰的看到，步骤如下：
- 兼容函数参数，转换成对象配置形式的参数
- 声明load方法，load方法中调用入参的加载函数，返回异步的组件对象(例如： 配置方式1中的import('./components/MyComponent.vue')返回结果)
- 返回通过defineComponent包装的组件
  - 包装组件setup中调用load函数，拿到实际要加载的组件对象
  - setup 返回函数，函数中调用createInnerComp根据 实际要加载的组件对象 创建实际组件



# 问题延伸
- setup 何时调用
- setup 返回值如何处理？ 返回的内联函数如何处理？