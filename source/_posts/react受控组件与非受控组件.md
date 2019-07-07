---
title: react受控组件与非受控组件
date: 2018-09-07 23:13:51
tags: 
  - 前端
  - javascript
  - react
categories:
  - 前端 
  - react
---

# 受控组件
 >在HTML中，标签**input**、**textarea**、**select**的值的改变通常是根据用户输入进行更新。在React中，可变状态通常保存在组件的状态属性中，并且只能使用 setState() 更新，而呈现表单的React组件也控制着在后续用户输入时该表单中发生的情况，以这种由React控制的输入表单元素而改变其值的方式，称为：“受控组件”,例如下面，input的value值依赖于state
```javascript
class Demo extends React.Component {
    constructor(props) {
        super(props);
        this.state = {name: ''};
        this.handleChange = this.handleChange.bind(this);
    }

    handleChange(event) {
        this.setState({ name: event.target.value });
    };

    render() {
        return (
            <div>
                <input type="text" value={this.state.name} onChange={this.handleChange}/>
            </div>
        );
    }
}
```
# 非受控制组件
> 表单数据由DOM本身处理。即不受setState()的控制，与传统的HTML表单输入相似，input输入值即显示最新值（使用 ref 从DOM获取表单值）
```javascript
class Demo extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleSubmit(event) {
    alert('name: ' + this.input.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={(input) => this.input = input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

参考 https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/
