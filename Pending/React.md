## 常见项目结构

### 前端文件目录

| 目录       | 描述                           |
| ---------- | ------------------------------ |
| assets     | 所有静态资源文件               |
| components | 复用的组件                     |
| hooks      | 自定义的hook函数               |
| routes     | 有关路由的文件夹               |
| types      | 复用的类型/接口定义            |
| utils      | 自定义的纯函数                 |
| pages      | 页面文件夹-对应有api/组件/网页 |
| config     | 全局常量                       |

### 辅助开发插件

- Eslint：	代码格式检查

- Husky： git提交自动化工作流

- Jest：  集成测试

- css预处理器：

  - less
  - scss
  - tailwind

- 状态管理：

  - redux

  - mobx

- Drizzle： 更安全的数据库调用

    



## Typescript

### 概述

TypeScript（简称 TS）是微软公司开发的一种基于 JavaScript （简称 JS）语言的编程语言

TS是JS的超集，在兼容JS的基础上，他完善了JS<u>类型系统</u>，并增加了许多特性

使用TS，可以使用**`tsc`** 编译生成`.js`文件，使用**`tsx`** 热加载并运行TS文件



### 类型系统

#### 类型的意义

类型是人为添加的一种编程约束和用法提示，它能

- 帮助开发者进行错误排查，尽早发现错误
- 完善函数的定义，提示开发者如何使用
- 利于发挥IDE提示、补全、纠错功能

JS的变量是<u>动态类型</u>，变量的类型、属性缺乏约束

TS引入了<u>静态类型</u>，他需要额外的工作量去编写类型声明，但更加规范、稳定

TS可能会丧失灵活性，增加工作量，这使得他不一定适合短期、小型的项目

但是由于纠错、重构、合作的巨大改进，大多情况下TS是值得的，特别是大型项目

#### 类型声明

```typescript
let foo:string;  //在变量后使用冒号进行类型声明
let num = 123;  //未进行声明时会自动进行类型判断

function toString(num:number||null):string {  //函数的类型声明
  return String(num);						//使用联合类型
}

let s:unknown = 'hello';  //使用unknow的顶层类型，需要在判断后进行调用
if (typeof s === 'string') {
  s.length; // 正确
}

let rep = requst() as Response //强制确定类型
```

对于复用的类型，可以用`type`进行声明

#### 类型判断

未进行声明的变量，编译器会自动推断变量类型

如果无法推断，他会将变量标记为**`any`**，他是所有类型的全集！（同时会污染正常的变量）

如果不想被推断为**`any`**，可以开启`noImplicitAny`编译选项



### 基础类型

#### 原始类型

- boolean
- string
- number
- bigint
- symbol

上面这五种原始类型的值，都有对应的**包装对象**（wrapper object）

包装对象，指的是这些值在需要时，会自动产生的对象。

```javascript
const s = new String('hello');
typeof s // 'object'
s.charAt(1) // 'e'
```

#### 对象类型

小写的`object`类型代表 JavaScript 里面的狭义对象，即对象、数组和函数

```javascript
obj = { foo: 123 };
const {val:foo} = obj; //解构赋值用于直接从对象中提取属性。
let { x: foo, y: bar }
  : { x: string; y: number } = obj; //指定x,y类型
```

#### 函数类型

`(  ) => (  )`是函数的标识

```typescript
const repeat = (
  str:string="",		//可以使用默认值
  times?:number			//使用？表示可选
):string => str.repeat(times);  //():表示返回值

function sum(
  { a, b, c }: {
     a: number;
     b: number;
     c: number			//变量解构/对象类型
  }
) {console.log(a + b + c);}
```



## React

React的设计哲学是**组件化**

### 基础语法

```typescript
import React, {useState} from 'react';
import { useNavigate } from 'react-router-dom';  //用于路由跳转

const Welcome = ()=> {
  const [inputValue, setInputValue] = useState('');  //创建钩子
  const handleSend = async () => {			
    if (inputValue.trim()) {
      const userName = inputValue.trim();
      navigate("/room", { state: userName });
    }
  };

  return (
    <div className="welcome-container">
          <div className="form-group">
            <input type="text"
            className="form-input"
            value={inputValue}
            onChange={(e) => setInputValue(e.target.value)} />	//传递输入框的值
          </div>
          <button type="submit" className="submit-button" onClick={()=>handleSend()}>提交</button>
    </div>
  );};

export default Welcome;
```

#### 组件化

目前主流的设计理念是函数化组件

组件会具有从上到下的树状结构关系，函数、参量可以有上到下传递

在`tsx`中，使用` <ComponentName value={...} />`使用组件并传递参量

 使用`export` 关键字使此函数可以在此文件之外访问。`default` 关键字表明它是文件中的主要函数。



### 钩子函数

#### useState

`useState`用于创建一个可更新的状态量，它可以在组件中自上而下传递

使用例如：`const [inputValue（值）, setInputValue（更改函数）] = useState(初始值);`

可以读取值，但不能直接赋值，而是使用更改函数进行替换（如果要合并，使用`[... Newdata]`）

#### useRef

`useRef`用于创造一个引用，它可以让函数实时获取最新的、相同的值

使用`const ref = useRef(初始值)`进行初始化，使用`.current`获取/修改其值

修改`ref`并不会导致组件重新渲染

#### useEffect

`useEffect`用于处理副作用（side effects），即那些影响到组件外部环境的操作，其语法如下：

```typescript
useEffect(() => {
  ...  // 副作用逻辑
  return () => {
  ...  // 清理逻辑
  };
}, [dependencies]);//触发依赖
```

- 副作用逻辑：每次触发时会执行的函数
- 清理逻辑：使用清理逻辑清除遗留效果
- 触发依赖：
  - 空数组 `[]`: 仅在组件挂载（触发副作用）和卸载（触发清理）时执行。
  - 依赖项变化时: 当依赖项中的某个值发生变化时，<u>先执行旧逻辑的清理函数</u>，然后执行新的副作用。

然而`useEffect`的功能比较初级，对许多情况下推荐使用集成的第三方库

#### useSWR

使用`import useSWR, { mutate }  from 'swr';`进行引入

基础语法: `const {data, error, isLoading} = useSWR(key, fetcher，{...设置})`

`key`是一个状态量唯一的标识，`fetcher`会接受`key`并获取数据，其返回值会自动更新状态量

设置中可以传入如`refreshInterval:300`，对自动更新间隔等进行更新

可以全局使用由`key`标记的状态量

可以全局使用`mutate(key)`的方式，立即进行更新并重新渲染

（注意，由于使用`mutate`更新与获取数据是异步的，需要使用`await`等方式延后`mutate`的更新）



### 设计哲学

- 将 UI 拆解为组件层级结构
- 使用 React 构建一个静态版本
- 找出 UI 精简且完整的 state 表示与位置
- 添加数据流逻辑



 

## TRPC

### 选择TRPC

对于非TS语言的后端开发，会出现重复的代码和繁琐的前后端对接

在开发小型项目时，可以考虑使用TS全栈开发后端，减少工作量

在这里可以使用Trpc作为前后端的连接器，非常地方便



### 对象声明

引入库

```typescript
import { initTRPC } from '@trpc/server';
import * as trpcExpress from '@trpc/server/adapters/express';  //不同模板
import express from 'express';
import cors from 'cors';
```

创造对象

```typescript
const createContext = ...
type Context = Awaited<ReturnType<typeof createContext>>;
const t1 = initTRPC.context<Context>().create();
```



### 创建路由函数

```typescript
const roomRouter = t1.router({
  list: t1.procedure
  .query(async () => {
    const roomList = await db.room.getRoomList();
    return responseCreater(roomList);
  }),
  add: t1.procedure
  .input(res => res)
  .mutation(async (opts) => { 
    const input = opts.input as inter.RoomAddArgs;
    const output = await db.room.addRoomList(input);
    return responseCreater(output);
  }),
  message: roomMessageRouter,
});
```

创建路由函数后，赋予属性函数，由`t1.procedure`开始

对于`GET`请求使用`query`，对于`POST`请求使用`mutation`

也可以直接使用路由嵌套

使用`export type RoomRouter = typeof roomRouter;`将类型暴露，让前端引入



### 创造后端服务

```typescript
const app = express(); //使用express
app.use(cors(...));  //创造跨域名限制
app.use(
  '/api/room',
  trpcExpress.createExpressMiddleware({
    router: roomRouter,
    createContext,
  }),
);					//绑定路由
app.listen(3000); //监听接口
```

启动服务后，前端调用如下：

```typescript
const room_trpc = createTRPCProxyClient<RoomRouter>({
  links: [
    httpBatchLink({
      url: `${API_URL}/api/room`,
    }),
  ],
});
```

