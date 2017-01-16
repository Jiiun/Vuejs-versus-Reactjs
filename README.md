# Vuejs-and-Reactjs
对比Vuejs和Reactjs这两个框架在开发过程中的区别
今天这里要讨论的话题，不是前端框架技术哪家强，因为在[Vuejs官网](http://cn.vuejs.org/v2/guide/comparison.html#React)就已经有了比较全面客观的介绍，并且是中文的。
![preview](https://cloud.githubusercontent.com/assets/13991287/21755604/696f182c-d651-11e6-8026-145a10a475d2.png)

上图是一月份前端框架的排名，Reactjs位居第一，Vuejs排名第三。还清晰记得，十月份进入该showcase并未看到Vuejs，可见Vuejs 2.0有多受欢迎，而排名第二的Angularjs当时位居第一，短短数月Reactjs，Vuejs都有了比较好的成绩，Angularjs的stars没什么增长，是否可以理解为，这段时间新出的项目都不再考虑Angular。

我也是在今年开始接触前端框架开发，对于今年关注度最高的Reactjs和Vuejs，想在这里谈谈在开发过程中的一些体会和差异
##Vue语法很自由，很好上手
声明一个Vue实例，与ES5中声明一个类，并使用prototype定义方法的方式很类似
#####Vuejs#####
```
var CheckBox = Vue.extend({
  props: {
    isChecked: {
      type: Bollean,
      default: false
    }
  },
  getValue: function(){
    return this.isChecked;
  },
  setValue: function(val){
    this.isChecked = val;
  }
})
```
#####ES5 类#####
```
function CheckBox(config){
  this.isChecked = config.isChecked || false;
}
CheckBox.prototype = {
  constructor: CheckBox,
  getValue: function(){
    return this.isChecked;
  },
  setValue: function(val){
    this.isChecked = val;
  }
  
}
```
##一个页面中多个input，Reactjs让你很蛋疼有木有##

Reactjs和Vuejs如何拿多个input组件的value
```
Reactjs
class Demo extends React.Component{
	constructor(props){
  	super(props)
    this.state={
    	inputA: '',
      inputB: ''
    }
  }
  _onChangeA(e){
  	this.setState({
    	inputA: e.target.value
    })
  }
  _onChangeB(e){
  	this.setState({
    	inputB: e.target.value
    })
  }
  render() {
    return (
    	<div>
      	<input 
          onChange={this._onChangeA.bind(this)} 
          value={this.state.inputA}
          />
        <input 
          onChange={this._onChangeB.bind(this)} 
          value={this.state.inputB}
          />
      </div>
    );
  }
};

ReactDOM.render(
  <Demo/>,
  document.getElementById('container')
);

```
```
Vuejs
<div id="demo">
    <input 
      v-model="inputA"
      :value="inputA"/>
    <input 
      v-model="inputB"
      :value="inputB"
      />
    <button
      @click="show"
      >
      show
    </button>
</div>

new Vue({
    el: '#demo',
    data: {
    	inputA: '',
      inputB: ''
    }
})
```
##Vue的前端模板功能强大，相对JSX语法更被接受
##Reactjs的组件像是UI组件，Vuejs的组件更接近对象
##父子组件间通信
##Vuex和Redux
