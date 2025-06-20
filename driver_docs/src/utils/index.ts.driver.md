# Purpose
This code file provides a collection of utility functions and a custom error class, primarily focused on handling JSON data, array manipulation, and promise management. The [`removeJson`](#Promptimizer/src/utils/indexremoveJson) function is designed to strip JSON content from a given text, while the [`extractJson`](#Promptimizer/src/utils/indexextractJson) function attempts to parse and return JSON data from a string, handling various non-JSON markers and sanitizing the input. The [`shuffleArray`](#Promptimizer/src/utils/indexshuffleArray) function implements the Fisher-Yates algorithm to randomly shuffle elements in an array, providing a straightforward utility for array manipulation.

Additionally, the file defines a custom error class, `PromiseAllWithErrorsError`, which is used in conjunction with the [`promiseAllWithErrors`](#Promptimizer/src/utils/indexpromiseAllWithErrors) function. This function extends the standard `Promise.all` behavior by allowing the collection of both fulfilled and rejected promises, throwing a custom error if any promises are rejected. The [`handlePromiseAllWithErrorsError`](#Promptimizer/src/utils/indexhandlePromiseAllWithErrorsError) function provides a mechanism to handle these custom errors, logging the errors and returning the successful results. Overall, the file serves as a utility module that can be imported and used in other parts of a codebase, offering specific functionalities related to JSON processing, array operations, and enhanced promise handling.
# Classes

---
### PromiseAllWithErrorsError<!-- {{#class:Promptimizer/src/utils/index.PromiseAllWithErrorsError}} -->
- **Members**:
    - `successes`: An array of successful results from the promises.
    - `errors`: An array of error messages from the rejected promises.
- **Description**: The `PromiseAllWithErrorsError` class extends the standard `Error` class to provide a mechanism for handling multiple promises where some may fail. It captures both the successful results and the error messages of the failed promises, allowing for more granular error handling when using `Promise.allSettled`. This class is particularly useful in scenarios where you want to process all promises and handle errors without stopping execution at the first failure.
- **Methods**:
    - [`Promptimizer/src/utils/index.PromiseAllWithErrorsError.constructor`](#PromiseAllWithErrorsErrorconstructor)
- **Extends/Implements**:
    - `Error`

**Methods**

---
#### PromiseAllWithErrorsError\.constructor<!-- {{#callable:Promptimizer/src/utils/index.PromiseAllWithErrorsError.constructor}} -->
The constructor initializes a new instance of the PromiseAllWithErrorsError class with provided successes and errors, setting up the error message and class properties.
- **Inputs**:
    - `successes`: An array of successful results of type T.
    - `errors`: An array of error messages as strings.
- **Control Flow**:
    - Calls the parent class (Error) constructor with a formatted error message created by joining the errors array with a semicolon separator.
    - Sets the name property of the error to 'PromiseAllWithErrorsError'.
    - Assigns the provided successes array to the instance's successes property.
    - Assigns the provided errors array to the instance's errors property.
- **Output**: An instance of the PromiseAllWithErrorsError class is created and initialized with the provided successes and errors.
- **See also**: [`Promptimizer/src/utils/index.PromiseAllWithErrorsError`](#PromiseAllWithErrorsError)  (Base Class)



# Functions

---
### removeJson<!-- {{#callable:Promptimizer/src/utils/index.removeJson}} -->
The `removeJson` function removes JSON content from a given text string, handling both code block markers and JSON braces, and returns the cleaned text.
- **Inputs**:
    - `text`: A string potentially containing JSON content to be removed.
- **Control Flow**:
    - Check if the input text is empty; if so, return it immediately.
    - Find the first occurrence of a code block marker (```), or if not found, the first JSON opening brace ({).
    - Find the last occurrence of a code block marker (```), or if not found, the last JSON closing brace (}).
    - If no valid JSON markers are found or the markers are in an invalid order, return the original text.
    - Remove the JSON content between the identified markers from the text.
    - Attempt to extract JSON using the [`extractJson`](#Promptimizer/src/utils/indexextractJson) function; if extraction fails, return the original text.
    - Remove extra new lines from the result and trim the text before returning.
- **Output**: The function returns the input text with JSON content removed, or the original text if no valid JSON is found or an error occurs.
- **Functions called**:
    - [`Promptimizer/src/utils/index.extractJson`](#Promptimizer/src/utils/indexextractJson)


---
### extractJson<!-- {{#callable:Promptimizer/src/utils/index.extractJson}} -->
The `extractJson` function extracts and parses JSON content from a given string, returning it as a JavaScript object or undefined if parsing fails.
- **Inputs**:
    - `text`: A string potentially containing JSON content to be extracted and parsed.
- **Control Flow**:
    - Check if the input text is empty; if so, return undefined.
    - Use a regular expression to search for JSON content within the text.
    - If no JSON content is found, return undefined.
    - Extract the JSON string from the matched content.
    - Remove known non-JSON markers and sanitize the string by replacing certain patterns and non-printable characters.
    - Attempt to parse the sanitized JSON string using `JSON.parse`.
    - If parsing is successful, return the parsed object; otherwise, return undefined.
- **Output**: A JavaScript object representing the parsed JSON content, or undefined if the input is empty, no JSON is found, or parsing fails.


---
### shuffleArray<!-- {{#callable:Promptimizer/src/utils/index.shuffleArray}} -->
The `shuffleArray` function randomly shuffles the elements of an array in place using the Fisher-Yates algorithm.
- **Inputs**:
    - `array`: An array of any type of elements that needs to be shuffled.
- **Control Flow**:
    - Iterate over the array from the last element to the second element.
    - For each element, generate a random index from 0 to the current index.
    - Swap the current element with the element at the random index.
- **Output**: The same array with its elements shuffled in a random order.


---
### handlePromiseAllWithErrorsError<!-- {{#callable:Promptimizer/src/utils/index.handlePromiseAllWithErrorsError}} -->
The function handles errors from a PromiseAllWithErrorsError by logging each error and returning the successful results, or rethrows the error if it's not of that type.
- **Inputs**:
    - `err`: An Error object that may be an instance of PromiseAllWithErrorsError.
- **Control Flow**:
    - Check if the error is an instance of PromiseAllWithErrorsError.
    - If true, iterate over the errors array in the error object and log each error to the console.
    - Return the successes array from the error object.
    - If the error is not an instance of PromiseAllWithErrorsError, rethrow the error.
- **Output**: An array of successful results if the error is a PromiseAllWithErrorsError, otherwise the function throws the error.


---
### promiseAllWithErrors<!-- {{#callable:Promptimizer/src/utils/index.promiseAllWithErrors}} -->
The function `promiseAllWithErrors` executes an array of promises and returns the results of the fulfilled promises, throwing an error if any promise is rejected.
- **Inputs**:
    - `promises`: An array of promises of type T to be executed.
- **Control Flow**:
    - The function uses `Promise.allSettled` to execute all promises and wait for their completion, regardless of whether they are fulfilled or rejected.
    - It initializes two arrays: `fulfilled` to store the values of successfully resolved promises, and `errors` to store the reasons for any rejected promises.
    - It iterates over the results of `Promise.allSettled`, pushing values to `fulfilled` if the promise was fulfilled, or pushing reasons to `errors` if the promise was rejected.
    - If there are any errors, it throws a `PromiseAllWithErrorsError` containing both the fulfilled results and the errors.
    - If there are no errors, it returns the array of fulfilled results.
- **Output**: An array of fulfilled promise results of type T, or throws a `PromiseAllWithErrorsError` if any promise is rejected.


