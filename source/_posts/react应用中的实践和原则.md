---
layout:
  - page
title: react应用中的实践和原则
date: 2022-04-25 11:17:42
tags:
---

### React 应用中的模式和原则

##### React 的设计思想

###### React 的基础原则

1. React 界面完全由数据驱动；

   修改数据，由数据去驱动 React 来修改界面。我们开发者要做的，就是设计出合理的数据模型，让我们的代码完全根据数据来描述界面应该画成什么样子，而不必纠结如何去操作浏览器中的 DOM 树结构。

2. React 中一切都是组件；

   1. 用户界面就是组件；

   2. 组件可以嵌套包装组成复杂功能；

   3. 组件可以用来实现副作用。

      一个组件可以什么都不画，或者把画界面的事情交给其他组件去做，自己做一些和界面无关的事情，比如获取数据。

3. props 是 React 组件之间通讯的基本方式。

   1. 如果一个父组件有话要对子组件说，也就是，想要传递数据给子组件，则应该通过 props。

   2. 如果子组件有话要同父组件说，那应该支持函数类型的 props。让父组件传递一个函数类型的 props 进来，当子组件要传递数据给父组件时，调用这个函数类型 props，就把信息传递给了父组件。

   3. 如果两个完全没有关系的组件之间有话说，

      业界对于这种场景，往往会采用第三方数据管理工具来解决

​ 不依赖于第三方工具，React 也提供了自己的跨组件通讯方式，这种方式叫 Context。

​ 提供者模式

##### 组件实践

###### 组件实践：如何定义清晰可维护的接口

1. 设计原则
   1. 保持接口小，props 数量要少
   2. 根据数据边界来划分组件，充分利用组合（composition）
   3. 把 state 往上层组件提取，让下层组件只需要实现为纯函数。

##### 组件设计模式

###### 组件设计模式：容器组件和展示组件

1. 为什么要分割聪明组件和傻瓜组件

   1. 把获取和管理数据的逻辑放在父组件，也就是聪明组件；把渲染界面的逻辑放在子组件，也就是傻瓜组件。

   2. 可以灵活地修改数据状态管理方式，比如，最初你可能用 Redux 来管理数据，然后你想要修改为用 Mobx，如果按照这种模式分割组件，那么，你需要改的只有聪明组件，傻瓜组件可以保持原状。

2. 怎么提高傻瓜组件的性能

   1. PureComponent

      React 提供了一个简单的实现工具 PureComponent，可以满足绝大部分需求。但是只是浅层次的比较。来决定是否渲染

      ```
      class Joke extends React.PureComponent {
        render() {
          return (
            <div>
              <img src={SmileFace} />
              {this.props.value || 'loading...' }
            </div>
          );
        }
      }
      ```

   2. React.memo

      ```
      const Joke = React.memo(() => (
          <div>
              <img src={SmileFace} />
              {this.props.value || 'loading...' }
          </div>
      ));
      ```

###### 组件设计模式：高阶组件

1.  应用场景

    对于很多网站应用，有些模块都需要在用户已经登录的情况下才显示。比如，对于一个电商类网站，“退出登录”按钮、“购物车”这些模块，就只有用户登录之后才显示，对应这些模块的 React 组件如果连“只有在登录时才显示”的功能都重复实现，那就浪费了。

2.  说明

    它接受至少一个 React 组件为参数，并且能够返回一个全新的 React 组件作为结果，当然，这个新产生的 React 组件是对作为参数的组件的包装，所以，有机会赋予新组件一些增强的“神力”。

    一个简单的高阶组件，高阶组件的命名一般都带 `with` 前缀

    ```
    const withDoNothing = (Component) => {
      const NewComponent = (props) => {
        return <Component {...props} />;
      };
      return NewComponent;
    };
    ```

    1. 高阶组件不能去修改作为参数的组件，高阶组件必须是一个纯函数，不应该有任何副作用。
    2. 高阶组件返回的结果必须是一个新的 React 组件，这个新的组件的 JSX 部分肯定会包含作为参数的组件。
    3. 高阶组件一般需要把传给自己的 props 转手传递给作为参数的组件。

    ```
    const LogoutButton = () => {
      if (getUserId()) {
        return ...; // 显示”退出登录“的JSX
      } else {
        return null;
      }
    };
    const ShoppintCart = () => {
      if (getUserId()) {
        return ...; // 显示”购物车“的JSX
      } else {
        return null;
      }
    };

    const withLogin = (Component) => {
      const NewComponent = (props) => {
        if (getUserId()) {
          return <Component {...props} />;
        } else {
          return null;
        }
      }

      return NewComponent;
    };

    const LogoutButton = withLogin((props) => {
      return ...; // 显示”退出登录“的JSX
    });

    const ShoppingCart = withLogin(() => {
      return ...; // 显示”购物车“的JSX
    });
    ```

3.  高阶组件的高级用法

    高阶组件只需要返回一个 React 组件即可，没人规定高阶组件只能接受一个 React 组件作为参数，完全可以传入多个 React 组件给高阶组件。

    ```
    const withLoginAndLogout = (ComponentForLogin, ComponentForLogout) => {
      const NewComponent = (props) => {
        if (getUserId()) {
          return <ComponentForLogin {...props} />;
        } else {
          return <ComponentForLogout{...props} />;
        }
      }
      return NewComponent;
    };

    const TopButtons = withLoginAndLogout(
      LogoutButton,
      LoginButton
    );
    ```

4.  链式调用高阶组件

        	1. 高阶组件不得不处理 `displayName`，不然 debug 会很痛苦。当 React 渲染出错的时候，靠组件的 displayName 静态属性来判断出错的组件类，而高阶组件总是创造一个新的 React 组件类，所以，每个高阶组件都需要处理一下 displayName。
        	1. 果内层组件包含定制的静态函数，这些静态函数的调用在 React 生命周期之外，那么高阶组件就必须要在新产生的组件中增加这些静态函数的支持，这更加麻烦。
        	1. 但是如果真的一大长串高阶组件被应用的话，当组件出错，你看到的会是一个超深的 stack trace，十分痛苦。
        	1. 使用高阶组件，一定要非常小心，要避免重复产生 React 组件

###### 组件设计模式：render props 模式

高阶组件并不是 React 中唯一的重用组件逻辑的方式，在这一小节中，我们会介绍另一种方式 render props。

1. render props

   ```
   const RenderAll = (props) => {
     return(
        <React.Fragment>
        	{props.children(props)}
        </React.Fragment>
     );
   };

   <RenderAll>
      {() => <h1>hello world</h1>}
   </RenderAll>

   ```

2. 传递 props

   ```
   const Login = (props) => {
     const userName = getUserName();

     if (userName) {
       const allProps = {userName, ...props};
       return (
         <React.Fragment>
           {props.children(allProps)}
         </React.Fragment>
       );
     } else {
       return null;
     }
   };

    <Login>
      {({userName}) => <h1>Hello {userName}</h1>}
    </Login>
   ```

3. 不局限于 children

   ```
   const Auth= (props) => {
     const userName = getUserName();

     if (userName) {
       const allProps = {userName, ...props};
       return (
         <React.Fragment>
           {props.login(allProps)}
         </React.Fragment>
       );
     } else {
       <React.Fragment>
         {props.nologin(props)}
       </React.Fragment>
     }
   };

    <Auth
       login={({userName}) => <h1>Hello {userName}</h1>}
       nologin={() => <h1>Please login</h1>}
     />

   ```

4. 依赖注入

   逻辑 A 依赖于逻辑 B，如果让 A 直接依赖于 B，当然可行，但是 A 就没法做得通用了。依赖注入就是把 B 的逻辑以函数形式传递给 A，A 和 B 之间只需要对这个函数接口达成一致就行，如此一来，再来一个逻辑 C，也可以用一样的方法重用逻辑 A。

   在上面的代码示例中，`Login` 和 `Auth` 组件就是上面所说的逻辑 A，而传递给组件的函数类型 props，就是逻辑 B 和 C。

###### 组件设计模式：提供者模式

1. 所谓 Context 功能，就是能够创造一个“上下文”，在这个上下文笼罩之下的所有组件都可以访问同样的数据
2. React v16.3.0 之后的提供者模式

###### 组件设计模式：组合组件

1. 应用组合组件的往往是共享组件库，把一些常用的功能封装在组件里，让应用层直接用就行。在 antd 和 bootstrap 这样的共享库中，都使用了组合组件这种模式。
2. https://juejin.cn/book/6844733754326401038/section/6844733754431258632

##### React 的状态管理

###### 组件状态

1. 为什么要了解 React 组件自身状态管理

   1. React 组件自身的状态管理是基础，其他第三方工具都是在这个基础上构筑的，连基础都不了解，无法真正理解第三方工具。

   2. 对于很多应用场景，React 组件自身的状态管理就足够解决问题，犯不上动用 Redux 和 MobX 这样的大杀器，简单问题简单处理，可以让代码更容易维护。

2. 组件自身状态 state

3. state 改变引发重新渲染的时机

   并不是一次 `setState` 调用肯定会引发一次重新渲染，每次 setState 函数调用都会往 React 的任务队列里放一个任务，多次 setState 调用自然会往队列里放多个任务。React 会选择时机去批量处理队列里执行任务，当批量处理开始时，React 会合并多个 setState 的操作，比如上面的三个 setState 就被合并为只更新 state 一次，也只引发一次重新渲染。

   因为这个任务队列的存在，React 并不会同步更新 state，所以，在 React 中，setState 也不保证同步更新 state 中的数据。

###### Redux 使用模式

1. Redux 和 React 没有任何直接关系，Redux 可以用来管理 React 的状态，也可以用来管理其他应用的状态

2. 理解 Redux
