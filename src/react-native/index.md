# Khởi động React Native

## Bài 1

Bài này sẽ học về React nên qua [react.dev](https://react.dev/) để học nha.

## Bài 2

Bình thường thì sẽ là thế này:

```bash
npx create-expo-app@latest <app-name>
# Khởi tạo thì nó sẽ ra dự án default với cả đống code mà bạn không cần, nên để dọn đi thì chỉ cần chạy
npm run reset-project
# Là xong
```

Nhưng với dự án học tập và để hiểu về React Native với React thì có thể sử dụng phần dưới đây:

```bash
npx create-expo-app@latest <app-name> --template blank
# Do cái này không hỗ trợ Preview trên trình duyệt nên cần tải thêm
npx expo install react-dom react-native-web @expo/metro-runtime
# Là xong
```

Để chạy ứng dụng thì chỉ cần gõ:

```bash
npx expo start
```

What’s inside a React Native project?

```
.gitignore:
  .expo
  .vscode
node_modules:
  # Holds 3rd party packages

assets:
  # Holds images used inside app

package.json, package-lock.json:
  # Holds dependencies, script commands, etc.

app.json:
  # Configuration settings

App.js:
  # The real code
```

There’s a function component called `App`. This component acts as the root component by default. In React Native, you can’t use HTML tags in JSX code. Unlike React where your application runs on the browser, React Native application runs on mobile devices.

[React Native components documentation](https://reactnative.dev/docs/components-and-apis)

| React Native UI Component | Android View   | iOS View     | Web Analog               | Description                                                                      |
| :------------------------ | :------------- | :----------- | :----------------------- | :------------------------------------------------------------------------------- |
| `<View>`                  | `<ViewGroup>`  | `<UIView>`   | A non-scrolling `<div>`  | A container that supports layout with flexbox, style, some touch handling, and accessibility controls |
| `<Text>`                  | `<TextView>`   | `<UITextView>` | `<p>`                    | Displays, styles, and nests strings of text and even handles touch events          |
| `<Image>`                 | `<ImageView>`  | `<UIImageView>`| `<img>`                  | Displays different types of images                                               |
| `<ScrollView>`            | `<ScrollView>` | `<UIScrollView>`| `<div>`                  | A generic scrolling container that can contain multiple components and views       |
| `<TextInput>`             | `<EditText>`   | `<UITextField>`| `<input type="text">`    | Allows the user to enter text                                                    |

Trong React Native thì chúng ta sẽ sử dụng `StyleSheet Objects` (Kiểu CSS, nhưng viết bằng JavaScript).

Về cơ bản thì nó sẽ trông như thế này:

```jsx
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  }
});
```

## Bài 3

### What is Fast Refresh?

- Fast Refresh is a feature in React that updates the application instantly after editing the source code without reloading the entire page.
- Key benefits:
  - Preserves state in function components and Hooks.
  - Speeds up development and reduces interruptions.

Limitations: State is not preserved in:

- Class components.
- Modules with multiple exports besides React components.
- Higher-order components returning class components.

(Phần về Fast Refresh chắc là học qua phần lý thuyết thôi)

```jsx
import React, { Component } from "react";
import { Button, Text, View } from "react-native";

// State is not preserved in class components
class App extends Component {
  state = { count: 0 };

  increment = () => this.setState({ count: this.state.count + 1 });

  render() {
    return (
      <View>
        <Text>{this.state.count}</Text>
        <Button title="Increment" onPress={this.increment} />
      </View>
    );
  }
}
```

### Fast Refresh and Hooks

- `useState`, `useRef`: Preserve their values if the arguments are unchanged.
- `useEffect`, `useMemo`, `useCallback`: Always update during Fast Refresh, even if dependencies don’t change (Luôn được cập nhật trong Fast Refresh).

```jsx
import React, { useEffect, useMemo, useState } from "react";
import { Button, Text, View } from "react-native";

const App = ({ multiplier }) => {
  const [count, setCount] = useState(0);

  // useMemo will re-run when edited
  const double = useMemo(() => count * multiplier, [count]);

  useEffect(() => {
    console.log("Component re-rendered");
  }, []);

  return (
    <View>
      <Text>Double: {double}</Text>
      <Button title="Increment" onPress={() => setCount(count + 1)} />
    </View>
  );
};
```

> Mẹo hay: Type “?” on Terminal to see full list of command while `npm start` process is running. Press `m` to toggle a menu on device/emulator.

### Building Adaptive User Interfaces

Responsive Units:

- Use relative units like `%, vh, or vw` for widths and heights.
- Use scalable units like `em or rem` for font sizes.
- Use third-party libraries such as `react-native-size-matters` to simplify responsive development.

Using `maxWidth` or `minWidth` besides the regular width to create more responsive sizes.

### Dimensions API

```jsx
import { Dimensions } from 'react-native';

const windowWidth = Dimensions.get('window').width;
const windowHeight = Dimensions.get('window').height;

// `useWindowDimensions()` is the preferred API for React components.
const { width, height } = useWindowDimensions();
```

### Managing layout when keyboard is visible

`KeyboardAvoidingView`

```jsx
import { KeyboardAvoidingView } from 'react-native';

<KeyboardAvoidingView style={styles.screen} behavior="position">
  <TextInput .../>
</KeyboardAvoidingView>
```

### Handling user input

- Input Components:
  - `TextInput`: The core component for receiving user input (Hộp để ghi chữ vào).
  - `Button`: Used to handle submission or trigger an action.
- Events:
  - `onChangeText`: captures changes in TextInput (so that we can update the state) (Cái này để cập nhật lại thông tin vào state trong `<TextInput>`)
  - `onPress`: Bằng `onClick` bình thường.

### Core component: ScrollView

Đây là phần Data để thử nghiệm

```js
const users = [
  {
    userName: "John Doe",
    userAge: 28,
    userAddress: "123 Main St, Anytown, USA"
  },
  {
    userName: "Jane Smith",
    userAge: 34,
    userAddress: "456 Oak Ave, Othercity, USA"
  },
  {
    userName: "Alice Johnson",
    userAge: 22,
    userAddress: "789 Elm St, Yetanothertown, USA"
  },
  {
    userName: "Bob Brown",
    userAge: 45,
    userAddress: "321 Pine Rd, Somewhere, USA"
  },
  {
    userName: "Eve Davis",
    userAge: 30,
    userAddress: "654 Cedar Ln, Placesburg, USA"
  }
];
```

```jsx
<View>
  <ScrollView>
    {users.map((user, index) => (
      // Add a key prop when mapping over arrays
      <View key={index}>
        <Text>username: {user.userName}</Text>
        <Text>userage: {user.userAge}</Text>
        <Text>user addr: {user.userAddress}</Text>
      </View>
    ))}
  </ScrollView>
</View>;
```

#### FlatList

```jsx
<View style={styles.bottomSection}>
  <FlatList
    data={DATA}
    renderItem={({ item }) => <Item title={item.title} />}
    keyExtractor={item => item.id}
  />
</View>;
```

### Core component: Pressable

Tương tự `Button` hoặc `TouchableOpacity`

```jsx
<Pressable
  // Only works on Android: A feedback effect when a Pressable is touched
  android_ripple={{ color: '#cccccc' }}
  onPress={() => { console.log('Pressed') }}
>
  <Text style={styles.goalItem}>{obj.item}</Text>
</Pressable>
```

Key Features of Pressable:

- `onPress`: Handles the basic press event.
- `onPressIn`: Triggered when the press gesture starts.
- `onPressOut`: Triggered when the press gesture ends.
- `onLongPress`: Triggered when the user presses and holds the component.
- `style`: Allows you to define styles that change based on the component's state (e.g., pressed, hovered).

### Core component: Image

```jsx
// Static image:
<Image source={require('./assets/favicon.png')} />

// Network image:
<Image
  style={{ width: 64, height: 64 }}
  source={{
    uri: '[https://reactnative.dev/img/tiny_logo.png](https://reactnative.dev/img/tiny_logo.png)'
  }}
/>
```

### Core component: Modal

The Modal component is a basic way to present content above an enclosing view. It will appear above the main view.

```jsx
function TestModal() {
  const [modalVisible, setModalVisible] = useState(true);

  return (
    <Modal
      visible={modalVisible}
      onRequestClose={() => {
        Alert.alert('Modal has been closed.');
        setModalVisible(!modalVisible);
      }}
    >
      <View>
        <Text>Phần View này sẽ trôi nổi trên màn hình</Text>
        <Button title="Title" onPress={() => setModalVisible(!modalVisible)} />
      </View>
    </Modal>
  )
}
```

## Lecture 4

| Events         | Description                                            |
| :------------- | :----------------------------------------------------- |
| `onPress`      | Triggered when a user taps a button or Touchable component. |
| `onChangeText` | Fires when text changes in an TextInput field.         |
| `onSubmitEditing`| Executes when the user presses "Enter" or submits input.|
| `onLongPress`  | Activated when a user presses and holds a button.      |
| `onFocus` / `onBlur`| Used for handling focus state in input fields.       |

```jsx
import React, { useState } from "react";
import { View, Text, Button, StyleSheet } from "react-native";

const CounterApp = () => {
  const [count, setCount] = useState(0);
  return (
    <View style={styles.container}>
      <Text>Count: {count}</Text>
      <Button title="Increase" onPress={() => setCount(count + 1)} />
    </View>
  );
};

export default CounterApp;

// Assume styles object is defined elsewhere
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
});
```

- The `count` state stores a number.
- `setCount` updates the value when the button is clicked.

Syntax of `useEffect`, không hiểu lắm (Hình như là để giữ state cho external systems hoặc gì đó thôi)

```jsx
useEffect(() => {
  // Code to run on mount
  return () => {
    // Cleanup function (like componentWillUnmount)
  };
}, [dependencies]); // Dependencies control re-runs
```

### Core Component: `ImageBackground`

```jsx
<ImageBackground
  source={require("./assets/background.jpg")}
  style={styles.background}
>
  <Text style={styles.text}>Weather App</Text>
</ImageBackground>
```

### StatusBar

```jsx
<StatusBar barStyle="light-content" backgroundColor="#000" />
```

### Core component: `ActivityIndicator`

```jsx
<ActivityIndicator size="large" color="#0000ff" animating={true} />
```

### Core component: TextInput

```jsx
<TextInput
  style={{...}} // Apply your styles here
  placeholder="Enter city name"
  onChangeText={(text) => console.log(text)}
/>
```

### Gọi API trong React

Tương tự như bên JS thôi, dùng `Workspace()` để gọi API.

```jsx
const getData = async () => {
  try {
    const response = await fetch("API_URL");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error("Error fetching weather:", error);
  }
};
```

## Lecture 5

Nếu tải sử dụng template `default` của Expo thì không dùng được cái này đâu nha ;-; Tui chưa biết sửa sao nên tải template `blank` thì dùng được.

```bash
# Linux
npm install @react-navigation/native-stack @react-navigation/elements
# Bên Windows thì phải dùng lệnh này
npm install @react-navigation/native-stack; npm install @react-navigation/elements
```

### React Navigation: Static vs Dynamic APIs

#### Creating a native stack navigator (Static)

```jsx
import { createStaticNavigation } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import { View, Text } from "react-native"; // Import necessary components

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>Home Page</Text>
    </View>
  );
}

function AboutScreen() {
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>About Page</Text>
    </View>
  );
}

const RootStack = createNativeStackNavigator({
  screens: {
    Home: HomeScreen,
    About: AboutScreen,
  },
});

const Navigation = createStaticNavigation(RootStack);

export default function App() {
  return <Navigation />;
}
```

Thử tạo xong mà có thấy cái gì đâu :<. Phần tiếp theo là thêm chút Options vào nè:

```jsx
import { createStaticNavigation } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import { View, Text } from "react-native"; // Import necessary components

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>Home Page</Text>
    </View>
  );
}

function AboutScreen() {
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>About Page</Text>
    </View>
  );
}

const RootStack = createNativeStackNavigator({
  initialRouteName: "Home",
  // Cũng có thể thêm Options cho tất cả các màn hình thế lày
  screenOptions: {
    headerStyle: { backgroundColor: "tomato" },
  },
  screens: {
    Home: {
      screen: HomeScreen,
      options: {
        title: "Welcome",
      },
    },
    About: AboutScreen,
  },
});

const Navigation = createStaticNavigation(RootStack);

export default function App() {
  return <Navigation />;
}
```

Có thể mọi người sẽ hướng đến dùng Dynamic API hơn, cái trên học cho biết lý thuyết thôi :< mà chạy Preview có thấy j đâu.

#### Creating a native stack navigator (Dynamic API)

Khá lằng nhằng nên đây sẽ là Code luôn. Đầu tiên cần khai báo trước đã.

```jsx
import { NavigationContainer, useNavigation } from '@react-navigation/native'; // Import useNavigation
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { View, Text, Button } from 'react-native'; // Import Button and other necessary components

const Stack = createNativeStackNavigator();

// Define your screen components
function Product() {
  const navigation = useNavigation();
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ fontSize: 30 }}>Product Page</Text>
      <Button
        // Navigating to a new screen
        title='Click on to navigate detail page'
        onPress={() => navigation.navigate('Detail')}
      ></Button>
    </View>
  );
}

function ProductDetail() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ fontSize: 30 }}>Product Detail Page</Text>
    </View>
  );
}

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Product" component={Product} />
        <Stack.Screen name="Detail" component={ProductDetail} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

Cần phải tạo `Stack` sử dụng `createNativeStackNavigator()`. Rồi sau đó lần lượt thêm
`<NavigationContainer>` rồi `<Stack.Navigator>`, trong đó sẽ thêm một cái kiểu như
`<Stack.Screen name="Product" component={Product} />` để thể hiện một màn hình.

yep, mình chẳng hiểu mình đã viết cái gì nữa. nma cứ kệ đi nha, miễn dùng được là được. Tiếp đến là tạo một Component:

```jsx
import { useNavigation } from '@react-navigation/native';
import { View, Text, Button } from 'react-native'; // Import necessary components

function Product() {
  const navigation = useNavigation();
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ fontSize: 30 }}>Product Page</Text>
      <Button
        // Navigating to a new screen
        title='Click on to navigate detail page'
        onPress={() => navigation.navigate('Detail')}
      ></Button>
    </View>
  );
}
```

Tạo một navigation object sử dụng `const navigation = useNavigation()`, sau đó viết giùm cái nút
và thêm cái này vào `onPress`: `onPress={() => navigation.navigate('Detail')}`, chính là cái `navigation` vừa khai báo! Xong.

> warning: When using the Dynamic API, the component prop accepts a component, not a render function. Don't pass an inline function (e.g. `component={() => <HomeScreen />}`), or your component will unmount and remount, losing all state, when the parent component re-renders.

Bạn cũng có thể thêm Options vào nữa:

```jsx
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { View, Text } from 'react-native'; // Import necessary components

// Define your screen components
function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>Home Page</Text>
    </View>
  );
}

function AboutScreen() {
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>About Page</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

<NavigationContainer>
  {/* Hoặc thêm điểm khởi đầu: <Stack.Navigator initialRouteName='About'> */}
  <Stack.Navigator initialRouteName="About">
    <Stack.Screen
      name="Home"
      component={HomeScreen}
      options={{ title: "Welcome" }}
    />
    <Stack.Screen name="About" component={AboutScreen} />
  </Stack.Navigator>
</NavigationContainer>;
```

### Passing additional props

Theo lý thuyết trên Slides thì: Use React context and wrap the navigator with a context provider to pass data to the screens (recommended).

The `useContext` hook: A React Hook that lets you create context data (values) in a parent component and retrieve them from its descendant components (TODO: Không hiểu gì cả).

```jsx
// First, use the createContext function to create a Context.
import { createContext, useContext } from "react";
import { NavigationContainer } from "@react-navigation/native"; // Import necessary component
import { createNativeStackNavigator } from "@react-navigation/native-stack"; // Import necessary component
import { View, Text } from "react-native"; // Import necessary components

// Define your screen components (HomeScreen, AboutScreen) as needed

const ScreenNameContext = createContext(null);

// Then, wrap the context’s provider around the components that will use the context value.
const Stack = createNativeStackNavigator();

// Assume scrNames is defined somewhere
const scrNames = { home: 'Trang Chủ', about: 'Giới Thiệu' }; // Example scrNames

<ScreenNameContext.Provider value={scrNames}>
  <NavigationContainer>
    <Stack.Navigator>
      <Stack.Screen name="Home" component={HomeScreen} />
      <Stack.Screen name="About" component={AboutScreen} />
    </Stack.Navigator>
  </NavigationContainer>
</ScreenNameContext.Provider>;
```

Retrieving Context Data from a component

```jsx
import { useContext } from "react";
import { View, Text } from "react-native"; // Import necessary components

// Assume ScreenNameContext is imported or defined
// Assume ScreenNameContext is created with createContext(null);

// Technically, any can retrieve the context data.
// However, it’s recommended that the component is one of the context provider’s descendant.
function AboutScreen() {
  const screenNames = useContext(ScreenNameContext);
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text>{screenNames?.about}</Text> {/* Use optional chaining */}
    </View>
  );
}
```

### Difference between `Maps` and `push` (add multiple instances)

```jsx
import { useNavigation } from "@react-navigation/native";
import { Button } from "react-native"; // Import Button

// If you want to open a new instance of the About screen, use navigation.push('About').
// Each time push is called, a new About screen instance is added to the navigation stack.
function MyComponent() { // Example component
  const navigation = useNavigation();
  return (
    <Button title="About Us... again" onPress={() => navigation.push("About")} />
  );
}
```

- `Maps('About')` → Does nothing if already on the About screen.
- `Maps.push('About')` → Creates a new instance of the About screen. This approach is useful when passing unique data to each instance of a screen.

### Manually Triggering Back Navigation

Use `navigation.goBack()` to programmatically navigate to the previous screen.

```jsx
import { useNavigation } from "@react-navigation/native";
import { Button } from "react-native"; // Import Button

function MyComponent() { // Example component
  const navigation = useNavigation();
  return (
    <>
      <Button title="Go Back" onPress={() => navigation.goBack()} />
      {/* Assuming 'Home' is a screen name in your stack */}
      <Button title="Go Home" onPress={() => navigation.popToTop()} /> {/* Changed to popToTop for typical Home behavior */}
    </>
  );
}
```
*Note: `popTo("Home")` is not a standard method in `@react-navigation/native-stack`. `popToTop()` is used to go back to the first screen in the stack.*

### Ném Params khi `Maps()`

Đầu tiên, thêm cái `{}` vào sau phần tên màn hình như này:

```jsx
onPress={() => navigation.navigate("About", { name: "Quan" })}
```

Chuyển qua màn hình khác, nhận params kiểu gì:

```jsx
import { useNavigation, useRoute } from "@react-navigation/native"; // Import useRoute
import { View, Text } from "react-native"; // Import necessary components

function AboutScreen() {
  const navigation = useNavigation();
  const route = useRoute(); // Use the useRoute hook
  const { name } = route.params || {}; // Use optional chaining and default empty object
  // ... rest of your component
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text>Welcome, {name}!</Text>
    </View>
  );
}
```

Hoặc khởi tạo dùng `initialParams`

```jsx
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { View, Text } from 'react-native'; // Import necessary components

// Define AboutScreen component as shown above
function AboutScreen({ route }) {
  const { name } = route.params || {};
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text>Welcome, {name}!</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

<Stack.Screen
  name="About"
  component={AboutScreen}
  initialParams={{ name: "Us" }}
/>;
```

Hoặc cập nhật params dùng `setParams`:
```jsx
import { useNavigation } from "@react-navigation/native";
import { Button } from "react-native"; // Import Button

function MyComponent() { // Example component
  const navigation = useNavigation();
  return (
    <Button
      title="Set Name to Vinh"
      onPress={() => navigation.setParams({ name: "Vinh" })}
    />
  );
}
```

> Mẹo: Avoid using `setParams` to update screen options such as `title`. If you need to update options, use `setOptions` instead.

```jsx
// Don't do this
navigation.navigate("Profile", {
  user: {
    id: 21,
    firstName: "Jane",
    lastName: "Done",
    age: 25,
  },
});

// Do this
navigation.navigate("Profile", { userId: 21 });
```

Ở đây, bạn không nên ném dữ liệu kiểu này, mà thay vào đó, ném ID để có thể sử dụng làm địa chỉ truy cập phần dữ liệu được lưu ở chỗ khác (global store or cache).

> What should be in params?: Params should not be used for state management. If data is needed across multiple screens, it should be stored in a global store or cache.

## Lecture 6

### Ngăn chặn các Header trùng lặp

Tránh các header bị trùng lặp -> ẩn header của thành phần cha (Đặt `headerShown: false` trong `options` của `Stack.Screen` là xong):

```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { View, Text } from 'react-native'; // Import necessary components

// Assume HomeTabs component is defined elsewhere
function HomeTabs() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Home Tabs</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

// Inside your Navigator
// <Stack.Navigator>
  <Stack.Screen
    name="Home"
    component={HomeTabs}
    options={{ headerShown: false }}
  />
  // ... other screens
// </Stack.Navigator>
```

Bạn cũng có thể ẩn tất cả các header bằng cách đặt `headerShown: false` trong `screenOptions` của `Stack.Navigator`:

```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();

<Stack.Navigator screenOptions={{ headerShown: false }}>
  {/* ... your screens */}
</Stack.Navigator>
```

### Cấu hình Header Bar

#### Đặt tiêu đề cố định

Sử dụng thuộc tính `title` trong `options` để đặt tiêu đề cố định cho mỗi màn hình:

```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { View, Text } from 'react-native'; // Import necessary components

// Assume HomeScreen component is defined elsewhere
function HomeScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

// Inside your Navigator
// <Stack.Navigator>
  <Stack.Screen
    name="Home"
    component={HomeScreen}
    options={{ title: 'My home' }}
  />
  // ... other screens
// </Stack.Navigator>
```

#### Sử dụng Params trong tiêu đề

`options` nên là một hàm nhận vào `route` và trả về tiêu đề.

Đối số được truyền vào hàm `options` là một đối tượng với các thuộc tính `navigation` và `route` (Cái này thì chịu nha).

```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { View, Text } from 'react-native'; // Import necessary components

// Assume ProfileScreen component is defined elsewhere
function ProfileScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Profile Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

// Inside your Navigator
// <Stack.Navigator>
  <Stack.Screen
    name="Profile"
    component={ProfileScreen}
    options={({ route }) => ({ title: route.params.name })}
  />
  // ... other screens
// </Stack.Navigator>
```

#### Cập nhật tiêu đề với `setOptions`

Để cập nhật cấu hình `options` cho màn hình đang hoạt động từ chính component màn hình đã mount (mount là cái gì?), có thể dùng `navigation.setOptions`.

```javascript
import { useNavigation } from '@react-navigation/native';
import { Button } from 'react-native'; // Import Button

function MyComponent() { // Example component
  const navigation = useNavigation();
  return (
    <Button
      onPress={() => navigation.setOptions({ title: 'Updated!' })}
      title="Update the title"
    />
  );
}
```

### Tùy chỉnh Header Styles

Có ba thuộc tính chính để tạo kiểu cho header:

* `headerStyle`: Tạo kiểu cho nền header.
* `headerTintColor`: Đặt màu cho nút back và tiêu đề.
* `headerTitleStyle`: Tùy chỉnh các thuộc tính font cho tiêu đề.

Ví dụ:

```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { View, Text } from 'react-native'; // Import necessary components

// Assume HomeScreen component is defined elsewhere
function HomeScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

// Inside your Navigator
// <Stack.Navigator>
  <Stack.Screen
    name="Home"
    component={HomeScreen}
    options={{
      title: 'My home',
      headerStyle: { backgroundColor: '#f4511e' },
      headerTintColor: '#fff',
      headerTitleStyle: { fontWeight: 'bold' },
    }}
  />
  // ... other screens
// </Stack.Navigator>
```

#### Chia sẻ Common Options giữa các màn hình

Thay vì lặp lại các kiểu cho mỗi màn hình, bạn có thể đặt `screenOptions` trong `Stack.Navigator`.

```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { View, Text } from 'react-native'; // Import necessary components

// Assume HomeScreen and DetailsScreen components are defined elsewhere
function HomeScreen() { /* ... */ }
function DetailsScreen() { /* ... */ }

const Stack = createNativeStackNavigator();

<Stack.Navigator
  screenOptions={{
    headerStyle: { backgroundColor: '#f4511e' },
    headerTintColor: '#fff',
    headerTitleStyle: { fontWeight: 'bold' },
  }}
>
  <Stack.Screen name="Home" component={HomeScreen} options={{ title: 'My home' }} />
  <Stack.Screen name="Details" component={DetailsScreen} />
</Stack.Navigator>
```

#### Thay thế tiêu đề bằng Custom Component

Sử dụng `headerTitle` để thay thế tiêu đề văn bản bằng một custom component, chẳng hạn như hình ảnh hoặc nút.

```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { Image, View, Text } from 'react-native'; // Import necessary components

function LogoTitle() {
  return (
    <Image
      style={{ width: 50, height: 50 }}
      source={require('@expo/snack-static/react-native-logo.png')}
    />
  );
}

// Assume HomeScreen component is defined elsewhere
function HomeScreen() { /* ... */ }

const Stack = createNativeStackNavigator();

// ...
// Inside your Navigator
// <Stack.Navigator>
  <Stack.Screen
    name="Home"
    component={HomeScreen}
    options={{ headerTitle: (props) => <LogoTitle {...props} /> }}
  />
  // ... other screens
// </Stack.Navigator>
```

#### Sự khác biệt giữa `title` và `headerTitle`

* `title` được sử dụng cho nhiều loại điều hướng như tab bars và drawers.
* `headerTitle` dành riêng cho stack navigators và thay thế component `Text` mặc định.

### Header Buttons

Bạn có thể đặt các nút trong header bằng cách sử dụng `headerLeft` (bên trái) hoặc `headerRight` (bên phải).

```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { Button, View, Text } from 'react-native'; // Import necessary components

// Assume HomeScreen component is defined elsewhere
function HomeScreen() { /* ... */ }

const Stack = createNativeStackNavigator();

// Inside your Navigator
// <Stack.Navigator>
  <Stack.Screen
    name="Home"
    component={HomeScreen}
    options={{
      headerRight: () => (
        <Button
          onPress={() => alert('This is a button!')}
          title="Info"
        />
      ),
    }}
  />
  // ... other screens
// </Stack.Navigator>
```

#### Tương tác của Header với Screen Component

Khi bạn định nghĩa nút theo cách này, biến `this` trong `options` không phải là instance của `HomeScreen`, vì vậy bạn không thể gọi `setState` hoặc bất kỳ phương thức instance nào trên đó.

Việc các nút trong header tương tác với màn hình mà header thuộc về là phổ biến. Để nút tương tác với trạng thái của màn hình, hãy sử dụng `navigation.setOptions`. Bằng cách sử dụng `navigation.setOptions` bên trong component màn hình, chúng ta có quyền truy cập vào các props, state, context, v.v. của màn hình.

```javascript
import { useNavigation } from '@react-navigation/native';
import React, { useState, useEffect } from 'react'; // Import React, useState, useEffect
import { Button, Text, View } from 'react-native'; // Import necessary components

function HomeScreen() {
  const navigation = useNavigation();
  const [count, setCount] = useState(0); // Correct usage of useState

  useEffect(() => { // Correct usage of useEffect
    navigation.setOptions({
      headerRight: () => (
        <Button
          onPress={() => setCount((c) => c + 1)}
          title="Update count"
        />
      ),
    });
  }, [navigation, setCount]); // Added setCount to dependency array

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Count: {count}</Text>
    </View>
  );
}
```

### Tùy chỉnh nút Back

React Navigation cung cấp các giá trị mặc định dành riêng cho từng nền tảng cho các nút back. Trên iOS, nút back hiển thị tiêu đề của màn hình trước đó khi có đủ không gian.

#### Các tùy chọn tùy chỉnh:

* `headerBackTitle`: Thay đổi văn bản nút back.
* `headerBackTitleStyle`: Tạo kiểu cho văn bản nút back.
* `headerBackImageSource`: Đặt hình ảnh tùy chỉnh cho nút back.

Ví dụ:

```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { View, Text } from 'react-native'; // Import necessary components

// Assume HomeScreen and DetailsScreen components are defined elsewhere
function HomeScreen() { /* ... */ }
function DetailsScreen() { /* ... */ }

const Stack = createNativeStackNavigator();

<Stack.Navigator>
  <Stack.Screen name="Home" component={HomeScreen} />
  <Stack.Screen
    name="Details"
    component={DetailsScreen}
    options={{
      headerBackTitle: 'Custom Back',
      headerBackTitleStyle: { fontSize: 30 },
    }}
  />
</Stack.Navigator>
```

#### Ghi đè nút Back

Nếu bạn cần một nút back hoàn toàn tùy chỉnh, hãy sử dụng `headerLeft`.

```javascript
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { Button, View, Text } from 'react-native'; // Import necessary components

// Assume DetailsScreen component is defined elsewhere
function DetailsScreen() { /* ... */ }

const Stack = createNativeStackNavigator();

// Inside your Navigator
// <Stack.Navigator>
  <Stack.Screen
    name="Details"
    component={DetailsScreen}
    options={{
      headerLeft: () => (
        <Button
          onPress={() => alert('Custom Back Pressed')}
          title="Back"
        />
      ),
    }}
  />
  // ... other screens
// </Stack.Navigator>
```

### Nesting Navigators

Nesting Navigators có nghĩa là đặt một navigator bên trong một màn hình của một navigator khác. Ví dụ: một Tab Navigator bên trong một Stack Navigator.

#### Các hành vi chính của Nested Navigators

* Lịch sử điều hướng độc lập: Mỗi navigator duy trì lịch sử điều hướng back riêng.
* Tùy chọn màn hình riêng biệt: Các tùy chọn của một navigator lồng nhau (ví dụ: tiêu đề) không ảnh hưởng đến navigator cha.
* Params độc lập: Params của một màn hình lồng nhau không thể truy cập được từ các màn hình cha/con.
* Navigation Actions Bubble Up: Nếu một navigator con không thể xử lý một hành động, navigator cha sẽ xử lý.
* Các phương thức dành riêng cho Navigator: Các phương thức như `openDrawer` chỉ có sẵn bên trong navigator đó.
* Không kế thừa Parent Events: Các màn hình bên trong một navigator lồng nhau không nhận được các sự kiện từ navigator cha.
* Parent UI Renders on Top: Một drawer được đặt bên trong một stack sẽ xuất hiện bên dưới header của stack.

#### Ví dụ: Nesting Navigators

```javascript
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs'; // Import Tab navigator creator
import { createNativeStackNavigator } from '@react-navigation/native-stack'; // Import Stack navigator creator
import { NavigationContainer } from '@react-navigation/native'; // Import NavigationContainer
import { View, Text } from 'react-native'; // Import necessary components

const HomeScreen = () => (
  <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
    <Text>Home Screen</Text>
  </View>
);
const SettingsScreen = () => (
  <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
    <Text>Settings Screen</Text>
  </View>
);
const DetailsScreen = () => (
  <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
    <Text>Details Screen</Text>
  </View>
);

const Tab = createBottomTabNavigator();

function MyTabs() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  );
}

const { Navigator, Screen } = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Navigator>
        <Screen name="Tabs" component={MyTabs} />
        <Screen name="Details" component={DetailsScreen} />
      </Navigator>
    </NavigationContainer>
  );
}
```

#### Điều hướng trong Nested Navigators

Để điều hướng đến một màn hình bên trong một nested navigator:

```javascript
import { useNavigation } from '@react-navigation/native';

function MyComponent() { // Example component
  const navigation = useNavigation();
  // ...
  navigation.navigate('Tabs', {
    screen: 'Settings'
    // Truyền params khi điều hướng:
    // params: { user: 'jane' }
  });
  // ...
}
```
Để truyền params khi điều hướng đến màn hình trong tab:
```javascript
import { useNavigation } from '@react-navigation/native';

function MyComponent() { // Example component
  const navigation = useNavigation();
  // ...
  navigation.navigate('Tabs', {
    screen: 'Settings',
    params: { user: 'jane' }
  });
  // ...
}
```

## Lecture 7

### Style in React Native

Cái nào được đặt sau thì sẽ sử dụng cái đấy, ví dụ như trong hình thì `<Text style={[styles.bigBlue, styles.red]}>bigBlue, then red</Text>` sẽ hiện màu đỏ, chứ không phải xanh.

```jsx
import { StyleSheet, Text, View } from 'react-native'; // Import necessary components

// Assume styles object is defined below
const styles = StyleSheet.create({
  container: { marginTop: 50 },
  bigBlue: { color: 'blue', fontWeight: 'bold', fontSize: 30 },
  red: { color: 'red' }
});

<View style={styles.container}>
  <Text style={styles.red}>just red</Text>
  <Text style={styles.bigBlue}>just bigBlue</Text>
  <Text style={[styles.bigBlue, styles.red]}>bigBlue, then red</Text>
  <Text style={[styles.red, styles.bigBlue]}>red, then bigBlue</Text>
</View>
```

Dựa trên nội dung bạn cung cấp từ Lecture 7, dưới đây là các đoạn code và giải thích về Styling và Layout với Flexbox trong React Native:

### Styling trong React Native

* **Styling với JavaScript**: Các component trong React Native chấp nhận một `style` prop. Tên thuộc tính style được viết theo kiểu `camelCase` (ví dụ: `backgroundColor` thay vì `background-color`).
* **Định nghĩa styles**:
    * Styles có thể là một đối tượng JavaScript đơn giản.
    * Có thể sử dụng một mảng các styles (style cuối cùng trong mảng sẽ được ưu tiên).
    * Đối với các component phức tạp, sử dụng `StyleSheet.create` giúp tổ chức styles gọn gàng hơn.

#### Sử dụng `StyleSheet.create`

`StyleSheet.create` giúp định nghĩa nhiều style ở một nơi.

* **Lợi ích**: Hiệu suất (styles bất biến), Validation, Tính nhất quán, Tích hợp tốt hơn với React Native, Minification (giảm kích thước bundle).

```javascript
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

export default function LotsOfStyles() {
  return (
    <View style={styles.container}>
      <Text style={styles.red}>just red</Text>
      <Text style={styles.bigBlue}>just bigBlue</Text>
      <Text style={[styles.bigBlue, styles.red]}>
        bigBlue, then red</Text>
      <Text style={[styles.red, styles.bigBlue]}>
        red, then bigBlue</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { marginTop: 50 },
  bigBlue: { color: 'blue', fontWeight: 'bold', fontSize: 30 },
  red: { color: 'red' }
});
```

* **Kế thừa styles**: Một component có thể nhận `style` prop và truyền nó cho các subcomponent, cho phép styles "cascade" giống như trong CSS.
* **Vấn đề tương thích**: Có một số khác biệt so với web CSS, ví dụ: vùng chạm không mở rộng ra ngoài view cha và lề âm không được hỗ trợ trên Android.

### Height and Width

#### Fixed Dimensions (Kích thước cố định)

Cách thông thường để đặt kích thước của một component là thêm `width` và `height` cố định vào `style`. Tất cả các kích thước trong React Native là *không có đơn vị*, và đại diện cho *density-independent pixels (dp)*.

```javascript
import { View } from 'react-native'; // Import necessary component

<View style={{ flex: 1 }}>
  <View style={{
    width: 50 ,
    height: 50 ,
    backgroundColor: 'powderblue'
  }} />
  <View style={{
    width: 100 ,
    height: 100 ,
    backgroundColor: 'skyblue'
  }} />
  <View style={{
    width: 150 ,
    height: 150 ,
    backgroundColor: 'steelblue'
  }} />
</View>
```

#### Flex Dimensions (Kích thước linh hoạt)

Sử dụng `flex` trong style của component để làm cho nó mở rộng hoặc co lại dựa trên không gian có sẵn.

* `flex: 1` làm cho component lấp đầy tất cả không gian có sẵn, chia sẻ đều với các component ngang cấp.
* Giá trị `flex` lớn hơn sẽ cho component một phần không gian lớn hơn.
* Một component chỉ có thể mở rộng nếu component cha của nó có kích thước được định nghĩa (kích thước cố định hoặc flex).

```javascript
import React from 'react';
import { View } from 'react-native'; // Import necessary component

export default function FlexDimensionsBasics() {
  return (
    // Try removing the `flex: 1` on the parent View.
    // The parent will not have dimensions, so the
    // children can't expand.
    // What if you add `height: 300` instead of `flex: 1`?
    <View style={{ flex: 1 }}>
      <View style={{
        flex: 1 , backgroundColor: 'powderblue'
      }} />
      <View style={{
        flex: 2 , backgroundColor: 'skyblue'
      }} />
      <View style={{
        flex: 3 , backgroundColor: 'steelblue'
      }} />
    </View>
  );
};
```

#### Percentage Dimensions (Kích thước theo phần trăm)

Thay vì `flex`, bạn có thể sử dụng giá trị phần trăm để kiểm soát kích thước của component, nhưng component cha phải có kích thước được định nghĩa.

```javascript
import { View } from 'react-native'; // Import necessary component

<View style={{ height: '100%' }}>
  <View style={{
    height: '15%',
    backgroundColor: 'powderblue'
  }} />
  <View style={{
    width: '66%',
    height: '35%',
    backgroundColor: 'skyblue'
  }} />
  <View style={{
    width: '33%',
    height: '50%',
    backgroundColor: 'steelblue'
  }} />
</View>
```

### Layout với Flexbox

Một component có thể chỉ định layout của các component con bằng thuật toán Flexbox. Flexbox được thiết kế để cung cấp layout nhất quán trên các kích thước màn hình khác nhau. Bạn thường sử dụng kết hợp `flexDirection`, `alignItems`, và `justifyContent` để đạt được layout mong muốn.

* **LƯU Ý**: Flexbox hoạt động tương tự trong React Native như trong CSS trên web, với một vài ngoại lệ: các giá trị mặc định khác nhau (`flexDirection` mặc định là `column` thay vì `row`, `alignContent` mặc định là `flex-start` thay vì `stretch`, `flexShrink` mặc định là 0 thay vì 1, tham số `flex` chỉ hỗ trợ một số duy nhất).

#### Flex

`flex` sẽ xác định cách các item của bạn "lấp đầy" không gian có sẵn dọc theo trục chính. Không gian sẽ được chia theo thuộc tính `flex` của mỗi phần tử.

```javascript
import React from 'react';
import { StyleSheet, View } from 'react-native'; // Import necessary components

export default function Flex() {
  return (
    <View style={[
      styles.container,
      { // Try setting 'flexDirection' to 'row'
        flexDirection: 'column'
      }]}>
      <View style={{flex: 1 , backgroundColor: 'red'}} />
      <View style={{flex: 2 , backgroundColor: 'darkorange'}} />
      <View style={{flex: 3 , backgroundColor: 'green'}} />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1 ,
    padding: 20
  }
});
```

#### Flex Direction

`flexDirection` kiểm soát hướng mà các component con được bố trí. Đây còn được gọi là trục chính. Trục chéo là trục vuông góc với trục chính.

* `column` (giá trị mặc định): Sắp xếp các component con từ trên xuống dưới.
* `row`: Sắp xếp các component con từ trái sang phải.
* `column-reverse`: Sắp xếp các component con từ dưới lên trên.
* `row-reverse`: Sắp xếp các component con từ phải sang trái.

Ví dụ về `flexDirection`:

Thiết lập ban đầu:

```javascript
import React, { useState } from 'react';
import { StyleSheet, Text, View } from 'react-native';

const FlexDirectionExample = () => {
  const [flexDirection, setFlexDirection] = useState('column');
  return (
    <View style={styles.container}>
      <Text style={styles.label}>flexDirection: {flexDirection}</Text>
      {/* Remaining parts will be introduced in later slides */}
    </View>
  );
};

// Assume styles object is defined elsewhere
// const styles = StyleSheet.create({ ... });
```

Thay đổi `flexDirection` với Buttons:

```javascript
import { TouchableOpacity, Text, View, StyleSheet } from 'react-native'; // Import necessary components
import React, { useState } from 'react'; // Import useState

// Assume styles object is defined elsewhere
const styles = StyleSheet.create({
  container: { flex: 1, padding: 10 },
  label: { textAlign: "center", fontSize: 18, marginBottom: 10 },
  buttons: { flexDirection: "row", justifyContent: "center" },
  button: {
    padding: 8,
    margin: 5,
    backgroundColor: "oldlace",
    borderRadius: 5,
  },
  selected: { backgroundColor: "coral" },
  boxContainer: { flex: 1, justifyContent: "center", alignItems: "center" },
  box: { width: 50, height: 50, margin: 5 }
});

const FlexDirectionExample = () => {
  const [flexDirection, setFlexDirection] = useState('column');
  return (
    <View style={styles.container}>
      <Text style={styles.label}>flexDirection: {flexDirection}</Text>
      <View style={styles.buttons}>
        {['column', 'row', 'row-reverse', 'column-reverse'].map(value => (
          <TouchableOpacity
            key={value}
            onPress={() => setFlexDirection(value)}
            style={[styles.button, flexDirection === value && styles.selected]}
          >
            <Text style={styles.buttonText}>{value}</Text>
          </TouchableOpacity>
        ))}
      </View>
      {/* The boxContainer part will be rendered below */}
    </View>
  );
};
```

Thay đổi layout với `flexDirection`:

```javascript
import { View, StyleSheet } from 'react-native'; // Import necessary components
import React, { useState } from 'react'; // Import useState

// Assume styles object is defined elsewhere
const styles = StyleSheet.create({
  container: { flex: 1, padding: 10 },
  label: { textAlign: "center", fontSize: 18, marginBottom: 10 },
  buttons: { flexDirection: "row", justifyContent: "center" },
  button: {
    padding: 8,
    margin: 5,
    backgroundColor: "oldlace",
    borderRadius: 5,
  },
  selected: { backgroundColor: "coral" },
  boxContainer: { flex: 1, justifyContent: "center", alignItems: "center" },
  box: { width: 50, height: 50, margin: 5 }
});

const FlexDirectionExample = () => {
  const [flexDirection, setFlexDirection] = useState('column');
  return (
    <View style={styles.container}>
      <Text style={styles.label}>flexDirection: {flexDirection}</Text>
      {/* Buttons part rendered above */}
      <View style={[styles.boxContainer, { flexDirection }]}>
        <View style={[styles.box, { backgroundColor: 'powderblue' }]} />
        <View style={[styles.box, { backgroundColor: 'skyblue' }]} />
        <View style={[styles.box, { backgroundColor: 'steelblue' }]} />
      </View>
    </View>
  );
};

export default FlexDirectionExample; // Export the component
```

Styles cho ví dụ `flexDirection`:

```javascript
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: { flex: 1 , padding: 10 },
  label: { textAlign: "center", fontSize: 18 , marginBottom: 10 },
  buttons: { flexDirection: "row", justifyContent: "center" },
  button: {
    padding: 8 ,
    margin: 5 ,
    backgroundColor: "oldlace",
    borderRadius: 5 ,
  },
  selected: { backgroundColor: "coral" },
  boxContainer: { flex: 1 , justifyContent: "center", alignItems: "center" },
  box: { width: 50 , height: 50 , margin: 5 }
});
```

#### Layout Direction

`layoutDirection` chỉ định hướng của các component con và văn bản. Nó ảnh hưởng đến ý nghĩa của `start` và `end`.

* `LTR` (giá trị mặc định): Văn bản và component con được bố trí từ trái sang phải.
* `RTL`: Văn bản và component con được bố trí từ phải sang trái.

#### Justify Content

`justifyContent` mô tả cách căn chỉnh các component con dọc theo trục chính của container.

* `flex-start` (giá trị mặc định): Căn chỉnh về đầu trục chính.
* `flex-end`: Căn chỉnh về cuối trục chính.
* `center`: Căn chỉnh vào giữa trục chính.
* `space-between`: Phân bổ đều không gian giữa các component con dọc theo trục chính.
* `space-around`: Phân bổ đều không gian xung quanh các component con dọc theo trục chính.
* `space-evenly`: Phân bổ đều không gian giữa các component con, không gian ở hai đầu container và khoảng cách giữa các item đều bằng nhau.

#### Align Items

`alignItems` mô tả cách căn chỉnh các component con dọc theo trục chéo của container. Nó tương tự như `justifyContent` nhưng dùng cho trục chéo.

* `stretch` (giá trị mặc định): Kéo dài các component con để khớp với chiều cao của trục chéo.
* `flex-start`: Căn chỉnh về đầu trục chéo.
* `flex-end`: Căn chỉnh về cuối trục chéo.
* `center`: Căn chỉnh vào giữa trục chéo.
* `baseline`: Căn chỉnh các component con dọc theo một đường cơ sở chung.

#### Align Self

`alignSelf` có các tùy chọn và hiệu ứng giống như `alignItems` nhưng áp dụng cho một component con duy nhất để thay đổi căn chỉnh của nó bên trong component cha. `alignSelf` ghi đè bất kỳ tùy chọn nào được đặt bởi component cha với `alignItems`.

#### Align Content

`alignContent` định nghĩa cách phân bổ các dòng dọc theo trục chéo. Điều này chỉ có hiệu lực khi các item được xuống dòng bằng `flexWrap`.

* `flex-start` (giá trị mặc định): Căn chỉnh các dòng đã xuống dòng về đầu trục chéo.
* `flex-end`: Căn chỉnh các dòng đã xuống dòng về cuối trục chéo.
* `stretch`: Kéo dài các dòng đã xuống dòng để khớp với chiều cao của trục chéo.
* `center`: Căn chỉnh các dòng đã xuống dòng vào giữa trục chéo.
* `space-between`: Phân bổ đều không gian giữa các dòng đã xuống dòng dọc theo trục chéo.
* `space-around`: Phân bổ đều không gian xung quanh các dòng đã xuống dòng dọc theo trục chéo.
* `space-evenly`: Phân bổ đều không gian giữa các dòng đã xuống dòng dọc theo trục chéo.

#### Flex Wrap

Thuộc tính `flexWrap` được đặt trên container và kiểm soát điều gì xảy ra khi các component con tràn ra ngoài kích thước của container dọc theo trục chính. Mặc định, các component con bị ép vào một dòng duy nhất. Nếu cho phép xuống dòng, các item sẽ được xuống dòng thành nhiều dòng dọc theo trục chính nếu cần.

#### Flex Basis và Grow

* **flexBasis**: Định nghĩa kích thước mặc định của một item dọc theo trục chính. Hoạt động giống như `width` nếu `flexDirection: row`, hoặc `height` nếu `flexDirection: column`. Kích thước của item trước khi điều chỉnh bởi `flexGrow` và `flexShrink`.
* **flexGrow**: Xác định lượng không gian còn lại mà một component con nên chiếm trong container. Chấp nhận giá trị ≥ 0 (mặc định là 0). Không gian được phân bổ theo tỷ lệ dựa trên giá trị `flexGrow`.

#### flexShrink

Định nghĩa cách một component con co lại khi nội dung tràn ra ngoài container. Chấp nhận giá trị ≥ 0 (mặc định là 0, nhưng là 1 trong web). Hoạt động với `flexGrow` để cho phép các phần tử mở rộng và co lại một cách linh hoạt.

#### Row Gap, Column Gap và Gap

* `rowGap`: Đặt kích thước của khoảng cách (gutter) giữa các hàng của một phần tử.
* `columnGap`: Đặt kích thước của khoảng cách (gutter) giữa các cột của một phần tử.
* `gap`: Đặt kích thước của khoảng cách (gutter) giữa các hàng và cột. Nó là shorthand cho `rowGap` và `columnGap`.

Bạn có thể sử dụng `flexWrap` và `alignContent` cùng với `gap` để thêm khoảng cách nhất quán giữa các item.

## Bài 8: Tailwind CSS for React Native i.e. NativeWind

```bash
npm install nativewind@2.0.11 tailwindcss@3.2.2
# Optional
npm install react-native-reanimated
# (optional)
npm install react-native-safe-area-context
```

2.  **Thiết lập NativeWind:** Chạy `npx tailwindcss init` để tạo file `tailwind.config.js`. Thêm đường dẫn đến tất cả các file component của bạn vào file `tailwind.config.js`.

```javascript
    /** @type {import('tailwindcss').Config} */
    module.exports = {
      content: [
        "./TailwindApp.js",
        "./components/*.js",
        "./screens/*.js"
      ],
      theme: {
        extend: {},
      },
      plugins: [],
    }
```

3.  **Thêm Babel preset:** Bước này đảm bảo NativeWind hoạt động chính xác trong React Native bằng cách biến đổi cú pháp JSX để hỗ trợ `className` theo kiểu Tailwind CSS. Tạo file `babel.config.js`.

```javascript
    module.exports = {
      presets: ["babel-preset-expo"],
      plugins: ["nativewind/babel"],
    };
```

### Styling với Utility Classes

Bạn tạo kiểu bằng Tailwind bằng cách kết hợp nhiều class trình bày (utility classes) có mục đích đơn lẻ trực tiếp trong markup của bạn.

```javascript
<View className="flex-1 justify-center items-center bg-blue-100">
  <View className="flex-row items-center shadow-2xl shadow-gray-900 gap-x-4 rounded-xl bg-white p-6">
    <View>
      <Image className="w-20 h-20" resizeMode="contain" source={require('./assets/chitchat.png')} />
    </View>
    <View>
      <Text className="text-xl font-medium text-black">ChitChat</Text>
      <Text className="text-gray-500">You have a new message!</Text>
    </View>
  </View>
</View>
```

Trong ví dụ trên, các utility classes như `flex`, `flex-row`, `p-6`, `justify-center`, `items-center`, `bg-white`, `rounded-xl`, `shadow-2xl`, `shadow-gray-900`, `w-20`, `h-20`, `gap-x-4`, `text-xl`, `text-black`, `font-medium`, `text-gray-500` được sử dụng để kiểm soát layout, căn chỉnh, kiểu dáng hộp, kích thước hình ảnh, khoảng cách và kiểu văn bản.

#### Lợi ích chính của Utility-First Styling:

* Phát triển nhanh hơn.
* Thay đổi an toàn hơn.
* Bảo trì dễ dàng hơn.
* Code portable hơn.
* Sử dụng bộ nhớ nhỏ.

Sử dụng utility classes có nhiều lợi thế quan trọng so với inline styles, bao gồm thiết kế với các ràng buộc, hỗ trợ các trạng thái như `focus`, `press` và media queries.

### Các tính năng cốt lõi của Tailwind/NativeWind

* Variants cho các trạng thái (ví dụ: `focus:bg-orange-200`).
* Thiết kế responsive sử dụng tiền tố như `sm:grid-cols-3`).
* Styling chế độ tối với `dark:bg-gray-800`).
* Arbitrary values (ví dụ: `bg-[#316ff6]`) cho các styles tùy chỉnh.

#### State Variants

Ví dụ về TextInput với các kiểu khác nhau khi focus và không focus:

```javascript
<TextInput
  className="border p-2 rounded focus:border-blue-500 focus:outline-none"
  placeholder="Enter text..."
/>
```

#### Media Queries và Breakpoints

Bạn có thể tạo kiểu cho các phần tử ở các breakpoint khác nhau bằng cách thêm tiền tố breakpoint vào utility class.

```javascript
<View className="flex flex-row sm:flex-col">
  <Text>A</Text>
  <Text>B</Text>
  <Text>C</Text>
</View>
```

#### Sử dụng Arbitrary Values

Khi bạn cần sử dụng một giá trị một lần nằm ngoài theme của mình, hãy sử dụng cú pháp ngoặc vuông đặc biệt để chỉ định arbitrary values.

```javascript
<Pressable className="bg-[#316ff6] ...">
  <Text className="text-[#ffe]">Sign in with Facebook</Text>
</Pressable>
```

## Lecture 10

Dưới đây là các đoạn code và giải thích từ tài liệu Lecture 10 về Quản lý State với Redux trong React Native:

### Tại sao sử dụng Redux?

* Quản lý state theo cách truyền thống trong React/React Native với `useState` có thể dẫn đến vấn đề "prop drilling" (truyền prop qua nhiều cấp), không có "single source of truth" (nguồn dữ liệu duy nhất) và liên kết chặt chẽ giữa presentation và data model.
* Redux cung cấp một giải pháp quản lý state tập trung, giúp giải quyết các vấn đề trên.
* Lợi ích của việc chọn Redux bao gồm: hệ sinh thái middleware phong phú, công cụ phát triển tuyệt vời, tài liệu tốt, là lựa chọn hàng đầu cho quản lý state trong React, hỗ trợ tốt bởi các thư viện/framework khác, và hỗ trợ React server side rendering.

### Các thành phần cốt lõi của Redux Data Flow

Để sử dụng Redux hiệu quả, bạn cần hiểu bốn thành phần chính của nó:

1.  **Store:** Nơi lưu trữ tập trung giữ toàn bộ state của ứng dụng. State trong store là read-only.
2.  **Views (UI Components):** Các component React kết nối với store để hiển thị dữ liệu và kích hoạt actions.
3.  **Actions:** Các đối tượng JavaScript thuần mô tả những gì đã xảy ra (ví dụ: tương tác của người dùng hoặc phản hồi từ API). Actions có thuộc tính `type` và thường có thuộc tính `payload`.

```javascript
    store.dispatch({ type: 'BEGIN_LOADING' });
    store.dispatch({ type: 'DONE_LOADING' });
    store.dispatch({ type: 'UPDATE_SEARCH_TERM', payload: 'mpr' });
```

4.  **Reducers:** Các pure function (hàm thuần khiết) nhận state hiện tại và một action, sau đó trả về một state mới. Reducer không sửa đổi state gốc trực tiếp mà trả về một đối tượng state mới. Mỗi reducer ánh xạ đến chính xác 1 phần của cây state.

```javascript
    function todoReducer(state = [], action) {
      switch (action.type) {
        case 'ADD_TODO':
          // creates new array, adds todo at the end
          return [...state, action.payload];
        case 'REMOVE_TODO':
          // creates new array filtering out matching todo by id
          return state.filter(todo => todo.id !== action.payload);
        default:
          return state;
      }
    }
```

#### Action Creators

Là các factory function trả về một action. Mặc dù không bắt buộc, nhưng nó thường được sử dụng.

```javascript
// searchTermActions.js
function updateSearchTerm(searchTerm) {
  return {
    type: 'UPDATE_SEARCH_TERM',
    payload: searchTerm,
  };
}

// SearchComponent.js
store.dispatch(updateSearchTerm('homework'));
```

#### Root Reducer

Một ứng dụng Redux thực sự chỉ có một hàm reducer: "root reducer" mà bạn sẽ truyền vào `createStore`. Root reducer này xử lý tất cả các actions được dispatch và tính toán toàn bộ state mới.

```javascript
export default function appReducer(state = initialState, action) {
  switch (action.type) {
    case 'todos/todoAdded': {
      return {
        ...state,
        todos: [...state.todos, newTodo]
      };
    }
    case 'todos/todoToggled': {
      return {
        ...state,
        todos: [...state.todos, toggledTodo]
      };
    }
    default:
      return state;
  }
}
```

#### Splitting Reducers

Reducers thường được chia nhỏ dựa trên phần state mà chúng cập nhật. Reducer cho một phần cụ thể của state được gọi là "slice reducer". Các actions liên quan đến một slice reducer nên có cùng tiền tố (ví dụ: `todos/todoAdd`).

```javascript
// todosSlice.js
const initialState = [
  { id: 0, text: 'Learn React', completed: true },
  { id: 1, text: 'Learn Redux', completed: false },
];

export default function todosReducer(state = initialState, action) {
  switch (action.type) {
    case 'todos/todoAdded': {
      // code to handle this action
      return state; // placeholder
    }
    case 'todos/todoToggled': {
      // code to handle this action
      return state; // placeholder
    }
    default:
      return state;
  }
}
```

#### Combining Reducers

Store chỉ cần một root reducer, vì vậy bạn cần kết hợp tất cả các slice reducer lại với nhau. Có thể làm thủ công hoặc sử dụng utility function `combineReducers`.

Sử dụng `combineReducers`:

```javascript
import { combineReducers } from 'redux';
import todosReducer from './features/todos/todosSlice';
import filtersReducer from './features/filters/filtersSlice';

const rootReducer = combineReducers({
  todos: todosReducer,
  filters: filtersReducer,
});

export default rootReducer;
```

Kết hợp thủ công:

```javascript
import todosReducer from './features/todos/todosSlice';
import filtersReducer from './features/filters/filtersSlice';

export default function rootReducer(state = {}, action) {
  return {
    // the value of `state.todos` is whatever
    // the todos reducer returns
    todos: todosReducer(state.todos, action),
    // For both reducers, we only pass in
    // their slice of the state
    filters: filtersReducer(state.filters, action),
  };
}
```

### Redux Store

* Store là nơi chứa toàn bộ cây state của ứng dụng. State tree là read-only. Cách duy nhất để thay đổi state bên trong store là dispatch một action lên nó.
* Store không phải là một class, nó chỉ là một đối tượng với một vài phương thức.

#### Tạo một Redux store

```javascript
import { createStore, combineReducers, applyMiddleware } from 'redux';

// Assume loadingReducer, todosReducer, searchTermReducer, bookMarksReducer, userProfileReducer are defined
// Assume INITIAL_STATE is defined
// Assume logger, thunk are defined (middleware)

const store = createStore(
  combineReducers({
    isLoading: loadingReducer,
    todos: todosReducer,
    searchTerm: searchTermReducer,
    bookmarks: bookMarksReducer,
    userProfile: userProfileReducer,
  }), // required – the root reducer
  INITIAL_STATE, // optional
  applyMiddleware(logger, thunk), // optional
);
```

#### Lắng nghe cập nhật của store

Phương thức `subscribe` thêm một hàm lắng nghe sự thay đổi. Nó sẽ được gọi bất cứ khi nào một action được dispatch và một phần nào đó của cây state có thể đã thay đổi.

```javascript
let currentValue;

function handleChange() {
  let previousValue = currentValue;
  currentValue = store.getState().some.property; // Replace 'some.property' with the actual state slice you want to track

  if (previousValue !== currentValue) {
    console.log(
      'Some property changed from',
      previousValue,
      'to',
      currentValue
    );
  }
}

const unsubscribe = store.subscribe(handleChange);

// Call unsubscribe() later to remove the listener
```

### Redux và Middleware

* Middleware là một lớp nằm giữa action creators và reducers. Nó có thể chặn hoặc dispatch các actions bổ sung, có toàn quyền truy cập vào action và state của ứng dụng, và có thể xử lý các async actions.
* Middleware được áp dụng cho store bằng cách sử dụng hàm `applyMiddleware`.
* Ví dụ về Middleware: Redux Logger (ghi log state trước action, action được dispatch, state sau action) và Redux DevTools (hiển thị timeline của actions, cho phép time travel debugging).

### Làm việc với Redux store bằng `react-redux`

Thư viện `react-redux` cung cấp các hook để tương tác với Redux store trong các component React.

#### Hook `useSelector()`

Cho phép component React đọc dữ liệu từ Redux store. Nó nhận một hàm selector làm đối số, hàm này nhận toàn bộ state của store, đọc một giá trị từ state và trả về kết quả đó. `useSelector` tự động subscribe vào store.

```javascript
import { useSelector } from 'react-redux';
import { FlatList, View, Text } from 'react-native'; // Import necessary components

// Assume Todo component is defined elsewhere
function Todo({ todo }) {
  return (
    <View>
      <Text>{todo}</Text>
    </View>
  );
}

const selectTodos = state => state.todos;

export const TodoList = ({ DATA }) => { // Assuming DATA is passed as a prop or comes from context/state
  const todos = useSelector(selectTodos);

  return (
    <FlatList
      data={todos} // Or use DATA prop if needed
      renderItem={({ item }) => <Todo key={item.id} todo={item.text} />} // Assuming item has id and text properties
      style={{}} // Apply your styles here, e.g., styles.todoList
    />
  );
};
```

#### Hook `useDispatch()`

Hook `useDispatch` trả về phương thức `dispatch` của store. Bạn có thể khai báo `const dispatch = useDispatch()` trong bất kỳ component nào và sử dụng `dispatch(someAction)` khi cần.

```javascript
import { useDispatch } from 'react-redux';
import { useState } from 'react'; // Import useState
import { TextInput, Button, View } from 'react-native'; // Import necessary components

export default Header = () => {
  const [text, setText] = useState('');
  const dispatch = useDispatch();

  const handleSubmit = () => { // Changed to a regular function
    // Dispatch the "todo added" action with this text
    dispatch({ type: 'todos/todoAdded', payload: text });
    // Clear out the text input
    setText('');
  };

  return (
    <View>
      <TextInput
        placeholder="What needs to be done?"
        value={text}
        onChangeText={val => setText(val)}
        onSubmitEditing={handleSubmit}
      />
    </View>
  );
};
```

#### Redux store Provider

Để `useSelector` và `useDispatch` tìm thấy store, bạn cần bọc toàn bộ ứng dụng bằng component `<Provider>` từ `react-redux` và truyền store vào prop `store`.

```javascript
// index.js (or App.js depending on your project structure)
import { Provider } from 'react-redux';
import { registerRootComponent } from 'expo'; // If using Expo
import App from './App';
import store from './store'; // Assume your store is exported from './store.js'

// If using Expo
registerRootComponent(
  <Provider store={store}>
    <App />
  </Provider>
);

// If not using Expo, typically wrap your root component in your entry file
/*
import React from 'react';
import { AppRegistry } from 'react-native';
import App from './App';
import store from './store'; // Assume your store is exported from './store.js'

const AppWithStore = () => (
  <Provider store={store}>
    <App />
  </Provider>
);

AppRegistry.registerComponent('YourAppName', () => AppWithStore);
*/
```

### Async Actions với `redux-thunk` middleware

Redux chỉ hoạt động đồng bộ theo mặc định. Để xử lý các async actions, bạn cần sử dụng middleware như `redux-thunk`.

#### Thunk là gì?

Thunk là một hàm bọc một biểu thức để trì hoãn việc tính toán của nó.

```javascript
// calculation of 1 + 2 is immediate
// x === 3
let x = 1 + 2;

// calculation of 1 + 2 is delayed
// foo can be called later to perform the calculation
// foo is a thunk!
let foo = () => 1 + 2;
```

#### Sử dụng `redux-thunk`

Đầu tiên, tạo một phiên bản store đã được sửa đổi để chấp nhận các thunk function bằng cách áp dụng middleware `redux-thunk`.

```javascript
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducer'; // Assume your root reducer is exported from './reducer.js'

// The store now has the ability to accept thunk functions in `dispatch`
const store = createStore(rootReducer, applyMiddleware(thunk));

export default store;
```

#### Ví dụ: Dispatching một Function (sử dụng `redux-thunk`)

```javascript
// thunk function
// Assume fetchData is defined elsewhere (e.g., your API utility)
export async function fetchTodos(dispatch) {
  try {
    const response = await fetchData('/fakeApi/todos');
    dispatch({ type: 'todos/todosLoaded', payload: response.todos });
  } catch (error) {
    // Handle errors, maybe dispatch an error action
    console.error("Error fetching todos:", error);
  }
}

// using the thunk function later on
// Assume store is imported
// store.dispatch(fetchTodos); // Call the thunk function
```

## Lecture 11: Multimedia

### Expo ImagePicker

`expo-image-picker` cung cấp quyền truy cập vào UI hệ thống để chọn hình ảnh và video từ thư viện của điện thoại hoặc chụp ảnh bằng camera.

* **Cài đặt:**

```bash
npx expo install expo-image-picker
```

* **Cấu hình trong `app.json` (với plugin config):**
    Cấu hình này được sử dụng khi sử dụng Expo Image Picker thông qua hệ thống plugin của Expo. `photosPermission` chỉ định thông báo sẽ hiển thị cho người dùng khi ứng dụng yêu cầu quyền truy cập vào ảnh của họ (chỉ cho iOS).

```json
"expo": {
  "plugins": [
    [
      "expo-image-picker",
      {
        "photosPermission": "The app accesses your photos to let you share them with your friends."
      }
    ]
  ]
}
```

* **Các thuộc tính cấu hình khác (chỉ cho iOS):**

    * `photosPermission`: Đặt thông báo quyền truy cập thư viện ảnh.
    * `cameraPermission`: Đặt thông báo quyền truy cập camera.
    * `microphonePermission`: Đặt thông báo quyền truy cập microphone.

* **Sử dụng Image Picker:**
    Nhập thư viện và sử dụng hook `useState` để quản lý trạng thái ảnh đã chọn.

```javascript
import * as ImagePicker from 'expo-image-picker';
import { useState } from 'react'; // Assuming useState is imported
import { Button, Image, View, StyleSheet } from 'react-native'; // Import necessary components

// Assume styles object is defined elsewhere
const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: 'center', alignItems: 'center' },
  image: { width: 200, height: 200, marginTop: 20, resizeMode: 'contain' },
});

function MyImagePickerComponent() { // Example component
  const [image, setImage] = useState(null);

  const pickImage = async () => {
    // No permissions request is necessary for launching the image library
    let result = await ImagePicker.launchImageLibraryAsync({
      mediaTypes: ['images', 'videos'],
      allowsEditing: true,
      aspect: [4, 3],
      quality: 1,
    });

    if (!result.canceled) {
      setImage(result.assets[0].uri);
      console.log(result.assets[0].uri); // Log the URI
    }
    console.log(result); // Log the entire result object
  };

  // Hiển thị ảnh đã chọn:
  return (
    <View style={styles.container}>
      <Button title="Pick an image from camera roll" onPress={pickImage} />
      {image && <Image source={{ uri: image }} style={styles.image} />}
    </View>
  );
}

export default MyImagePickerComponent; // Export the component
```

* `ImagePicker.launchImageLibraryAsync` mở thư viện ảnh.
* `mediaTypes` xác định loại media có thể chọn.
* `allowsEditing` cho phép chỉnh sửa ảnh đã chọn.
* `aspect` đặt tỷ lệ khung hình khi chỉnh sửa.
* `quality` đặt chất lượng nén.

**Kiểm tra quyền cho iOS (cách cũ, tài liệu mới có thể khác):**

```javascript
import { askAsync, MEDIA_LIBRARY } from 'expo-permissions'; // Note: expo-permissions might be deprecated in newer Expo versions
import * as ImagePicker from 'expo-image-picker'; // Import ImagePicker

const pickImage = async () => {
  // Request permission to access the photo library
  const { status } = await askAsync(MEDIA_LIBRARY);
  if (status !== 'granted') {
    alert('Sorry, we need camera roll permissions to make this work!');
    return;
  }

  // Launch the image library
  let result = await ImagePicker.launchImageLibraryAsync({
    mediaTypes: ['images', 'videos'],
    allowsEditing: true,
    aspect: [4, 3],
    quality: 1,
  });

  if (!result.canceled) {
    console.log(result.assets[0].uri);
  }
};
```

### Expo ImageManipulator

`expo-image-manipulator` cho phép bạn thao tác trực tiếp với hình ảnh trong ứng dụng React Native, bao gồm xoay, lật, cắt và thay đổi kích thước.

* **Cài đặt:**

```bash
npx expo install expo-image-manipulator
```

* **Các tính năng chính:**

    * **Image Transformation:** Xoay, lật (ngang/dọc), cắt, thay đổi kích thước.
    * **Image Format:** Lưu dưới dạng JPEG hoặc PNG, điều chỉnh chất lượng nén.
    * **Integration:** Hoạt động tốt với ảnh từ camera hoặc thư viện, và có thể dùng với `expo-image-picker`.

* **Sử dụng ImageManipulator:**
    Ví dụ về xoay và lật ảnh:

```javascript
import { useState } from 'react';
import { Button, Image, StyleSheet, View } from 'react-native';
import { Asset } from 'expo-asset';
import { FlipType, SaveFormat, useImageManipulator } from 'expo-image-manipulator';

// Loading the Image
const IMAGE = Asset.fromModule(require('./assets/avatar.jpg'));

export default function App() { // Example App component
  const [image, setImage] = useState(IMAGE);

  // useImageManipulator creates a manipulation context for the image.
  const context = useImageManipulator(IMAGE.uri);

  // This function is triggered when the "Rotate and Flip" button is pressed.
  const rotate90andFlip = async () => {
    context.rotate(90).flip(FlipType.Vertical);
    const manipulatedImage = await context.renderAsync(); // Rename to avoid conflict
    const result = await manipulatedImage.saveAsync({
      format: SaveFormat.PNG,
    });
    setImage(result);
  };

  return (
    <View style={styles.container}>
      <View style={styles.imageContainer}>
        <Image source={{ uri: image.localUri || image.uri }} style={styles.image} />
      </View>
      <Button title="Rotate and Flip" onPress={rotate90andFlip} />
    </View>
  );
}

// Assume styles object is defined elsewhere
const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: 'center', alignItems: 'center' },
  imageContainer: { marginBottom: 20 },
  image: { width: 200, height: 200, resizeMode: 'contain' },
});
```

* `Asset.fromModule` tải ảnh từ assets.
* `useImageManipulator` tạo context để thao tác.
* `context.rotate()` và `context.flip()` thực hiện các phép biến đổi.
* `context.renderAsync()` áp dụng các phép biến đổi.
* `manipulatedImage.saveAsync()` lưu ảnh đã thao tác.

### Expo Video

Component `Expo Video` là một phần của thư viện `expo-av`, được sử dụng để phát âm thanh và video trong ứng dụng React Native.

* **Cài đặt:**

```bash
npx expo install expo-video
```

* **Cấu hình trong `app.json` (với config plugin):**

```json
{
  "expo": {
    "plugins": [
      [
        "expo-video",
        {
          "supportsBackgroundPlayback": true, // Chỉ cho iOS
          "supportsPictureInPicture": true    // Cho Android và iOS
        }
      ]
    ]
  }
}
```

    * `supportsBackgroundPlayback` (chỉ iOS): Bật phát lại nền.
    * `supportsPictureInPicture`: Bật chế độ Picture-in-Picture.

* **Phát media cục bộ từ thư mục assets:**
    Hỗ trợ phát media được tải bằng hàm `require`.

```javascript
import { VideoSource, useVideoPlayer } from 'expo-video'; // Import necessary components

const assetId = require('./assets/bigbuckbunny.mp4');

const videoSource: VideoSource = {
  assetId,
  metadata: {
    title: 'Big Buck Bunny',
    artist: 'The Open Movie Project'
  }
};

const player1 = useVideoPlayer(assetId); // You can use the `asset` directly as a video source
const player2 = useVideoPlayer(videoSource);
```

* **Preloading videos:**
    Có thể tải video trước khi hiển thị để chuyển đổi nhanh hơn.

```javascript
import { useVideoPlayer, VideoView, VideoSource } from 'expo-video';
import { useState, useCallback } from 'react'; // Import necessary hooks
import { View, Text, TouchableOpacity, StyleSheet } from 'react-native'; // Import necessary components

// Assume video sources are defined elsewhere
const bigBuckBunnySource: VideoSource = 'YOUR_BIG_BUCK_BUNNY_VIDEO_SOURCE';
const elephantsDreamSource: VideoSource = 'YOUR_ELEPHANTS_DREAM_VIDEO_SOURCE';

export default function PreloadingVideoPlayerScreen() {
  const player1 = useVideoPlayer(bigBuckBunnySource, player => {
    // Optional: You can perform actions when the player is ready
    // player.play();
  });
  const player2 = useVideoPlayer(elephantsDreamSource, player => {
    // Optional: You can perform actions when the player is ready
    // player.currentTime = 20;
  });

  const [currentPlayer, setCurrentPlayer] = useState(player1);

  const replacePlayer = useCallback(() => { // Removed async as it's not needed for pause/play
    if (currentPlayer) {
      currentPlayer.pause();
    }
    if (currentPlayer === player1) {
      setCurrentPlayer(player2);
      player2?.play(); // Use optional chaining in case player2 is null/undefined
    } else {
      setCurrentPlayer(player1);
      player1?.play(); // Use optional chaining
    }
  }, [player1, player2, currentPlayer]); // Added player2 to dependencies

  return (
    <View style={styles.contentContainer}>
      <VideoView player={currentPlayer} style={styles.video} nativeControls={false} />
      <TouchableOpacity style={styles.button} onPress={replacePlayer}>
        <Text style={styles.buttonText}>Replace Player</Text>
      </TouchableOpacity>
    </View>
  );
}

// Assume styles object is defined elsewhere
const styles = StyleSheet.create({
  contentContainer: { flex: 1, justifyContent: 'center', alignItems: 'center' },
  video: { width: 300, height: 200 },
  button: { marginTop: 20, padding: 10, backgroundColor: 'lightblue' },
  buttonText: { color: 'white' },
});
```

* `useVideoPlayer` hook quản lý playback.
* `VideoView` component hiển thị video.
* `VideoSource` đại diện cho nguồn video.

* **Sử dụng VideoPlayer trực tiếp (`createVideoPlayer`):**
    Trong các trường hợp nâng cao, có thể tạo `VideoPlayer` mà không tự động hủy khi component unmount. Cần gọi `release()` thủ công để tránh rò rò bộ nhớ.

```javascript
import { createVideoPlayer, VideoSource } from 'expo-video';

// Assume videoSource is defined
const videoSource: VideoSource = 'YOUR_VIDEO_SOURCE';

const player = createVideoPlayer(videoSource);
// Remember to call player.release() when the player is no longer needed
```

* **Nhận các sự kiện (Receiving events):**
    Các thay đổi trong `VideoPlayer` không cập nhật state React. Cần lắng nghe các sự kiện mà nó phát ra.

    * `useEvent` hook: Tạo listener trả về giá trị stateful. Tự động dọn dẹp khi component unmount.

    ```javascript
    import { useEvent } from 'expo';
    // ... Other imports, definition of the component, creating the player etc.
    // Assuming 'player' is a VideoPlayer instance
    const { status, error } = useEvent(player, 'statusChange', {
      status: player.status
    });
    // Rest of the component...
    ```

    * `useEventListener` hook: Tạo event listener với tự động dọn dẹp.

    ```javascript
    import { useEventListener } from 'expo';
    import { useState } from 'react'; // Import useState
    import { View, Text } from 'react-native'; // Import necessary components

    // ...Other imports, definition of the component, creating the player etc.
    // Assuming 'player' is a VideoPlayer instance passed as prop
    export default function MyVideoComponent({ player }) {
      const [playerStatus, setPlayerStatus] = useState(null);
      const [playerError, setPlayerError] = useState(null);

      useEventListener(player, 'statusChange', ({ status, error }) => {
        setPlayerStatus(status);
        setPlayerError(error);
        console.log('Player status changed: ', status);
      });

      // Rest of the component...
      return (
        <View>
          <Text>Player Status: {playerStatus}</Text>
          {playerError && playerError.message && <Text>Player Error: {playerError.message}</Text>} {/* Added check for playerError.message */}
          {/* Render VideoView or other UI */}
        </View>
      );
    }
    ```

    * `Player.addListener` method: Cách linh hoạt nhất nhưng yêu cầu dọn dẹp thủ công.

    ```javascript
    import { useEffect, useState } from 'react'; // Import useEffect and useState
    import { View, Text } from 'react-native'; // Import necessary components

    // ...Imports, definition of the component, creating the player etc.
    // Assuming 'player' is a VideoPlayer instance passed as prop
    export default function AnotherVideoComponent({ player }) {
      const [playerStatus, setPlayerStatus] = useState(null);
      const [playerError, setPlayerError] = useState(null);

      useEffect(() => {
        const subscription = player.addListener('statusChange', ({ status, error }) => {
          setPlayerStatus(status);
          setPlayerError(error);
          console.log('Player status changed: ', status);
        });

        return () => {
          subscription.remove();
        };
      }, [player]); // Add player to dependencies

      // Rest of the component...
      return (
        <View>
          <Text>Player Status: {playerStatus}</Text>
          {playerError && playerError.message && <Text>Player Error: {playerError.message}</Text>} {/* Added check for playerError.message */}
          {/* Render VideoView or other UI */}
        </View>
      );
    }
    ```

### Expo Camera

`expo-camera` cung cấp một component React hiển thị bản xem trước của camera trước hoặc sau của thiết bị.

* **Cài đặt:**

```bash
npx expo install expo-camera
```

* **Cấu hình trong `app.json` (với plugin):**

```json
"expo": {
  "plugins": [
    [
      "expo-camera",
      {
        "cameraPermission": "Allow $(PRODUCT_NAME) to access your camera",
        "microphonePermission": "Allow $(PRODUCT_NAME) to access your microphone",
        "recordAudioAndroid": true
      }
    ]
  ]
}
```

* **Permissions:**

    * **Android:** Cần thêm quyền `CAMERA` và `RECORD_AUDIO` (nếu quay video có âm thanh) vào `app.json`.

```json
    "android": {
      "permissions": [
        "CAMERA",
        "RECORD_AUDIO"
      ]
    },
```

* **iOS:** Cần khai báo `NSCameraUsageDescription` và `NSMicrophoneUsageDescription` trong `app.json` để hiển thị thông báo quyền cho người dùng.

* **Sử dụng Expo Camera:**
    Ứng dụng sử dụng Expo Camera cho phép mở/đóng camera, chuyển đổi giữa camera trước/sau, và yêu cầu quyền camera.

    * **Import:**

```javascript
import { CameraView, CameraType, useCameraPermissions } from 'expo-camera';
import { useState } from 'react'; // Import useState
import { View, Text, Button, TouchableOpacity, StyleSheet } from 'react-native'; // Import necessary components
```

* **Khởi tạo State:**

```javascript
const [facing, setFacing] = useState('back'); // 'back' or 'front'
const [cameraActive, setCameraActive] = useState(false);
const [permission, requestPermission] = useCameraPermissions();
```

* **Xử lý quyền:**

```javascript
import { useCameraPermissions } from 'expo-camera';
import { View, Text, Button, StyleSheet } from 'react-native'; // Import necessary components

// Assume styles object is defined elsewhere
const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: 'center', alignItems: 'center' },
  message: { textAlign: 'center', paddingBottom: 10 },
});


function CameraPermissionHandler() { // Example component
  const [permission, requestPermission] = useCameraPermissions();

  if (!permission) {
    // Camera permissions are still loading.
    return <View />;
  }
  if (!permission.granted) {
    // Camera permissions are not granted yet.
    return (
      <View style={styles.container}>
        <Text style={styles.message}>We need your permission to show the camera</Text>
        <Button onPress={requestPermission} title="Grant Permission" />
      </View>
    );
  }

  // If permission is granted, you can render the camera controls or CameraView here
  return <View>{/* Render camera controls or CameraView */}</View>;
}

export default CameraPermissionHandler; // Export the component
```

* **Hàm lật Camera:**

```javascript
import { useState } from 'react'; // Import useState
import { CameraType } from 'expo-camera'; // Import CameraType

function useCameraToggle() { // Custom hook for camera state
  const [facing, setFacing] = useState<CameraType>('back'); // Use CameraType for type safety

  const toggleCameraFacing = () => {
    setFacing((current) => (current === 'back' ? 'front' : 'back'));
  };

  return { facing, toggleCameraFacing };
}

// Usage in a component:
// const { facing, toggleCameraFacing } = useCameraToggle();
```

* **Hàm bật/tắt Camera:**

```javascript
import { useState } from 'react'; // Import useState

function useCameraActive() { // Custom hook for camera active state
  const [cameraActive, setCameraActive] = useState(false);

  const toggleCamera = () => {
    setCameraActive((current) => !current);
  };

  return { cameraActive, toggleCamera };
}

// Usage in a component:
// const { cameraActive, toggleCamera } = useCameraActive();
```

  * **Render CameraView có điều kiện:**

```javascript
import { CameraView, CameraType } from 'expo-camera';
import { View, StyleSheet } from 'react-native'; // Import necessary components

// Assume cameraActive and facing states are managed elsewhere
// Assume styles object is defined elsewhere

const styles = StyleSheet.create({
  camera: { flex: 1 }, // Example style
});

function CameraRenderer({ cameraActive, facing }) { // Example component to render camera
  return (
    <> {/* Use a Fragment */}
      {cameraActive && (
        <CameraView style={styles.camera} facing={facing}>
          {/* ... các controls hoặc overlay khác bên trong camera view */}
        </CameraView>
      )}
    </>
  );
}
```

* `cameraActive` điều khiển việc hiển thị CameraView.
* `style={styles.camera}` áp dụng kiểu dáng.
* `facing={facing}` đặt camera trước hoặc sau.

**Nút lật Camera (hoặc đóng Camera tùy thuộc vào logic đầy đủ):**

```javascript
import { TouchableOpacity, Text, StyleSheet } from 'react-native'; // Import necessary components
// Assume toggleCameraFacing or toggleCamera function is available in the component's scope
// Assume styles object is defined elsewhere

const styles = StyleSheet.create({
  button: { /* button styles */ },
  text: { /* text styles */ },
});


function CameraControlButton({ onPress, title }) { // Reusable button component
  return (
    <TouchableOpacity style={styles.button} onPress={onPress}>
      <Text style={styles.text}>{title}</Text>
    </TouchableOpacity>
  );
}

// Usage in a component:
// <CameraControlButton onPress={toggleCameraFacing} title="Flip Camera" />
// <CameraControlButton onPress={toggleCamera} title="Close Camera" />
```

* `TouchableOpacity` tạo nút có phản hồi chạm.
* `onPress` gọi hàm xử lý sự kiện.

**Nút mở Camera (hiển thị khi Camera không hoạt động):**

```javascript
import { Button } from 'react-native'; // Import Button
// Assume cameraActive and toggleCamera function is available in the component's scope

function OpenCameraButton({ cameraActive, toggleCamera }) { // Example component
  return (
    <> {/* Use a Fragment */}
      {!cameraActive && (
        <Button title="Open Camera" onPress={toggleCamera} />
      )}
    </>
  );
}
```
