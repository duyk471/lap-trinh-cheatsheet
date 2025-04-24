# Học MPR để đi thi cực nhanh

Tổng hợp từ đống Lectures

## Tutorial 2

To create a new Expo project, run the following in your terminal:

```
npx create-expo-app@latest <app-name>
```

What’s inside a React Native project?

```
.gitignore:
.expo
.vscode
node_modules:
#Holds 3rd party packages
assets:
#Holds images used inside app
package.json, package-lock.json:
#Holds dependencies, script commands, etc.
app.json:
#Configuration settings
App.js:
#The real code
```

Cách chạy ứng dụng

```bash
npx expo install react-dom react-native-web @expo/metro-runtime
npm run web
```
To run your app on Expo Go:

1. Open terminal
2. Type “npx expo start”

There’s a function component called `App`. This component acts as the root component by default.
In React Native, you can’t use HTML tags in JSX code. Unlike React where your application runs on the browser, React Native application runs on mobile devices

[React Native components documentation](https://reactnative.dev/docs/components-and-apis)

| React Native UI Component | Android View | iOS View | Web Analog | Description |
| --- | --- | --- | --- | --- |
| `<View>` | `<ViewGroup>` | `<UIView>` | A non-scrolling `<div>` | A container that supports layout with flexbox, style, some touch handling, and accessibility controls |
| `<Text>` | `<TextView>` | `<UITextView>` | `<p>` | Displays, styles, and nests strings of text and even handles touch events |
| `<Image>` | `<ImageView>` | `<UIImageView>` | `<img>` | Displays different types of images |
| `<ScrollView>` | `<ScrollView>` | `<UIScrollView>` | `<div>` | A generic scrolling container that can contain multiple components and views |
| `<TextInput>` | `<EditText>` | `<UITextField>` | `<input type="text">` | Allows the user to enter text |

React Native styling There Is No CSS! Inline Styles <-> StyleSheet Objects. Written in JavaScript

## Lecture 3

### What is Fast Refresh?
- Fast Refresh is a feature in React that updates the application instantly after editing the source code without reloading the entire page.
- Key benefits:
	- Preserves state in function components and Hooks.
	- Speeds up development and reduces interruptions.

	
Limitations: State is not preserved in:

- Class components.
- Modules with multiple exports besides React components.
- Higher-order components returning class components.

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

- useState and useRef: Preserve their values if the arguments are unchanged.
- useEffect, useMemo, and useCallback: Always update during Fast Refresh, even if dependencies don’t change.

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

Debugging JavaScript Remotely: Type “?” on Terminal to see full list of command while `npm start` process is running. Press m to toggle a menu on device/emulator.


### Building Adaptive User Interfaces
Responsive Units:

- Use relative units like `%, vh, or vw` for widths and heights.
- Use scalable units like `em or rem` for font sizes
- Use third-party libraries such as `react-native-size-matters` to simplify responsive development.

Using `maxWidth` or `minWidth` besides the regular width to create more responsive sizes.

### Dimensions API

```jsx
import {Dimensions} from 'react-native';

const windowWidth = Dimensions.get('window').width;
const windowHeight = Dimensions.get('window').height;
```

`useWindowDimensions()` is the preferred API for React components.

```jsx
import {useWindowDimensions} from 'react-native';
const {width, height} = useWindowDimensions();
```

### Managing layout when keyboard is visible

`KeyboardAvoidingView`

```jsx
import {KeyboardAvoidingView} from 'react-native';

<KeyboardAvoidingView style={styles.screen} <TextInput .../> behavior="position">
```

### Handling user input

- State Management:
	- React Native uses React's `useState` hook to manage the input values dynamically.
	- The state holds the value entered by the user.
- Input Components:
	- `TextInput`: The core component for receiving user input.
	- `Button`: Used to handle submission or trigger an action.
- Events:
	- `onChangeText`: captures changes in TextInput (so that we can update the state).
	- `onPress`: handles button clicks (to trigger a function to process input data).


### Core component: ScrollView
```jsx
<View style={styles.bottomSection}>
  <ScrollView>
    {goals.map((g, i) => (
      <Text key={i} style={styles.goalItem}>
        {g}
      </Text>
    ))}
  </ScrollView>
</View>;

```

FlatList (Better performance):

```jsx
<View style={styles.bottomSection}>
  <FlatList
    data={goals}
    renderItem={(obj) => (
      <Text key={obj.index} style={styles.goalItem}>
        {obj.item}
      </Text>
    )}
  />
</View>;

```

### Core component: Pressable
Used to make other components pressable just like `Button` or `TouchableOpacity`

```jsx: 
<Pressable 
	// Only works on Android: A feedback effect when a Pressable is touched
	android_ripple={{ color: '#cccccc' }}
	onPress={() => { console.log('Pressed') }}>
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
		uri: 'https://reactnative.dev/img/tiny_logo.png'
	}}
/>
```

### Core component: Modal

The Modal component is a basic way to present content above an enclosing view.

```jsx
 <Modal
  animationType="slide"
  transparent={true}
  visible={modalVisible}
  onRequestClose={() => {
    Alert.alert('Modal has been closed.');
    setModalVisible(!modalVisible);
  }}>
  <View style={styles.centeredView}>
    <View style={styles.modalView}>
      ....
    </View>
  </View>
</Modal>
```

## Lecture 4

Events | Description
--- | --- 
onPress | Triggered when a user taps a button or Touchable component.
onChangeText | Fires when text changes in an TextInput field.
onSubmitEditing | Executes when the user presses "Enter" or submits input.
onLongPress | Activated when a user presses and holds a button.
onFocus / onBlur | Used for handling focus state in input fields.


```jsx
import React from "react";
import { View, Text, Button, Alert, StyleSheet } from "react-native";
const HandleEventDemo = () => {
  const handlePress = () => {
    Alert.alert("Button Pressed!", "You clicked the button.");
  };
  return (
    <View style={styles.container}>
      <Text>Press the button below:</Text>
      <Button title="Click Me" onPress={handlePress} />
    </View>
  );
};
export default HandleEventDemo;

```

Recall: The useState hook

```jsx
const [state, setState] = useState(initialValue);
```

Example: Managing State with useState

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

```

- The count state stores a number.
- setCount updates the value when the button is clicked.

Syntax of `useEffect` (Don't get it, will revise later)

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
import { ImageBackground, Text, StyleSheet } from "react-native";
export default function App() {
  return (
    // Use it as a wrapper to display content on top of an image.
    <ImageBackground
      source={require("./assets/background.jpg")}
      style={styles.background}
    >
      <Text style={styles.text}>Weather App</Text>
    </ImageBackground>
  );
}

```

### StatusBar

```jsx
import { StatusBar } from "react-native";
export default function App() {
  return (
    <>
      <StatusBar barStyle="light-content" backgroundColor="#000" />
    </>
  );
}

```

### Core component: `ActivityIndicator`

```jsx
import { ActivityIndicator, View } from "react-native";
export default function App() {
  return (
    <View>
      <ActivityIndicator size="large" color="#0000ff" animating={true} />
    </View>
  );
}

```

### Core component: TextInput

```jsx
export default function AppTextInput() {
  return (
    <TextInput
      style={{
        padding: 10,
        borderWidth: 1,
        borderColor: "#ccc",
        margin: 10,
      }}
      placeholder="Enter city name"
      onChangeText={(text) => console.log(text)}
    />
  );
}
```

### Calling an API in React

Calling APIs in React Native

- Using fetch() for API calls
- Example API URL for weather information: https://api.open-meteo.com/v1/forecast?latitude=35&longitude=139&current_weather=true

```jsx
const getWeather = async () => {
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

To use the native stack navigator, we need to install:

```
npm install @react-navigation/native-stack
npm install @react-navigation/elements
```

### React Navigation: Static vs Dynamic APIs

#### Creating a native stack navigator (Static)

The createNativeStackNavigator function takes a configuration object and returns a stack navigator.

- The object contains screens and customization options.
- The screens are React components that will be displayed by the navigator.

The createStaticNavigation function takes the navigator created earlier and returns a component that can be rendered in the app. It's only called once in an app.

```jsx
import { createStaticNavigation } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>Home Page</Text>
    </View>
  );
}

function AboutScreen() {}

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

w/ options:

```jsx
const RootStack = createNativeStackNavigator({
  initialRouteName: "Home",
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

```

Cho tat ca luon

```jsx
const RootStack = createNativeStackNavigator({
  initialRouteName: "Home",
  screenOptions: {
    headerStyle: { backgroundColor: "tomato" },
  },
});

```

#### Creating a native stack navigator (Dynamic API)


The createNativeStackNavigator function returns an object containing 2 properties: Screen and Navigator. These components are used to create & configure the navigator structure. Navigator should contain Screen children. 

The NavigationContainer component manages the navigation tree and navigation state. 

• It must wrap all the navigators in the app. 
• It’s usually rendered as the root component of an app (the component exported from App.js)

```jsx
import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>Home Page</Text>
    </View>
  );
}

function AboutScreen() {}

const Stack = createNativeStackNavigator();
export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="About" component={AboutScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

```

warning: When using the Dynamic API, the component prop accepts a component, not a render function. Don't pass an inline function (e.g. `component={() => <HomeScreen />}`), or your component will unmount and remount, losing all state, when the parent component re- renders.


```jsx
<NavigationContainer>
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

Use React context and wrap the navigator with a context provider to pass data to the screens (recommended).

The `useContext` hook: A React Hook that lets you create context data (values) in a parent component and retrieve them from its descendant components.


```jsx
// First, use the createContext function to create a Context.
import { createContext, useContext } from "react";
const ScreenNameContext = createContext(null);

// Then, wrap the context’s provider around the components that will use the context value.
<ScreenNameContext.Provider value={scrNames}>
  <NavigationContainer>
    <Stack.Navigator>
      <Stack.Screen name="Home" component={HomeScreen} />
      <Stack.Screen name="About" component={AboutScreen} />
    </Stack.Navigator>
  </NavigationContainer>
</ScreenNameContext.Provider>;

```

### Retrieving Context Data from a component

```jsx
// Technically, any can retrieve the context data.
// However, it’s recommended that the component is one of the context provider’s descendant.
function AboutScreen() {
  const screenNames = useContext(ScreenNameContext);
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text>{screenNames.about}</Text>
    </View>
  );
}

```


### Navigating to a new screen

```jsx
import { useNavigation } from "@react-navigation/native";
function HomeScreen() {
  const navigation = useNavigation();
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>Home Page</Text>
      <Button title="About Us" onPress={() => navigation.navigate("About")} />
    </View>
  );
}
```

### Using push to add multiple instances

```jsx
// If you want to open a new instance of the About screen, use navigation.push('About').
//  Each time push is called, a new About screen instance is added to the navigation stack.
<Button title="About Us... again" onPress={() => navigation.push("About")} />;
```

### Difference between navigate and push

- navigate('About') → Does nothing if already on the About screen.
- navigate.push('About') → Creates a new instance of the About screen. This approach is useful when passing unique data to each instance of a screen.

### Manually Triggering Back Navigation

Use navigation.goBack() to programmatically navigate to the previous screen.


### Going Back Multiple Screens

```jsx
function AboutScreen() {
  const navigation = useNavigation();
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>About Page</Text>
      <Button
        title="About Us... again"
        onPress={() => navigation.push("About")}
      />
      <Button title="Go Back" onPress={() => navigation.goBack()} />
      <Button title="Go Home" onPress={() => navigation.popTo("Home")} />
      <Button
        title="Back to First Screen in Stack"
        onPress={() => navigation.popToTop()}
      />
    </View>
  );
}
```

### Passing parameters to routes


```jsx
function HomeScreen() {
  const navigation = useNavigation();
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 30 }}>Home Page</Text>
      <Button
        title="About Us"
        onPress={() => navigation.navigate("About", { name: "Quan" })}
      />
    </View>
  );
}

```

Receiving passed parameters:

```jsx
function AboutScreen({ route }) {
  const navigation = useNavigation();
  const { name } = route.params;
}
```

or init it using initialParams

```jsx
<Stack.Screen
  name="About"
  component={AboutScreen}
  initialParams={{ name: "Us" }}
/>;

// Or update it
navigation.setParams({
  name: "Vinh",
});

```

Avoid using setParams to update screen options such as title. If you need to update options, use setOptions instead.

### What should be in params?

Params should not be used for state management. If data is needed across multiple screens, it should be stored in a global store or cache.


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

## Lecture 6

### Configuring the header bar

```jsx
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{ title: "My home" }}
/>;

// Or using params in the title
<Stack.Screen
  name="Profile"
  component={ProfileScreen}
  options={({ route }) => ({ title: route.params.name })}
/>;

// Update dinamically

<Button onPress={() => navigation.setOptions({ title: "Updated!" })}>
  Update the title
</Button>;

// Customizing Header Styles
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{
    title: "My home",
    headerStyle: { backgroundColor: "#f4511e" },
    headerTintColor: "#fff",
    headerTitleStyle: { fontWeight: "bold" },
  }}
/>;

// Instead of repeating styles for each screen, set screenOptions in Stack.Navigator.
<Stack.Navigator
  screenOptions={{
    headerStyle: { backgroundColor: "#f4511e" },
    headerTintColor: "#fff",
    headerTitleStyle: { fontWeight: "bold" },
  }}
>
  <Stack.Screen
    name="Home"
    component={HomeScreen}
    options={{ title: "My home" }}
  />
  <Stack.Screen name="Details" component={DetailsScreen} />
</Stack.Navigator>;

// Replacing the Title with a Custom Component
// Use headerTitle to replace the text title with a custom component, such as an image or button.
function LogoTitle() {
  return (
    <Image
      style={{ width: 50, height: 50 }}
      source={require("@expo/snack-static/react-native-logo.png")}
    />
  );
}
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{ headerTitle: (props) => <LogoTitle {...props} /> }}
/>;


```


Difference Between title and headerTitle 

• title is used for multiple navigation types like tab bars and drawers. 
• headerTitle is specific to stack navigators and replaces the default Text component.


### Header interaction with its screen component

```jsx
function HomeScreen() {
  const navigation = useNavigation();
  const [count, setCount] = React.useState(0);
  React.useEffect(() => {
    navigation.setOptions({
      headerRight: () => (
        <Button onPress={() => setCount((c) => c + 1)}>Update count</Button>
      ),
    });
  }, [navigation]);
  return <Text>Count: {count}</Text>;
}
```

Customizing the Back Button

```jsx
<Stack.Screen
  name="Details"
  component={DetailsScreen}
  options={{
    headerLeft: () => (
      <Button onPress={() => alert("Custom Back Pressed")}>Back</Button>
    ),
  }}
/>;

```

### Example: Nesting Navigators

```jsx
const HomeScreen = () => (
  <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
    <Text>Home Screen</Text>
  </View>
);
const SettingsScreen = () => (
  <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
    <Text>Settings Screen</Text>
  </View>
);
const DetailsScreen = () => (
  <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
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

```

### Avoiding Multiple Headers

```jsx
<Stack.Screen
  name="Home"
  component={HomeTabs}
  options={{ headerShown: false }}
/>;

<Stack.Navigator screenOptions={{ headerShown: false }}>
```

This lecture we will work on the Time Tracker App (I'm gonna turn that to the Flowmodoro App)

## Lecture 7

###

```jsx
import React from "react";
import { StyleSheet, Text, View } from "react-native";
export default function LotsOfStyles() {
  return (
    <View style={styles.container}>
      <Text style={styles.red}>just red</Text>
      <Text style={styles.bigBlue}>just bigBlue</Text>
      <Text style={[styles.bigBlue, styles.red]}>bigBlue, then red</Text>
      <Text style={[styles.red, styles.bigBlue]}>red, then bigBlue</Text>
    </View>
  );
}
const styles = StyleSheet.create({
  container: { marginTop: 50 },
  bigBlue: { color: "blue", fontWeight: "bold", fontSize: 30 },
  red: { color: "red" },
});

```

After that we will learn the basics of Flexbox so I will skip

## Lecture 08: Tailwind CSS for React Native i.e. NativeWind

```
npm install nativewind@2.0.11 tailwindcss@3.2.2
# Optional
npm install react-native-reanimated
# (optional) 
npm install react-native-safe-area-context
```