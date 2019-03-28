--
   layout: post
   title:  "React高阶组件"
   tags:
   categories:
---

### 高阶组件

高阶组件通过包裹（wrapped）被传入的React组件，经过一系列处理，最终返回一个相对增强（enhanced）的React组件，供其他组件调用。

### 简单实例
```javascript
const Demo = (props) => {
  return (
    <div>
	  My name is {props.name}
	</div>
  )
};

const HOC = (WrapperComponent) => {
  return class Permission extends Component{
    render(){
      return (
        <div>
		  <header>nihao</header>
		  <WrapperComponent {...this.props}/>
		</div>
	  )
	}
  }
};

const WithDemo = HOC(Demo);

```

### react高阶组件有哪些分类
- 属性代理和反向继承
