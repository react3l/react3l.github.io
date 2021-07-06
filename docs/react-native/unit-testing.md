---
layout: default
title: Unit testing
parent: React Native Testing
nav_order: 2
---

# Unit testing
<https://github.com/callstack/react-native-testing-library>

## Why needed?
**Unit testing** ensures that all code meets quality standards before it's deployed. This ensures a reliable engineering environment where quality is paramount. Over the course of the product development life cycle, **unit testing** saves time and money, and helps developers write better code, more efficiently.
## Installation
```ts
npm install --save-dev @testing-library/react-native
# or
yarn add --dev @testing-library/react-native
```

### Additional Jest matchers 
In order to use additional React Native-specific jest matchers from `@testing-library/jest-native` package add it to your project:
```ts
npm install --save-dev @testing-library/jest-native
# or
yarn add --dev @testing-library/jest-native
```
### Custom Jest Preset
1. Then automatically add it to your jest tests by using setupFilesAfterEnv option in your Jest configuration (it's usually located either in `package.json` under "jest" key or in a `jest.config.json` file). 
2. If not automatically added, you need to add the following code to `package.json`:
```ts
"jest": {
    "preset": "react-native",
    "setupFilesAfterEnv": [
      "@testing-library/jest-native/extend-expect"
    ],
    "transformIgnorePatterns": [
      "node_modules/(?!(jest-)?react-native|@?react-navigation)"
    ],
    "setupFiles": [
      "./node_modules/react-native-gesture-handler/jestSetup.js"
    ],
    "moduleFileExtensions": [
      "ts",
      "tsx",
      "js",
      "jsx",
      "json",
      "node"
    ]
  }
```
3. You need to add the following code to your `scripts` in `pakage.json` to execute the test command:
```ts
"test": "jest --watchAll"
```

---

### Flow
Note for Flow users – you'll also need to install typings for `react-test-renderer`:
```ts
flow-typed install react-test-renderer
```
### Writing unit tests in TypeScript
```ts
  npm install --save @types/jest
  # or
  yarn add @types/jest
```
---
## How to write unit tests?
1. Identify the object to be test.
2. Create a file containing the test code.
3. Define Input, Output.
4. Proceed to write test code for the defined object.
5. Run `$ yarn test` or `$ npm run test`.

## How to write tests for functions?
1. Define a function to test.
2. Create a file or choose an existing file that will contain our actual test.
3. Determine the input and output values of the function.
4. Compare the actual output value with the expected output value returned.


Let's get started by writing a test for a hypothetical function that adds two numbers. First, create a `mathUntils.ts` file:
```ts
const sum = ( x: number, y:number ) => {
    return x+y;
}
export {
    sum
}
```
Then, create a file named `mathUntils.test.ts`. This will contain our actual test:
```ts
import { sum } from '../../functions/mathUntils';

test('adds 1 + 4 to equal 5', async () => {
    expect(sum(1,4)).toBe(5)
});
```
Finally, run `$ yarn test` or `$ npm run test` and Jest will print this message:

<div style="margin:20px 10px 10px 50px; width: 600px; "  markdown="1"> 

  ![resul_message](/assets/images/resultTestF.jpg)

</div>

---
Here is an example of testing Function with side effects, define one more function at `mathUntils.ts` file:
```ts
let isChanged = false; // defined is false

const changeFlowerName = (name: string) => {
    isChanged = true; // changes in value to true
    return name;
}
```
One more test case at `mathUntils.test.ts`:
```ts
test("test change", async () => {
  const result = changeFlowerName("have changed");
  console.log(isChanged);
  expect(result).toBe("have changed")
})
```
Finally, run `$ yarn test` or `$ npm run test` and Jest will print this message:
<div style="margin:20px 10px 10px 50px; width: 600px; "  markdown="1"> 

  ![resul_message](/assets/images/resultTestEF.jpg)

</div>

## Example Test a screen
There is a piece of code that allows the user to enter the length and width of the rectangle. Then display the resulting rectangular area on the screen: 

<div style="margin:20px 10px 10px 50px; width: 200px; "  markdown="1"> 

  ![example_design](/assets/images/exampleUnitTest-design.jpg)
</div>

In the `TabOneScreen.tsx` file, there is return : 
```ts
return (
    <ScrollView style={{ backgroundColor: '#fff' }}>
      <View style={styles.ContainerView}>
        <View style={{ flex: 0.1, alignItems: 'center', paddingTop: 50 }}>
          <Text style={styles.Title}>Tính diện tích hình chữ nhật</Text>
        </View>
        <View style={{ flex: 0.6, paddingTop: 50, marginBottom: 40 }}>
          <Input title={'Nhập chiều dài'} value={lenght} onChangeValue={(text) => setLenght(text)} TestId={"lenght"}></Input>
          <Input title={'Nhập chiều rộng'} value={width} onChangeValue={(text) => setWidth(text)} TestId={"width"}></Input>
          <Text  style={styles.Title} >kết quả : </Text> 
          <Text testID="result" style={styles.Title} >{value}</Text> 
        </View>

        <View style={{ flex: 0.3, justifyContent: 'space-between', }}>
          <TouchableOpacity 
            onPress={ValuePress} 
            testID="perform"
            style={styles.TouchableButton}>
            <Text style={{ color: 'white' }}>Thực hiện </Text>
          </TouchableOpacity>

        </View>
      </View>
    </ScrollView>
  );
```

Now write unit test:
1. Input: Lenght, Width.
2. Ouput: Rectangular area.
3. To perform unit test for the above function, we need to create a file `TabOneScreen.test.js`.
We do the following :
```ts
import React from 'react';
import {render, fireEvent} from '@testing-library/react-native';
import TabOneScreen from '../../screens/TabOneScreen';

test('TabOne should render OK', async () => {
  const {getByText, getByTestId, getAllByTestId, queryByText} = render(
    <TabOneScreen />,
  );
  const inputLenght = getByTestId('lenght');
  const inputWidth = getByTestId('width');
  const button = getByTestId('perform');
  fireEvent.changeText(inputLenght, '5');
  fireEvent.changeText(inputWidth, '10');
  fireEvent.press(button);
  
  const result = getByTestId('result');
  expect(result.props.children).toBe(50);
});

```

4. Testing and Check the result.
```ts
 $ yarn test
```
<div style="margin:20px 10px 10px 50px; width: 600px; "  markdown="1"> 

  ![ResultUnit_test_design](/assets/images/result-testing.jpg)
</div>

## Queries: 
Queries are the methods that Testing Library gives you to find elements on the page.
```ts
   ...
   getByTestId()
   getAllByTestId() 
   queryByText()
   ...
```
[View more](https://testing-library.com/docs/queries/about) in Testing Library.

[View project on GitHub](https://github.com/DuongThiHuongSen/Demo-LibraryTesting-ReactNative).

