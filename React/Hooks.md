

# Hooks

## Hook使用规则

不能在

- 循环、
- 条件表达式
- 任何不是定义组件的函数

中调用 *useState* ， *useEffect* 等函数。这样做是为了确保Hook总是以相同的顺序调用，如果不是这样，应用的行为就会不规则。



## state hooks





### Effect-hooks

- 默认情况下，effect是*总是* 在组件渲染之后才运行
- 默认情况下，effects 在每次渲染完成后运行，但是你可以选择只在某些值发生变化时才调用。
- *useEffect*的第二个参数用于指定effect运行的频率
  - 第二个参数是一个空数组 *[]*，那么这个effect只在组件的第一次渲染时运行。


























