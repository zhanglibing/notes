--
   layout: post
   title:  "React高阶组件"
   tags:
   categories:
---

### 高阶组件
- 高阶组件通过包裹（wrapped）被传入的React组件，经过一系列处理，最终返回一个相对增强（enhanced）的React组件，供其他组件调用。
- 实现不同组件中的逻辑复用,
- 将一些可以单独抽离的逻辑处理给要返回的新组件里面复用

### react高阶组件有哪些分类
- 代理方式的高阶组件
   - 操作props
   - 抽取状态
   - 访问ref
   - 包装组件
   
- 继承方式高阶组件
  - 操作props
  - 操作生命周期
  
### 代理方式的高阶组件 实例 
```javascript
import React,{Component} from 'react'
export default function(WrapperComponent) {
    return class HIC extends Component{
        //设置高阶组件的名称   多处使用的时候方便区分
        static displayName=`HIC(${getDisplayName(WrapperComponent)})`
        constructor(props){
            super(props);
            this.state={
                value:''
            }
            this.onInputChange=this.onInputChange.bind(this)
        }
        onInputChange(e){
            this.setState({
                value:e.target.value
            })
        }
        render(){
            const {...otherProps} =this.props;
            //可以过滤整合props   也可以传自定义属性
            //提取公共input
            const newProps={
                value:this.state.value,
                onChange:this.onInputChange,
            }
            return (
                <WrapperComponent sex={'男'} {...newProps} {...otherProps}/>
            )
        }

    }
}

function getDisplayName(WrapperComponent){
    return WrapperComponent.displayName||WrapperComponent.name||'Component'
}



import React,{Component} from 'react'
import HIC from './high'
class High extends Component{
    constructor(props){
        super(props);
    }
    render(){
        const { value, onChange}=this.props;
        return (
            <div>
                <h1>正常组件</h1>
                <input type="text"  value={value} onChange={onChange} />
                {/*下面方法会提示警告    不要将组件的属性, 传给DOM节点。*/}
                {/*<input type="text"  {...this.props}/>*/}
            </div>
        )
    }
}
export default HIC(High)


```  

### 继承高阶组件
- 有多个页面需要执行页面初始化操作，可能是动态计算视口的宽高度、计算屏幕的分辨率，甚至为一些节点添加初始化的事件（注意页面卸载时注销），或者一些其他的http请求、验证等初始化行为。这些行为有一个特点：
`需要重复执行`或`某一时间后重复执行`。继承实现了代码的`共享与复用`。

```javascript

import React,{Component} from 'react'
export default function(WrapperComponent) {
    return class HIC extends WrapperComponent{
        //设置高阶组件的名称   多处使用的时候方便区分
        static displayName=`HIC(${getDisplayName(WrapperComponent)})`
        componentWillMount(){
            //操作生命周期
        }
        render(){
            const {user,...otherProps}=this.props;
            //操作props
            this.props=otherProps;
            return super.render()
        }

    }
}

function getDisplayName(WrapperComponent){
    return WrapperComponent.displayName||WrapperComponent.name||'Component'
}



import React,{Component} from 'react'
import HIC from './high'
class High extends Component{
    constructor(props){
        super(props);
    }
    render(){
        return (
            <div>
                <h1>正常组件</h1>
            </div>
        )
    }
}
export default HIC(High)
```