## CompositionAPI

### setup()

##### 返回值

**-返回对象**

* 暴露给模板、组件实例
* 选项式api钩子通过this访问组件实例，可以访问到setup中暴露的属性与方法，如果与optionsAPI有重名，setup优先

**-返回渲染函数**

##### 参数

**-第一个参数：props**

* 组件外部传递进来的，并被组件内部接收了的属性

* 一个 `setup` 函数的 `props` 是响应式的，并且会在传入新的 props 时同步更新
* 解构props对象后 解构出的变量会丢失响应性（推荐通过`props.x`来使用）
* 使用`toRefs()/toRef` —— 解构props对象并保持响应性/将某个prop传到外部函数并保持响应性

**-第二个参数：上下文对象context**

* 上下文对象是非响应性的，可以安全解构
* attrs、slots、emits
  * attrs与slots均是有状态的对象，总是随组件更新而更新——避免解构attrs与slots（通过attrs.x，slots.x使用）
  * attrs与slots的属性都是非响应性的
* expose
  * 用于显式地限制该组件暴露的属性，父组件通过模板引用`ref`访问组件实例时，仅能访问到expose函数暴露出的内容
  * 当setup()返回一个渲染函数时会阻止我们返回组件的属性与方法等，此时可以通过expose()将需要的东西暴露给父组件

##### 执行时机

* setup在`beforeCreate`之前执行（此时this为undefined，因此通过this获取不到组件实例，setup中获取实例可通过`getCurrentInstance`方法）
* setup调用发生在data、computed、methods解析之前

### 响应式API

ref()

* 用于创建一个包含响应式数据的引用对象（对象内部只有一个指向其内部值的属性value)
* 操作数据时需要通过.value，模板中读取数据时则不需要
* 接收的数据：基本数据类型和引用数据类型（定义引用类型时，内部会通过reactive转为代理对象）

* 对于基本数据类型：使用set和get进行数据劫持然后进行响应式；对于对象，会通过reactive（用proxy进行数据劫持）

reactive()

* 接收一个对象或数组，返回一个代理对象（proxy对象）
* reactive函数定义的响应式数据是深层次的，将会影响所有嵌套属性

