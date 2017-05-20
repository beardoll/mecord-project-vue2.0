# mecord-project-vue2.0
using vue 2.0 to implement the project

## How to debug in PC?

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
## The data flow of answers
1. Render the question

   In `components/Answers/QuestionList.vue`, you can see the disposals of questions and the corresponding answers. At the `<template>` module, you can see how we render the questions in vue component. Remember that in each component, we only render one question, which is decided by the conditional render syntax.

2. Two important functions in `method` module
 
   First we introduce the function `setDefaultValue(questiontype, data)`
   
   ```javascript
   setDefaultValue(questiontype, data)
   // This function is called in parent 'Answer.vue', which aims to set the answers that users have filled right now
   // It's important while we look back to the past questions
   // It's called after the render of the component, which we will introduce later
   // It's easy to comprehend the procedure of loading the varible 'data' to our component, for example
   case 'select':
    if (data !== '') {  // 多项填空题，当答案不为空时
      var checkedindex = parseInt(data[0])  // json存的是字符而不是数字
      $('input').each(function (index, element) {
        if (index === checkedindex) {
          $(this).prop('checked', true)
        } else {
          $(this).prop('checked', false)
        }
      })
    } else {   // 当答案为空时
      $('input').each(function (index, element) {
        $(this).prop('checked', false)
      })
    }
    break
    // First we tranform the data into 'int' format
    // Then we use the function '.each' in 'jQuery' to traverse all the options, and mark the option with index equal to 'checked index' as 'checked' in '$('input')' module, for other options we just label them as 'unchecked', then you can see 'data' is successfully rendered in '<template>'
   ```
   
   To clearly show how it works, let's focus on the parent component `/components/Answers/Answer.vue`. Here we can find two methods, named `goToNextOne` and `backToPastOne`, respectively. Both of the methods apply `setDefaultValue(questiontype, data)`.
   ```javascript
   
   ```

3. common questions ('blank', 'select', 'multi_blank', 'multi_select')

   All answers here will be saved in the `<form>` module, we can get the contents by applying:
   ```javascript
   formjson = $('form').serializeArray()
   ```
   The above code was written in function `dispatchAnswer(questionItem)`
   After receving the content, you should transform its format to what we need. This operation can be found in the `switch` syntax`. In the following we list the answer format.

   | question style  |  answer format |
   | --------------- | ---------------|
   |      blank      |     ['16']     |
   |     select      |      [0]       |
   |   multi_blank   |   ['3', '4']   | 
   |   multi_select  |   [1, 2, 3]    |
   
   After transformation, you should dispatch the answer the its parent component `Answer.vue`.
   
   Here, the variable `status` indicates whether this question has been answered, in `Answer.vue`, we will use this sign to judge whether it's justifiable to move to next question.
