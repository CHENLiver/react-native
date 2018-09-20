
## 一、须知
技术栈 
- 核心库：React-Native@0.55.4
- 路由导航：React-Navigation@2.12.1 
- 状态管理：mobx@5.1.0
## 二、快速建立一个RN App
https://reactnative.cn/

如果RN的基础配置环境没有配置好，请点击上方链接到官网进行配置

1.首先需打开android模拟器

2.打开cmd运行以下命令

``` 
react-native init ReactNativeNavigationDemo
cd ReactNativeNavigationDemo
react-native run-android
``` 
运行后
![](https://user-gold-cdn.xitu.io/2018/8/31/1658ee99348fc688?w=391&h=773&f=png&s=66280)
>模拟器刷新 使用ctrl+m

## 三、官方 Demo 目录介绍

[react navite](https://user-gold-cdn.xitu.io/2018/8/31/1658eb9370f81f02?w=503&h=350&f=jpeg&s=11154)

上面的目录结构说明如下，重要的有：

- android/ android 原生代码（使用 android studio 要打开这个目录，如果直接打开父目录报错）
- ios/ ios 原生代码（使用 xcode 打开这个目录，如果直接打开父目录报错）
- index.js 打包 app 时进入 react native（js 部分） 的入口文件（0.49 以后安卓、ios 共用一个入口文件）
- App.js 可以理解为 react native（js 部分） 代码部分的入口文件，比如整个项目的路由在这里导入

上面是最重要的四个目录/文件，其他说明如下：

- _test_/ 测试用（暂未使用）
- app.json 项目说明，主要给原生 app 打包用，包括项目名称和手机桌面展示名称 React Native : 0.41 app.json
- package.json 项目依赖包配置文件
- node_modules 依赖包安装目录
- yarn.lock yarn 包管理文件
- 其他配置文件暂时无需改动，在此不做说明

## 四、项目结构
```
|-- android 
|-- ios
|-- node_modules
|-- src // 放置所有原始的 react native 代码
    |-- assets // 静态图片
    |-- components // 公共组件
    |-- config // 配置文件，比如路由配置
        |-- route.js //  路由配置文件
    |-- screens/ 所有页面文件 
        |-- Home // 首页
            |-- index.js //  页面
        |-- Category // 分类页
    |-- store // 状态管理  
|-- index.js // 主入口
```
>注意页面文件的命名方式：大驼峰命名法，react native 推荐组件命名用大驼峰命名法，每个页面相当于一个组件。

## 五、配置路由
使用官方推荐 react navigation 管理路由

react navigation 常用 API 有三个：
- createStackNavigator 每个新屏幕放置在堆栈顶部的屏幕之间转换的方法
- createMaterialTopTabNavigator 点击路线或水平滑动来切换不同的路由
- createSwitchNavigator 一次只显示一个页面。 默认情况下，它不处理返回操作，并在你切换时将路由重置为默认状态。

### 5.1 createStackNavigator 实现页面间跳转
目录结构

![](https://user-gold-cdn.xitu.io/2018/9/18/165eb869ce1fc987?w=628&h=355&f=png&s=18808)

1）首先配置简单的路由：路由文件 route.js 此时内容如下，
```
import React, { Component } from 'react';
import { createSwitchNavigator } from "react-navigation";
import Home from "../screens/Home"; //页面
import ScreenHome from "../screens/ScreenHome"; //页面
import ScreenTab2 from '../screens/ScreenTab2'; //页面
import ScreenTab3 from '../screens/ScreenTab3'; //页面
import ScreenTab4 from '../screens/ScreenTab4'; //页面
// 配置路由
const AppStack  = createStackNavigator({
  Home: {
    screen: Home
  },
  ScreenHome: {
    screen: ScreenHome
  },
  ScreenTab2: {
    screen: ScreenTab2
  },
  ScreenTab3: {
    screen: ScreenTab3
  },
  ScreenTab4: {
    screen: ScreenTab4
  },
});
export default () => <AppNavigator />;
```
2）更新 App.js，对接路由文件：
```
import React, { Component } from 'react';
import { Text, View } from 'react-native';
import Route from './src/config/route';

export default class HelloWorldApp extends Component {
  render() {
    return <Route />;
  }
}
```

3）具体页面设置，以 Home 为例 新建index.js文件，其他页面内容一样
```
import React, { Component } from 'react';
import { Text, View } from 'react-native';


export default class Home extends Component {
  static navigationOptions = {
    title: '首页',
  };
  render() {
    return <Text>Hello</Text>;
  }
}
```
在 idnex.js 中在具体元素上定义具体跳转页面
```
import React, { Component } from 'react';
import { Text, View, Button } from 'react-native';

export default class Home extends Component {
  static navigationOptions = {
     title: "首页"
  };
  render() {
  	 const { navigate } = this.props.navigation;
    return (
    	<View>
    		<Text>Hello, Navigati</Text>
    		<Button title="Go to Jane's profile" onPress={() => navigate('ScreenTab4')}/>
    	</View>
    );
  }
}
```
### 5.2 createMaterialTopTabNavigator 实现页面底部 tab 切换
首先在 screens 目录下新建 Tabbar 页面，用于配置 createMaterialTopTabNavigator 。每个 tab 对应一个页面，

![](https://user-gold-cdn.xitu.io/2018/9/18/165eba59ecab6051?w=562&h=240&f=png&s=13042)
1）没有 tab 图标的最简配置
此时只需要配置 Tabbar 里面的 index.js 文件就好，如下：
```
import React, { Component } from 'react';
import { createMaterialTopTabNavigator, createStackNavigator } from "react-navigation";
import Home from "../Home";
import ScreenHome from "../ScreenHome";
import ScreenTab2 from "../ScreenTab2";
import ScreenTab3 from '../ScreenTab3';

//解决title显示问题
const FeedStack = createStackNavigator({
  FeedHome: Home,
  /* other routes here */
});

const Tabbar = createMaterialTopTabNavigator(
  // 配置 tab 路由
  {
    Home: {
      screen: FeedStack,
      navigationOptions:{
         tabBarLabel:'首页',
      }
    },
    ScreenHome: {
      screen: ScreenHome
    },
    ScreenTab2: {
      screen: ScreenTab2
    },
    ScreenTab3: {
      screen: ScreenTab3
    }
  },
  // 其他配置选项
  {
    tabBarPosition: "bottom"
  }
);

//解决title空白问题，清除标题栏
Tabbar.navigationOptions = {
  // Hide the header from AppNavigator stack
  header: null,
};


export default Tabbar;
```

2）路由文件 route.js 修改
```
**import React, { Component } from 'react';
import { createSwitchNavigator, createStackNavigator } from "react-navigation";

import Tabbar from '../screens/Tabbar';
import ScreenTab4 from '../screens/ScreenTab4';

// 配置路由
const AppNavigator  = createStackNavigator({
  Tabbar: {
	 screen: Tabbar
  },
  ScreenTab4: {
    screen: ScreenTab4
  },
});

export default () => <AppNavigator />;

```
### 5.3 自定义组件替换标题
目录结构

![](https://user-gold-cdn.xitu.io/2018/9/18/165ebb8361aa9519?w=577&h=135&f=png&s=6666)

1）新建 LogoTitle 在index.js内容
```
import React, { Component } from 'react';
import { Text, View } from 'react-native';

export default class LogoTitle extends Component {
  render() {
    return (
      	<View>
      		<Text>首页</Text>
      	</View>
    );
  }
}
```
2）修改Home模块内容
```
import React, { Component } from 'react';
import { Text, View, Button } from 'react-native';
import LogoTitle from "../../components/LogoTitle";
export default class Home extends Component {
  static navigationOptions = {
     headerTitle: <LogoTitle />,
  };
  render() {
  	 const { navigate } = this.props.navigation;
    return (
    	<View>
    		<Text>Hello, Navigati</Text>
    		<Button title="Go to Jane's profile" onPress={() => navigate('ScreenTab4')}/>
    	</View>
    );
  }
}
```
## 六、身份验证流程
请参考 https://reactnavigation.org/docs/zh-Hans/auth-flow.html
1）路由文件 route.js 修改
```
import React, { Component } from 'react';
import { createSwitchNavigator, createStackNavigator } from "react-navigation";

import Tabbar from '../screens/Tabbar';
import ScreenTab4 from '../screens/ScreenTab4';
import SignInScreen from '../screens/SignInScreen';
import AuthLoadingScreen from '../screens/AuthLoadingScreen';



// 配置路由
const AppStack  = createStackNavigator({
  Tabbar: {
	 screen: Tabbar
  },
  ScreenTab4: {
    screen: ScreenTab4
  },
});

const AuthStack = createStackNavigator({ SignIn: SignInScreen });

const AppNavigator =  createSwitchNavigator(
  {
    AuthLoading: AuthLoadingScreen,
    App: AppStack,
    Auth: AuthStack,
  },
  {
    initialRouteName: 'AuthLoading',
  }
);

export default () => <AppNavigator />;

```
2）路由文件 SignInScreen  内容
```
import React, { Component } from 'react';
import { AsyncStorage, Button, View, StyleSheet } from 'react-native';

export default  class SignInScreen extends Component {
  static navigationOptions = {
    title: 'Please sign in',
  };

  render() {
    return (
      <View style={styles.container}>
        <Button title="Sign in!" onPress={this._signInAsync} />
      </View>
    );
  }

  _signInAsync = async () => {
    await AsyncStorage.setItem('userToken', 'abc');
    this.props.navigation.navigate('App');
  };
}
const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```
3）路由文件 AuthLoadingScreen  内容
```
import React, { Component } from 'react';
import {
  ActivityIndicator,
  AsyncStorage,
  StatusBar,
  StyleSheet,
  View,
} from 'react-native';

export default class AuthLoadingScreen extends Component {
  constructor(props) {
    super(props);
    this._bootstrapAsync();
  }

  // Fetch the token from storage then navigate to our appropriate place
  _bootstrapAsync = async () => {
    const userToken = await AsyncStorage.getItem('userToken');

    // This will switch to the App screen or Auth screen and this loading
    // screen will be unmounted and thrown away.
    this.props.navigation.navigate(userToken ? 'App' : 'Auth');
  };

  // Render any loading content that you like here
  render() {
    return (
      <View style={styles.container}>
        <ActivityIndicator />
        <StatusBar barStyle="default" />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```







