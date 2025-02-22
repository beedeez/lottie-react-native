# Lottie for React Native, [iOS](https://github.com/airbnb/lottie-ios), and [Android](https://github.com/airbnb/lottie-android)

[![npm Version](https://img.shields.io/npm/v/lottie-react-native.svg)](https://www.npmjs.com/package/lottie-react-native) [![License](https://img.shields.io/npm/l/lottie-react-native.svg)](https://www.npmjs.com/package/lottie-react-native)

Lottie component for React Native (iOS and Android)

Lottie is a mobile library for Android and iOS that parses [Adobe After Effects](http://www.adobe.com/products/aftereffects.html) animations exported as JSON with [bodymovin](https://github.com/bodymovin/bodymovin) and renders them natively on mobile!

For the first time, designers can create **and ship** beautiful animations without an engineer painstakingly recreating it by hand.

## Installing (React Native >= 0.60.0)

Install `lottie-react-native` (latest) and `lottie-ios` (3.0.3):

```
yarn add lottie-react-native
yarn add lottie-ios@3.0.3
```

or

```
npm i --save lottie-react-native
npm i --save lottie-ios@3.0.3
```

Go to your ios folder and run:

```
pod install
```

**_ IMPORTANT _**

If you have issues linking your **iOS** project check out this [StackOverflow thread](https://stackoverflow.com/questions/52536380/why-linker-link-static-libraries-with-errors-ios) on how to fix it.

If your app crashes on **Android**, means auto linking didn't work. You will need to make the following changes:

**android/app/src/main/java/\<AppName\>/MainApplication.java**

- add `import com.airbnb.android.react.lottie.LottiePackage;` on the imports section
- add `packages.add(new LottiePackage());` in `List<ReactPackage> getPackages()`;

**android/app/build.gradle**

add `implementation project(':lottie-react-native')` in the `dependencies` block

**android/settings.gradle**

add:

```
include ':lottie-react-native'
project(':lottie-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/lottie-react-native/src/android')

```


## Installing (React Native == 0.59.x)

Install `lottie-react-native` (latest) and `lottie-ios` (3.0.3):

```
yarn add lottie-react-native
yarn add lottie-ios@3.0.3
```

or

```
npm i --save lottie-react-native
npm i --save lottie-ios@3.0.3
```

Use `react-native link` to add the library to your project:

```
react-native link lottie-ios
react-native link lottie-react-native
```

Go to your ios folder and run:

```
pod install
```

**_ IMPORTANT _**

If you have issues with your iOS project, open the Xcode project configuration and add the `Lottie.framework` as `Embedded Binaries`.

Apps that use static Xcode project linking need to set iOS deployment version to iOS 12 _or_ switch to CocoaPods-based linking (using frameworks) _or_ downgrade `lottie-react-native` to version **_2.6.1_**.

## Installing (React Native <= 0.58.x)

Install `lottie-react-native` (2.5.11) and `lottie-ios` (2.5.3):

```
yarn add lottie-react-native@2.5.11
yarn add lottie-ios@2.5.3
```

or

```
npm i --save lottie-react-native@2.5.11
npm i --save lottie-ios@2.5.3
```

Use `react-native link` to add the library to your project:

```
react-native link lottie-ios
react-native link lottie-react-native
```

Go to your ios folder and run:

```
pod install
```

**_ IMPORTANT _**

If you have issues with your iOS project, open the Xcode project configuration and add the `Lottie.framework` as `Embedded Binaries`.

Apps that use static Xcode project linking need to set iOS deployment version to iOS 12 _or_ switch to CocoaPods-based linking (using frameworks) _or_ downgrade `lottie-react-native` to version **_2.6.1_**.

## Usage

(If you are using TypeScript, please read [this first](/docs/typescript.md))

LottieView can be used in a declarative way:

```jsx
import React from 'react';
import LottieView from 'lottie-react-native';

export default class BasicExample extends React.Component {
  render() {
    return <LottieView source={require('./animation.json')} autoPlay loop />;
  }
}
```

Additionally, there is an imperative API which is sometimes simpler.

```jsx
import React from 'react';
import LottieView from 'lottie-react-native';

export default class BasicExample extends React.Component {
  componentDidMount() {
    this.animation.play();
    // Or set a specific startFrame and endFrame with:
    this.animation.play(30, 120);
  }

  render() {
    return (
      <LottieView
        ref={animation => {
          this.animation = animation;
        }}
        source={require('../path/to/animation.json')}
      />
    );
  }
}
```

Lottie's animation progress can be controlled with an `Animated` value:

```jsx
import React from 'react';
import { Animated, Easing } from 'react-native';
import LottieView from 'lottie-react-native';

export default class BasicExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      progress: new Animated.Value(0),
    };
  }

  componentDidMount() {
    Animated.timing(this.state.progress, {
      toValue: 1,
      duration: 5000,
      easing: Easing.linear,
    }).start();
  }

  render() {
    return (
      <LottieView source={require('../path/to/animation.json')} progress={this.state.progress} />
    );
  }
}
```

## API

You can find the full list of props and methods available in our [API document](https://github.com/airbnb/lottie-react-native/blob/master/docs/api.md). These are the most common ones:

| Prop           | Description                                                                                                                                                                                                                                                                     | Default                                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **`source`**   | **Mandatory** - The source of animation. Can be referenced as a local asset by a string, or remotely with an object with a `uri` property, or it can be an actual JS object of an animation, obtained (for example) with something like `require('../path/to/animation.json')`. | _None_                                                                                                              |
| **`style`**    | Style attributes for the view, as expected in a standard [`View`](https://facebook.github.io/react-native/docs/layout-props.html).                                                                                                                                              | The `aspectRatio` exported by Bodymovin will be set. Also the `width` if you haven't provided a `width` or `height` |
| **`loop`**     | A boolean flag indicating whether or not the animation should loop.                                                                                                                                                                                                             | `true`                                                                                                              |
| **`autoPlay`** | A boolean flag indicating whether or not the animation should start automatically when mounted. This only affects the imperative API.                                                                                                                                           | `false`                                                                                                             |

[More...](https://github.com/airbnb/lottie-react-native/blob/master/docs/api.md)

## More

View more documentation, FAQ, help, examples, and more at [airbnb.io/lottie](https://airbnb.io/lottie/)

![Example1](docs/gifs/Example1.gif)

![Example2](docs/gifs/Example2.gif)

![Example3](docs/gifs/Example3.gif)

![Community](docs/gifs/Community%202_3.gif)

![Example4](docs/gifs/Example4.gif)
