# Reactjs Comparison with Vuejs
这里要讨论的话题，不是前端框架哪家强，因为在 [Vuejs 官网](http://cn.vuejs.org/v2/guide/comparison.html#React)就已经有了比较全面客观的介绍，并且是中文的。
![框架排名](https://cloud.githubusercontent.com/assets/13991287/22885137/01f98ac6-f233-11e6-8935-6b7a56236d20.png)

上图是二月份前端框架排名，Reactjs 位居第一，Vuejs 排名第三。还清晰记得，16 年十月份该 showcase 首页并未看到 Vuejs，如今已有 4000+ stars，那时的 Reactjs 也差不多这个成绩，可见 Vuejs 2.0 有多受关注，而排名第二的 Angularjs 当时位居第一，短短数月 Reactjs、Vuejs 都有比较好的成绩，而 Angularjs 的 stars 没有明显增长，是否可以断章取义，Angularjs 正在慢慢地退出这个舞台。

对于近期关注度最高的 Reactjs 和 Vuejs，想在这里谈谈两个框架在开发风格上的差异。Vuejs 升级到 [2.0](https://cn.vuejs.org/v2/guide/migration.html#FAQ) 之后，有更多的特性向 Reactjs 靠拢，所以可以写的越来越少，下面将在几个细节上作对比。
##Vuejs更容易上手##
Vuejs 更容易上手！这是真的吗？我书读的少，作者是想支持国产吗？

Vuejs 的语法很自由，比如：
- data 和属性都是通过 this 直接访问
- 不需要认识太多的生命周期函数，可能只关心 mounted 和 Vue.nextTick（保证 this.$el 在 document 中）
- 熟悉的前端模板
- 父子组件间通信更灵活
- slot，可以大尺度地扩展组件（但也不要过度使用哦）
- v-model，mvvm 的方式处理表单更方便

从入门学习一个框架的角度看，少一些规则多一些自由空间，门槛会降些。

##表单在 Reactjs 中的蛋疼之处##

Reactjs 和 Vuejs 如何拿 input 的 value，先上代码
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
Vuejs 进行表单处理的方式是不是更简洁，由于 v-model 属性支持数据双向绑定，说白了就是（value 的单向绑定 + onChange 事件监听）的语法糖，但这个味道还不错吧，比起在 Reactjs 中需要绑定多个 onChange 事件确实要方便得多。

##父子组件间通信

父组件通知子组件都是通过 props 逐层传递，而子组件向上通信的方式会有些差异。父组件将函数传给子组件，Reactjs 把数据作为该函数的参数，并调用该函数来实现向上通信（Vuejs 也可以做到）。这样做的坏处是，当组件跨层级通信时，需将函数逐层往下传递，往回追溯的话比较麻烦，过程也比较繁琐。而 Vuejs 通过（on + dispatch）组合的方式实现通信，这样当组件跨级通信时比较方便，dispatch 可以将消息逐层往上派发，直到触发某一个监听函数时停止。

但写到这我竟有点不知所措，因为 Vuejs2.0 已经[废弃 dispatch](https://cn.vuejs.org/v2/guide/migration.html#dispatch-和-broadcast-替换)，这个之前让我一直很喜欢，觉得在父子组件间通信能力完爆 Reactjs 的特性跟我们说白白了，官方给出的理由是，基于组件树结构的事件流方式让人难以理解，并且在组件结构扩展过程中变得越来越脆弱。在 Vuejs 2.0 中构建小型应用，建议使用 [global event bus](http://vuejs.org/v2/guide/components.html#Non-Parent-Child-Communication)，它还可以有效地解决兄弟节点之间的通信问题，但个人觉得除了这点，其它都比不上 dispatch，因为我不知道当前监听的事件是在哪里被触发，最后只能全局搜索代码。
##JSX vs Templates##
刚接触 Reactjs，因为用惯了javascript 模板引擎，一直坚信视图与功能逻辑分离是正确的选择，突然看到 JSX 把 html 写在 js 里，内心是拒绝的！

Facebook官方好像知道大家对 JSX 有偏见，在文档一开始就给出[解释](http://reactjs.cn/react/docs/displaying-data.html#jsx-syntax)

> We strongly believe that components are the right way to separate concerns rather than "templates" and "display logic." We think that markup and the code that generates it are intimately tied together. Additionally, display logic is often very complex and using template languages to express it becomes cumbersome.

在这里结合我的理解翻译一下， Reactjs 团队坚信一个组件的正确用途是 "separate concerns"，而不是前端模板或者展示逻辑。我们认为前端模板和组件代码是紧密相连的。另外，模板语言经常让展示的逻辑变得更复杂。

刚开始没弄明白什么是 "separate concerns"，其实现在也... Facebook 可能是在强调组件应该从功能上去抽象定义，而不仅仅从视觉上区分。

看完官方答复我欣然接受了，有谁在写前端模板的时候，没有掺杂业务逻辑的，掺杂了不就违背 MVC 吗！Facebook 觉得这种“分离”让问题更复杂，不如把模板和逻辑代码结合到一块。而开发者一开始不接受 JSX，是受到传统js拼接字符串模板的死板方式影响，其实 JSX 更灵活，[它在逻辑能力表达上完爆模板，但也很容易写出凌乱的render函数，不如模板直观](https://www.zhihu.com/question/31585377)。

##Reactjs为什么很少用ref##

Reactjs 和 Vuejs 都有一个强大的特性，组件！组件可以扩展 HTML 元素，封装可重用的代码，提高了我们的开发效率，并且在组件本身质量有保障情况下，后续新功能也趋于稳定，bug 较少。在实际开发项目中，可能 Vuejs 先入为主，一开始觉得它的组件封装力度比 Reactjs 强，甚至还得出了这样的结论：Reactjs 组件像是 UI 组件，Vuejs 组件更接近对象。直到最近看了 Facebook 文档，才发现另有蹊跷。先看看之前用 Vuejs ，我是如何去创建一个列表（List）组件，并实现列表数据的新增和删除，以及调用 List 组件的方式。

没用过 ref 的同学，可以先看下[文档](https://facebook.github.io/react/docs/refs-and-the-dom.html#dont-overuse-refs)，不过看完下面代码也能大概知道 ref 的作用。
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
#####再看看 Reactjs 是怎么做的#####
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
通过上面两段代码可以看出，在调用 List 组件的时候，Reactjs 比 Vuejs 复杂的多，不仅仅是多了onChange，包括新增和删除的逻辑，都必须在父组件中实现，这样会导致项目中有多个地方调用 List 组件，都必须实现这套相似的逻辑，而这套逻辑在 Vuejs 中是封装在组件里的，所以给我的感觉，Reactjs 比较关注组件的展示，而 Vuejs 比较关注功能。

细心的同学可能发现了，Reactjs也有[ ref 属性](http://reactjs.cn/react/docs/more-about-refs.html#the-ref-callback-attribute)，它可以让父组件调用子组件的方法，但实际项目中却很少看到，为什么大家都这么一致呢？我查了一下文档，[原来 Facebook 不推荐过度使用ref](https://facebook.github.io/react/docs/refs-and-the-dom.html#dont-overuse-refs)
>Your first inclination may be to use refs to "make things happen" in your app. If this is the case, take a moment and think more critically about where state should be owned in the component hierarchy. Often, it becomes clear that the proper place to "own" that state is at a higher level in the hierarchy. See the Lifting State Up guide for examples of this.

官方还有个[栗子](https://facebook.github.io/react/docs/lifting-state-up.html)，这里我也举个比较常见的

![举个栗子](https://cloud.githubusercontent.com/assets/13991287/22298063/2b702c1c-e35a-11e6-81e5-503452256480.png)

基于上面的栗子，比如现在列表数据多啦！需要在列表顶部显示有多少条数据！我们可以定义一个显示条数的组件 Counts。如果按照 Vuejs 的实现方法（好吧！这里好像要黑Vuejs，其实是我一开始的误解），Counts 组件要监听两个事件（plus & minus），在事件函数中更新条数，当 List 进行 add() 或 delete() 需要触发 plus / minus，且不说这 Counts 组件复杂，这事件流也很难追溯，代码放久了看着都晕晕的！但是 Reactjs 的实现方法就没有这个问题，Counts组件只需要一个 count 属性代表显示的数字，父组件把 this.state.list.length 作为参数传入就可以了，这种方式就是不是很清晰。虽然 Reactjs 的这种方式，在不需要与其他组件共享数据的时候，调用起来很繁琐，但业务这种事情真的很难说，很多意想不到的情况都会发生，基于上面的栗子，指不定后期还要新加一个分页组件呢，所以我也悬崖勒马，以后不管在 Vuejs 还是 Reactjs，尽量少用 ref，给自己的代码留条后路！

上面不是在黑 Vuejs，只不过是在对比两种实现组件的方式！总结一下，当组件之间有共享数据时，该数据与操作该数据的逻辑，应该放在最接近它们的父组件，这样子组件的逻辑会更合理，更清晰，而使用 ref 会违背这个规则。

