# Vuejs Comparison with Reactjs
从开发风格上对比Vuejs和Reactjs
今天这里要讨论的话题，不是前端框架技术哪家强，因为在[Vuejs官网](http://cn.vuejs.org/v2/guide/comparison.html#React)就已经有了比较全面客观的介绍，并且是中文的。
![框架排名](https://cloud.githubusercontent.com/assets/13991287/21755604/696f182c-d651-11e6-8026-145a10a475d2.png)

上图是一月份前端框架的排名，Reactjs位居第一，Vuejs排名第三。还清晰记得，去年十月份进入该showcase并未看到Vuejs，可见Vuejs 2.0有多受欢迎，而排名第二的Angularjs当时位居第一，短短数月Reactjs，Vuejs都有了比较好的成绩，Angularjs的stars没什么增长，是否可以理解为，Angularjs正在慢慢地退出这个舞台。

我也是在16年开始接触前端框架开发，对于关注度最高的Reactjs和Vuejs，想在这里谈谈两个框架在开发风格上的体会。
##Vue语法很自由，很好上手
声明一个Vue实例，与ES5中声明一个类，并使用prototype定义方法的方式很类似。而Reactjs组件，需要先理解每个生命周期函数的意义，这多少需要些成本，在实际开发之前，总不能get到该在何时调用生命周期函数。

#####Vuejs#####
```javascript
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
```javascript
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
```jsx
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
```html
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
```
```javascript
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

##Reactjs为什么很少用ref##
我是先用了Vuejs再用Reactjs的，它们都有一个强大的功能，组件！组件可以扩展 HTML 元素，封装可重用的代码，提高了我们的开发效率。从维护成的角度，组件的质量决定了产品的质量，但是从组件的封装力度上，我一开始更喜欢Vuejs，甚至还得出了这样的结论：Reactjs组件像是UI组件，Vuejs组件更接近对象。直到最近看了facebook文档，才发现另有蹊跷。先看看之前用Vuejs，我是如何去创建一个List组件，并且父组件是如何调用的。
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

```javascript
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
```jsx
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

细心的同学可能发现了，Reactjs也有[ref属性](http://reactjs.cn/react/docs/more-about-refs.html#the-ref-callback-attribute)，它可以让父组件调用子组件的方法，但实际项目中却很少看到，为什么大家都这么一致呢？我查了一下文档，[原来facebook不推荐过度使用ref](https://facebook.github.io/react/docs/refs-and-the-dom.html#dont-overuse-refs)
>Your first inclination may be to use refs to "make things happen" in your app. If this is the case, take a moment and think more critically about where state should be owned in the component hierarchy. Often, it becomes clear that the proper place to "own" that state is at a higher level in the hierarchy. See the Lifting State Up guide for examples of this.
官方还有个[栗子](https://facebook.github.io/react/docs/lifting-state-up.html)，这里我也举个比较常见的
![举个栗子](https://cloud.githubusercontent.com/assets/13991287/22298063/2b702c1c-e35a-11e6-81e5-503452256480.png)
基于上面的栗子，比如现在列表数据多啦！需要显示列表数据的条数！我们可以抽离一个显示条数的组件Counts。如果按照Vuejs的实现方法（好吧！这里好像在黑Vuejs，其实是我的误解），该组件会有plus()和minus()方法，每新增一条数据，需要在父组件的add()中显示调用Counts.plus()来update计数，删除时也需要在List中dispatch一个事件，告诉父组件已经delete，父组件收到通知，再显示调用Counts.minus()进行update，且不说这Counts组件复杂，这数据流来来回回的，代码放久了回来都要看好久才明白！但是Reactjs的实现方法就没有这个问题，Counts组件只需要一个count属性代表显示的数字，父组件把this.state.list.length传入就可以了，这种方式是不是很清晰。虽然Reactjs的这种方式，在不需要与其他组件共享数据的时候，调用起来确实很繁琐，但业务这种事情真的很难说，很多意想不到的情况都会发生，上面的栗子，指不定后期还要新加一个分页组件呢，所以我也悬崖勒马，以后不管在Vuejs中还是Reactjs，都少用ref，给自己的代码留条后路！
##父子组件间通信
