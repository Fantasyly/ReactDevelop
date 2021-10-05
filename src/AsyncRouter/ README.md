实现react懒加载路由并添加监听
asyncRouter实际就是一个高级组件,将()=>import()作为加载函数传进来，然后当外部Route加载当前组件的时候，在componentDidMount生命周期函数，加载真实的组件，并渲染组件，我们还可以写针对路由懒加载状态定制属于自己的路由监听器beforeRouterComponentLoad和afterRouterComponentDidLoaded，类似vue中 watch $route 功能。接下来我们看看如何使用。

使用的方式如下：
```jsx
import AsyncRouter ,{ RouterHooks }  from './asyncRouter.js'
const { beforeRouterComponentLoad} = RouterHooks
const Index = AsyncRouter(()=>import('../src/page/home/index'))
const List = AsyncRouter(()=>import('../src/page/list'))
const Detail = AsyncRouter(()=>import('../src/page/detail'))
const index = () => {
  useEffect(()=>{
    /* 增加监听函数 */  
    beforeRouterComponentLoad((history)=>{
      console.log('当前激活的路由是',history.location.pathname)
    })
  },[])
  return <div >
    <div >
      <Router  >
      <Meuns/>
      <Switch>
          <Route path={'/index'} component={Index} ></Route>
          <Route path={'/list'} component={List} ></Route>
          <Route path={'/detail'} component={ Detail } ></Route>
          <Redirect from='/*' to='/index' />
       </Switch>
      </Router>
    </div>
  </div>
}

```