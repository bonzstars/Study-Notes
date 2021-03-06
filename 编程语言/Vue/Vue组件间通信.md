### 父组件向子组件通信

1. props
使用props，父组件可以使用props向子组件传递数据

2. $children
使用$children可以在父组件中访问子组件

### 子组件向父组件通信

1. $emit 自定义事件
父组件向子组件传递方法，子组件通过$emit触发事件，数据通过回调传给父组件

2. 通过修改父组件传递的props来修改父组件数据（引用类型）

这种方法只能在父组件传递一个**引用变量**时可以使用，字面变量无法达到相应效果。因为饮用变量最终无论是父组件中的数据还是子组件得到的props中的数据都是指向同一块内存地址，所以修改了子组件中props的数据即修改了父组件的数据。

但是并不推荐这么做，并不建议直接修改props的值，如果数据是用于显示修改的，在实际开发中我经常会将其放入data中，在需要回传给父组件的时候再用事件回传数据。这样做保持了组件独立以及解耦，不会因为使用同一份数据而导致数据流异常混乱，只通过特定的接口传递数据来达到修改数据的目的，而内部数据状态由专门的data负责管理。

3. $parent
使用$parent可以访问父组件中的数据

4. $refs
在父组件通过$refs属性获取子组件的引用

### 非父子组件、兄弟组件之间的数据传递

1. eventBus

```js
/*新建一个Vue实例作为中央事件总嫌*/
let event = new Vue();

/*监听事件*/
event.$on('eventName', (val) => {
    //......do something
});

/*触发事件*/
event.$emit('eventName', 'this is a message.');
```

### 复杂的单页应用数据管理

1. 使用vuex进行数据状态管理

