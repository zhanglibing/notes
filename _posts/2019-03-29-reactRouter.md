---
layout: post
title:  "react-router-dom"
tags:
categories:
---

### react-router-dom

```typescript jsx
import React,{Componen} from 'react'
import {
    HashRouter as Router,
    // BrowserRouter as Router,
    Route,
    Switch,
    NavLink,
}  from 'react-router-dom'


class Tab extends Componen{
    render(){
        return (
            <div>
               <p>这里是切换页</p>
               <NavLink to="/tab/tab1">tab1</NavLink>
               <NavLink to="/tab/tab2">tab2</NavLink>
               {this.props.children}
             </div>
        )
    }
}
//子路由设置
// 注意要在Tab 中添加 {this.props.children}
let childRouter=()=>{
    return (
        <Tab>
               <Switch>     
                    <Route exact path="/tab/tab1" component={Tab1}> </Route>
                    <Route exact path="/tab/tab2" component={Tab2}> </Route>
               </Switch>
        </Tab>
    )
}

//Switch 让路由职匹配一个
class a extends Component{
    render(){
        return(
            <Router>
              <Switch>
                  {/*越详细的路由越往后，权限越大的路由越往后放*/}
                  {/*给权限大的路由加一个 exact属性  精确定位*/}
                  {/*
                     路由组件的渲染方式：
                     1.component：可以渲染状态组件和无状态组件
                     2.render：只可以渲染无状态组件
                  */}
                  <Route exact path="/" component={Index}> </Route>
                  <Route exact path="/list" component={List}> </Route>
                  <Route exact path="/tab" component={childRouter}> </Route>
                  {/*和上面这个效果一样*/}
                  {/*<Route exact path="/tab" render={childRouter}> </Route>*/}
              </Switch>
            </Router>
        )
    }
}

```