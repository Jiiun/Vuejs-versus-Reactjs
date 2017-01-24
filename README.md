# Vuejs Comparison with Reactjs
从开发风格上对比Vuejs和Reactjs
今天这里要讨论的话题，不是前端框架技术哪家强，因为在[Vuejs官网](http://cn.vuejs.org/v2/guide/comparison.html#React)就已经有了比较全面客观的介绍，并且是中文的。
![preview](https://cloud.githubusercontent.com/assets/13991287/21755604/696f182c-d651-11e6-8026-145a10a475d2.png)

上图是一月份前端框架的排名，Reactjs位居第一，Vuejs排名第三。还清晰记得，去年十月份进入该showcase并未看到Vuejs，可见Vuejs 2.0有多受欢迎，而排名第二的Angularjs当时位居第一，短短数月Reactjs，Vuejs都有了比较好的成绩，Angularjs的stars没什么增长，是否可以理解为，Angularjs正在慢慢地退出这个舞台。

我也是在16年开始接触前端框架开发，对于关注度最高的Reactjs和Vuejs，想在这里谈谈两个框架在开发风格上的体会。
##Vue语法很自由，很好上手
声明一个Vue实例，与ES5中声明一个类，并使用prototype定义方法的方式很类似。而Reactjs组件，需要先理解每个生命周期函数的意义，这多少需要些成本，在实际开发之前，总不能get到该在何时调用生命周期函数。

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
##表单在Reactjs中的蛋疼之处##

Reactjs和Vuejs如何拿input的value，先上代码
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
由于Vuejs遵循mvvm模式支持数据双向绑定，v-model说白了就是（value的单向绑定 + onChange事件监听的语法糖），但这个味道我喜欢。比起在Reactjs表单需要绑定多个onChange事件确实要方便得多。当然这里是在不引入第三方架构（FLUX/Redux）的前提下进行对比的，现实中也有很多项目是这样的。

##JSX vs Templates##
好吧，我承认标题是来自[Vue官网（对比React框架）](https://cn.vuejs.org/v2/guide/comparison.html#HTML-amp-CSS)。刚接触Reactjs，然后用惯了javascript 模板引擎，一直坚信视图与功能逻辑分离是正确的选择，突然看到JSX把html写在js里，觉得很low有木有！

对于JSX的偏见，[facebook官方](http://reactjs.cn/react/docs/displaying-data.html#jsx-syntax)在最开始就给出了解释

> We strongly believe that components are the right way to separate concerns rather than "templates" and "display logic." We think that markup and the code that generates it are intimately tied together. Additionally, display logic is often very complex and using template languages to express it becomes cumbersome.

在这里结合我的理解翻译一下， Reactjs 团队坚信定义一个组件，正确的方法是通过功能或者关注点来区分，而不是前端模板或者展示逻辑。我们认为前端模板和组件代码是紧密相连的。另外，模板语言经常让展示的逻辑变得更复杂。

看到这我欣然接受了，使用前端模板，有些逻辑写在模板里会更合理，但却违背了MVC原则，常常徘徊在这种不知所措的矛盾之间。React团队觉得这样问题更复杂，不如把模板和逻辑代码结合到一块。而开发者一开始不接受JSX，是受到传统js拼接字符串模板的死板方式影响，其实JSX更灵活。

[JSX和模板都是个人偏好问题，JSX在逻辑能力表达上完爆模板，但也很容易写出凌乱的render函数，不如模板直观。](https://www.zhihu.com/question/31585377)

##Reactjs的组件像是UI组件，Vuejs的组件更接近对象##
Reactjs和Vuejs都有一个强大的功能，组件！组件可以扩展 HTML 元素，封装可重用的代码，提高了我们的开发效率。从维护成的角度，组件的质量决定了产品的质量，但是从组件的封装力度上，我更喜欢Vuejs。我们先看看Vuejs是怎样创建一个List组件，父组件是如何调用的。
#####Vuejs#####
```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>

<div id="demo">
  <input
    :value="input"
    v-model="input"
    />
  <button
    @click="add"
    >
    add
  </button>
  <List
    ref="list"
  />
</div>
```

```js
var List = Vue.extend({
	props: {
  	list: {
    	type: Array,
      default: function(){return []}
    }
  },
	template:'<div><ul v-for="(item, index) in list"><li>{{item.name}} <i @click="deleteItem(item, index)">delete</i></li></ul></div>',
  data: function(){
  	return{
    	input: ''
    }
  },
  methods: {
  	addItem: function(name){
    	this.list.push({name: name})
    },
  	deleteItem: function(item, index){
    	this.list.splice(index, 1)
    }
  }
})

Vue.component('List',List)

new Vue({
    el: '#demo',
    data: {
    	input: ''
    },
    methods: {
    	add: function(){
      	this.$refs.list.addItem(this.input)
      }
    }
})
```
#####再看看Reactjs是怎么做的#####
```
class List extends React.Component{
  _delete(index){
    this.props.onDelete && this.props.onDelete(index)
  }
  render() {
    return (
    	<ul>
      {
      	this.props.list.map((item, index)=>{
          return (
            <li 
              key={index}
            >
             	{item} 
              <i onClick={this._delete.bind(this, index)}>
              	delete
              </i>							
            </li>
          )
        })
      }
      </ul>
    );
  }
};

class Page extends React.Component{
  constructor(props){
    super(props)
    this.state={
    	input: '',
      list: []
    }
  }
  _bindChange(e){
    this.setState({
      input: e.target.value
    })
  }
  _add(){
    this.state.list.push(this.state.input)
    this.forceUpdate()
  }
  _delete(index){
    this.state.list.splice(index, 1)
    this.forceUpdate()
  }
  render() {
    return (
      <div>
        <input
          onChange={this._bindChange.bind(this)}
          value={this.state.input}
          />
        <button
          onClick={this._add.bind(this)}
          >
          add
        </button>
        <List
          list={this.state.list}
          onDelete={this._delete.bind(this)}
        />
      </div>
    );
  }
};

ReactDOM.render(
  <Page/>,
  document.getElementById('container')
);
```
通过上面两段代码可以看出，在调用List组件的时候，Reactjs比Vuejs复杂的多，不仅仅是多了onChange，包括新增和删除的逻辑，都必须在父组件中实现，这样会导致项目中有多个地方调用List组件，都必须实现这套相似的逻辑，而这套逻辑在Vuejs中是封装在组件里的，所以给我的感觉，Reactjs像UI组件，而Vuejs更接近对象。

从实现的角度，其实Vuejs比Reactjs多了ref属性，实际上Reactjs也有[ref](http://reactjs.cn/react/docs/more-about-refs.html#the-ref-string-attribute)，但facebook并不推荐这种写法，原因在这个[commit](https://github.com/facebook/react/commit/5ee8a93280987bf1547687f5d8665be89058f321#all_commit_comments)给大家回复了
##父子组件间通信
