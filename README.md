# Vuejs Comparison with Reactjs
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
#####Reactjs#####
```
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
#####Vuejs#####
```
html
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

js
new Vue({
    el: '#demo',
    data: {
      inputA: '',
      inputB: ''
    }
})
```
由于Vuejs遵循mvvm模式，支持数据双向绑定，所以他只需在input组件中，v-model属性指向inputA、inputB，就完成了双向绑定。而React中的数据流是单向的，所以在不借助其他数据流框架的情况下，只能通过绑定dom事件来更新inputA、inputB。如果一个页面有多个input，Reactjs需要绑定多个事件。

##JSX vs Templates##
好吧，我承认标题是来自[Vue官网中对比其他框架的子标题](https://cn.vuejs.org/v2/guide/comparison.html#HTML-amp-CSS)。刚接触前端框架时，对这种页面预留一个div容器，把html模板拼凑好再一股脑塞进去的性能很担忧，大家都是过来人有木有！然后用惯了javascript 模板引擎，一直坚信视图与功能逻辑分离是正确的选择，突然看到JSX把html写在js里，觉得很low有木有！

对于第一个问题，可能更多考虑的是首屏渲染的性能问题，首屏拼凑整个页面html字符串确实开销不小，但是构建SPA每个页面都是前端计算出来的，如果为了首屏另搞一套，维护性扩展性不是很好啦。

对于JSX的偏见，[facebook官方](http://reactjs.cn/react/docs/displaying-data.html#jsx-syntax)在最开始就给出了解释

> We strongly believe that components are the right way to separate concerns rather than "templates" and "display logic." We think that markup and the code that generates it are intimately tied together. Additionally, display logic is often very complex and using template languages to express it becomes cumbersome.

在这里结合我的理解翻译一下， Reactjs 团队坚信定义一个组件，正确的方法是通过功能或者关注点来区分，而不是前端模板或者展示逻辑。我们认为前端模板和组件代码是紧密相连的。另外，模板语言经常让展示的逻辑变得更复杂。

看到这我欣然接受了，使用前端模板，有些逻辑写在模板里会更合理，但却违背了MVC原则，常常徘徊在这种不知所措的矛盾之间。React团队觉得这样问题更复杂，不如把模板和逻辑代码结合到一块。而开发者一开始不接受JSX，是受到传统js拼接字符串模板的死板方式影响，其实JSX比它更灵活。

##Reactjs的组件像是UI组件，Vuejs的组件更接近对象##
Reactjs和Vuejs都有一个强大的功能，组件！组件可以扩展 HTML 元素，封装可重用的代码，提高了我们的开发效率。从维护成的角度，组件的质量决定了产品的质量，
##父子组件间通信
##Vuex和Redux
