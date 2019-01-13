# React Native

## Styles
  - Suffix names with `Container`, `Button`, `Text`, `Title`, `Subtitle`, `Description` depending on the component it refers to.
  - **NEVER name styles with reserved keywords**. This does not provide enough context for the component and defeats the purpose of extending the style for the component.
    ```
    const styles = {
      paddingLeftContainer: {
        paddingLeft: 8,
      }
    }

    <View style={styles.paddingLeftContainer}>
      ...
    </View>
    ```
  - Reserve **inline styles** for implementing dynamic values such as animating height, opacity, etc.
## Component

- Don't ignore warning messages during development. Resolve them immediately.
- **Never cut and paste components**.
- **Don't duplicate passed down props as component-level state**. [Lift state up](https://reactjs.org/docs/lifting-state-up.html) to have a **single source of truth**.
- [Think in React](https://reactjs.org/docs/thinking-in-react.html) with **one-way** data flow while favouring [Composition over Inheritance](https://reactjs.org/docs/composition-vs-inheritance.html).
    - **Top-down** when building components
    - **Bottom-up** when writing tests as you build. 

## Folder Structure

- Organized by **feature**. Inspired by this [post](https://medium.freecodecamp.org/how-to-structure-your-project-and-manage-static-resources-in-react-native-6f4cfc947d92) from medium

```
index.android.js
index.ios.js
App.js
package.json
ios/
android/
src
  features
    authentication
      LoginScreen.js
      SignupScreen.js
      SmsCodeScreen.js
      SuccessScreen.js
    directory
      HomeScreen.js
      Navigator.js
  lib
    package.json
    components
      index.js
      ImageButton.js
      RoundImage.js
      NavigationHeader.js
    utils
      moveToBottom.js
      safeArea.js
      usersHelper.js
    networking
      API.js
      Auth.js
    redux
      actions/
      reducers/
  res
    package.json
    strings.js
    colors.js
    palette.js
    fonts.js
    images.js
    images
      logo@2x.png
      logo@3x.png
      button@2x.png
      button@3x.png
scripts
```

### Notes

- `src` will have 3 main folders: `features`, `lib`, `res`
- `lib` and `res` will have their own `package.json` to act as local node modules to enable importing with absolute path for `feature` screens.
  ```
  // ------ HomeScreen.js ------

  // Old
  import { NavigationHeader } from '../../lib/components';

  // New
  import { NavigationHeader } from 'lib/components';
  ```
- `index.js` optional depending on parent folder.