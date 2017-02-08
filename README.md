# Reactjs Comparison with Vuejs
这里要讨论的话题，不是前端框架哪家强，因为在[Vuejs官网](http://cn.vuejs.org/v2/guide/comparison.html#React)就已经有了比较全面客观的介绍，并且是中文的。
![框架排名](https://cloud.githubusercontent.com/assets/13991287/21755604/696f182c-d651-11e6-8026-145a10a475d2.png)

上图是一月份前端框架排名，Reactjs位居第一，Vuejs排名第三。还清晰记得，16年十月份进入该showcase并未看到Vuejs，可见Vuejs 2.0有多受欢迎，而排名第二的Angularjs当时位居第一，短短数月Reactjs，Vuejs都有了比较好的成绩，Angularjs的stars没什么增长，是否可以理解为，Angularjs正在慢慢地退出这个舞台。

对于关注度最高的Reactjs和Vuejs，想在这里谈谈两个框架在开发风格上的差异。Vuejs升级到[2.0](https://cn.vuejs.org/v2/guide/migration.html#FAQ)之后，有越来越多的特性向Reactjs靠近，导致我可以写的东西越来越少。
##Vuejs更容易上手##
Vuejs更容易上手！这是真的吗？我书读的少，作者是想支持国产吗？

Vuejs的语法很自由，比如：
- 不需要分清state和props的区别，反正this都可以get
- 不需要认识太多的生命周期函数，可能只关心mounted和Vue.nextTick（保证this.$el在document中）
- 熟悉的前端模板
- 父子组件间通信更灵活
- slot，可以大尺度地扩展组件（但也不要过度使用哦）
- v-model，mvvm的方式处理表单更方便

从入门学习一个框架的角度看，少了一些规则，门槛就降低了。

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
由于Vuejs遵循mvvm模式，v-model属性支持数据双向绑定，v-model说白了就是（value的单向绑定 + onChange事件监听）的语法糖，但这个味道还不错吧。比起在Reactjs表单需要绑定多个onChange事件确实要方便得多。前提是不引入第三方架构（FLUX/Redux）下进行对比的，现实中在创建中大型单页面应用才会用到这些框架。

##父子组件间通信

写到这我竟然有点不知所措，因为Vuejs2.0已经[废弃dispatch](https://cn.vuejs.org/v2/guide/migration.html#dispatch-和-broadcast-替换)，这个之前让我一直很喜欢，觉得在父子组件间通信能力完爆Reactjs的特性，由于基于组件树结构的事件流方式让人难以理解，并且在组件结构扩展过程中变得越来越脆弱，如果构建小型应用，建议使用[global event bus](http://vuejs.org/v2/guide/components.html#Non-Parent-Child-Communication)，它还可以有效地解决兄弟节点之间的通信问题，但个人觉得除了这点，其它都比dispatch low，因为你不知道当前监听的事件是哪里emit的而要全局搜索代码。
##JSX vs Templates##
刚接触Reactjs，因为用惯了javascript 模板引擎，一直坚信视图与功能逻辑分离是正确的选择，突然看到JSX把html写在js里，内心是拒绝的！

facebook官方好像知道大家对JSX有偏见，在文档一开始就给出[解释](http://reactjs.cn/react/docs/displaying-data.html#jsx-syntax)

> We strongly believe that components are the right way to separate concerns rather than "templates" and "display logic." We think that markup and the code that generates it are intimately tied together. Additionally, display logic is often very complex and using template languages to express it becomes cumbersome.

在这里结合我的理解翻译一下， Reactjs 团队坚信定义一个组件，正确的方法是通过功能或者关注点来区分，而不是前端模板或者展示逻辑。我们认为前端模板和组件代码是紧密相连的。另外，模板语言经常让展示的逻辑变得更复杂。

看到这我欣然接受了，有谁在写前端模板的时候，没有掺杂任何逻辑的，这不是违背了MVC原则吗！facebook觉得这种“分离”让问题更复杂，不如把模板和逻辑代码结合到一块。而开发者一开始不接受JSX，是受到传统js拼接字符串模板的死板方式影响，其实JSX更灵活，[它在逻辑能力表达上完爆模板，但也很容易写出凌乱的render函数，不如模板直观](https://www.zhihu.com/question/31585377)。

##Reactjs为什么很少用ref##

在实际项目中，我是先用了Vuejs，后一个项目才用了Reactjs，它们都有一个强大的功能，组件！组件可以扩展 HTML 元素，封装可重用的代码，提高了我们的开发效率。从维护成的角度，组件的质量决定了产品的质量，基于高质量的组件开发出来的功能，交互上的bug都会比较少。但是从组件的封装力度上，我一开始更喜欢Vuejs，甚至还得出了这样的结论：Reactjs组件像是UI组件，Vuejs组件更接近对象。直到最近看了facebook文档，才发现另有蹊跷。先看看之前用Vuejs，我是如何去创建一个List组件，并且父组件是如何调用的。

没用过ref的同学，可以看下[文档](https://facebook.github.io/react/docs/refs-and-the-dom.html#dont-overuse-refs)，不过看下面的代码，也能知道ref的作用。
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

基于上面的栗子，比如现在列表数据多啦！需要在列表顶部显示有多少条数据！我们可以定义一个显示条数的组件Counts。如果按照Vuejs的实现方法（好吧！这里好像在黑Vuejs，其实是我一开始的误解），Counts组件要监听两个事件（plus & minus），在事件中更新条数，当List进行add()或delete()需要emit plus/minus，且不说这Counts组件复杂，这事件流很难追溯，代码放久了看着都晕晕的！但是Reactjs的实现方法就没有这个问题，Counts组件只需要一个count属性代表显示的数字，父组件把this.state.list.length作为参数传入就可以了，这种方式就是是不是很清晰。虽然Reactjs的这种方式，在不需要与其他组件共享数据的时候，调用起来确实很繁琐，但业务这种事情真的很难说，很多意想不到的情况都会发生，上面的栗子，指不定后期还要新加一个分页组件呢，所以我也悬崖勒马，以后不管在Vuejs中还是Reactjs，都少用ref，给自己的代码留条后路！

上面不是在黑Vuejs，只不过是在对比两种实现组件的方式！总结一下，当组件之间有共享数据时，该数据与操作该数据的逻辑，应该放在最接近它们的父组件，这样子组件的逻辑会更合理，更清晰，而使用ref会违背这个规则。

