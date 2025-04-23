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


<!-- ### The `<TextInput>` component
- A basic component for inputting text into the app via a keyboard.
- Important props (attributes):
	- : set the predefined value for the text field
	-: displays a placeholder text when the input is empty
	-: set the function to call every time the text value changes.
	-: make the input editable or not (default:)
	- Some other common props:
 -->
<!-- 
### The `<Button>` component
- A basic button that should render nicely on any platform.
- Supports a minimal level of customization.
- If more customization is needed,
- Important props (attributes):
	•: the actual display text of the button
	•: the event handling attribute
	•: set the theme color for the button
	(renders differently on iOS and Android)
	- Other props: -->

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
import React, { Component } from 'react';
import { Button, Text, View } from 'react-native';
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
import React, { useEffect, useMemo, useState } from 'react';
import { Button, Text, View } from 'react-native';
const App = ({ multiplier }) => {
const [count, setCount] = useState(0);
// useMemo will re-run when edited
const double = useMemo(() => count * multiplier, [count]);
useEffect(() => {
console.log('Component re-rendered');
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

<KeyboardAvoidingView style={styles.screen} behavior="position">

<KeyboardAvoidingView 
	style={{
		flex: 1,
		alignItems: 'center',
		justifyContent: 'center',
		backgroundColor: '#ECF0F1'
	}} 
	behavior="height">
	<StatusBar barStyle="light-content" />
	<TextInput 
		style={{
			padding: 10,
			borderWidth: 1,
			borderRadius: 5,
			backgroundColor: '#ccc'
		}} 
		placeholder='Enter something' 
	/>
</KeyboardAvoidingView>
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
	{goals.map(
	(g, i) => <Text key={i} style={styles.goalItem}>{g}</Text>)}
	</ScrollView>
</View>
```

FlatList (Better performance):

```jsx
<View style={styles.bottomSection}>
	<FlatList
		data={goals}
		renderItem={(obj) => <Text key={obj.index} style={styles.goalItem}>{obj.item}</Text>}
	/>
</View>
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
      <Text style={styles.modalText}>Hello World!</Text>
      <Pressable
        style={[styles.button, styles.buttonClose]}
        onPress={() => setModalVisible(!modalVisible)}>
        <Text style={styles.textStyle}>Hide Modal</Text>
      </Pressable>
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
import React from 'react';
import { View, Text, Button, Alert, StyleSheet } from 'react-native';
	const HandleEventDemo = () => {
		const handlePress = () => {
		Alert.alert('Button Pressed!', 'You clicked the button.');
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
import React, { useState } from 'react';
import { View, Text, Button, StyleSheet } from 'react-native';

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
import { ImageBackground, Text, StyleSheet } from 'react-native';
export default function App() {
	return (
		// Use it as a wrapper to display content on top of an image.
		<ImageBackground
			source={require('./assets/background.jpg')}
			style={styles.background}
		>
			<Text style={styles.text}>Weather App</Text>
		</ImageBackground>
	);
}
```

### StatusBar

```jsx
import { StatusBar } from 'react-native';
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
import { ActivityIndicator, View } from 'react-native';
export default function App() {
	return (
		<View>
		<ActivityIndicator
		size="large"
		color="#0000ff"
		animating={true} />
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
				borderColor: '#ccc',
				margin: 10
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