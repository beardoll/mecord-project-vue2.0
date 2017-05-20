# mecord-project-vue2.0
using vue 2.0 to implement the project

### How to debug in PC?

1. In `src/main.js`, focus on `methods/login`, uncomment the following sentences:
  ```javascript
  this.accesstoken.id = data    
  this.accesstoken.userId = 1  
  ```
  
2. then comment the following sentences:
  ```javascript
  this.accesstoken.userId = data.userId
  this.accesstoken.id = data.id
  ```
  
3. In `components/login/Gate.vue`, focus on script codes, then uncomment all the codes in `mounted` model. Also you should comment the `created` model together with the sentence:
  ```javascript
  var qs = require('querystring')
  ```
4. When debugging in PC, you should get the accesstoken at the backend and write the info into the `mounted` model of `components/login/Gate.vue`:
  ```javascript
  this.$root.login(wxurl, 'ez5vvico5xy020LMruUEZW7ed2xAW9u7RPap5YFINk3pNFlS6IDFYUf4VFErmjWI')  
  ```
5. After that, in terminal, use `cd` order to reach the root directory of the project, and run:
  ```Shell
  npm run dev
  ```
6. Remember that if you have not installed the dependencies modules, you should execute:
  ```Shell
  npm install
  ```
### The data flow of answers

1. common questions ('blank', 'select', 'multi-blank', 'multi-select')

   In `components/Answers/QuestionList.vue`, you can see the disposals of questions and the corresponding answers. At the `<template>` module, you can see how we render the questions in vue component. Remember that in each component, we only render on question, which is decided by the conditional render syntax.
   All answers here will be saved in the `<form>` module, we can get the contents by applying:
   ```javascript
   formjson = $('form').serializeArray()
   ```
   The above code was written in function `dispatchAnswer(questionItem)` in `method` module.
   After receving the content, you should transform its format to what we need. This operatoin can be found in the `switch` syntax in 'dispatchAnswer(questionItem)'. 

### The result of testing on PASCAL VOC 2007 

```
| Classes       | AP    |
|-------------|--------|
| aeroplane   | 0.698 |
| bicycle     | 0.788 |
| bird        | 0.657 |
| boat        | 0.565 |
| bottle      | 0.478 |
| bus         | 0.762 |
| car         | 0.797 |
| cat         | 0.793 |
| chair       | 0.479 |
| cow         | 0.724 |
| diningtable | 0.648 |
| dog         | 0.803 |
| horse       | 0.797 |
| motorbike   | 0.732 |
| person      | 0.770 |
| pottedplant | 0.384 |
| sheep       | 0.664 |
| sofa        | 0.650 |
| train       | 0.766 |
| tvmonitor   | 0.666 |
| mAP        | 0.681 |
```

