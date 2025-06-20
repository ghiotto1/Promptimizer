# Purpose
This JavaScript file is a script designed to automate the process of generating and validating SQL queries based on user inputs, with the goal of populating "ground truths" for a dataset. The script interacts with a database and a chat-based AI model to iteratively refine SQL queries until they are deemed correct. It imports several modules, including database management, chat controller services, and file system operations, to facilitate its operations. The script reads inputs, processes them through a chat-based AI model to generate SQL queries, executes these queries against a BigQuery database, and then prompts the user for manual validation of the query results. If the query is incorrect, the script allows for iterative refinement by capturing user feedback and updating the query accordingly.

The script is structured around an asynchronous function, [`populate_ground_truths`](#Promptimizer/src/populateGroundTruthpopulate_ground_truths), which handles the main logic of the process. It uses a combination of user interaction through the command line and automated query generation and execution to ensure the accuracy of the SQL queries. The script also manages file operations to store input and output data, ensuring that results are saved for future reference. This script is not intended to be a reusable module but rather a standalone executable that performs a specific task of generating and validating SQL queries, making it a specialized tool for data preparation and validation tasks.
# Imports and Dependencies

---
- `./models/prompts/abstract`
- `./services/bigQueryClient`
- `./services/db`
- `./models/prompts/defaults`
- `./services/chatController`
- `fs`
- `./inputs`
- `readline`


# Functions

---
### populate\_ground\_truths<!-- {{#callable:Promptimizer/src/populateGroundTruth.populate_ground_truths}} -->
The `populate_ground_truths` function processes a list of inputs to generate and verify SQL queries, storing the results in a specified directory.
- **Inputs**: None
- **Control Flow**:
    - Initialize a database connection and a readline interface for user input.
    - Define helper functions for asking questions, checking affirmative/negative responses, and updating messages based on errors.
    - Iterate over each input, checking if the output file already exists to skip processing if it does.
    - For each input, generate an initial message using a chat controller and store it in a messages array.
    - Enter a loop to verify the correctness of the SQL query generated from the message, executing it against a BigQuery instance.
    - If the query execution is successful, prompt the user for manual review to confirm correctness.
    - If the query is incorrect or an error occurs, update the message with user feedback or error details and retry up to a maximum number of retries.
    - Once a query is verified as correct, save the input and output to respective files in the specified directory.
    - Close the readline interface after processing all inputs.
- **Output**: The function does not return a value; it writes input and output data to files in a directory structure based on the input's folder name.
- **Functions called**:
    - [`Promptimizer/src/populateGroundTruth.askQuestion`](#Promptimizer/src/populateGroundTruthaskQuestion)
    - [`Promptimizer/src/populateGroundTruth.isAffirmative`](#Promptimizer/src/populateGroundTruthisAffirmative)
    - [`Promptimizer/src/populateGroundTruth.isNegative`](#Promptimizer/src/populateGroundTruthisNegative)
    - [`Promptimizer/src/populateGroundTruth.getUpdatedMessage`](#Promptimizer/src/populateGroundTruthgetUpdatedMessage)


---
### askQuestion<!-- {{#callable:Promptimizer/src/populateGroundTruth.askQuestion}} -->
The `askQuestion` function prompts the user with a question, processes the input by removing punctuation and converting it to lowercase, and returns the cleaned response as a promise.
- **Inputs**:
    - `question`: A string representing the question to be asked to the user.
- **Control Flow**:
    - The function returns a new Promise that resolves when the readline interface receives an answer from the user.
    - The readline interface prompts the user with the provided question.
    - Upon receiving an answer, the function removes punctuation from the answer using a regular expression, trims whitespace, and converts the text to lowercase.
    - The cleaned answer is then resolved as the result of the promise.
- **Output**: A promise that resolves to a string, which is the user's input cleaned of punctuation and converted to lowercase.


---
### isAffirmative<!-- {{#callable:Promptimizer/src/populateGroundTruth.isAffirmative}} -->
The `isAffirmative` function checks if a given string response is an affirmative answer by comparing it against a predefined list of affirmative words.
- **Inputs**:
    - `response`: A string representing the user's response to be checked for affirmation.
- **Control Flow**:
    - Define a constant array `affirmatives` containing strings: 'yes', 'yeah', 'ya', 'yea'.
    - Check if the `response` is included in the `affirmatives` array using the `includes` method.
    - Return the result of the `includes` method, which is a boolean indicating if the response is affirmative.
- **Output**: A boolean value indicating whether the response is considered affirmative (true) or not (false).


---
### isNegative<!-- {{#callable:Promptimizer/src/populateGroundTruth.isNegative}} -->
The `isNegative` function checks if a given string response is a negative response by comparing it against a predefined list of negative words.
- **Inputs**:
    - `response`: A string input representing the user's response to be checked for negativity.
- **Control Flow**:
    - Define an array `negatives` containing the strings 'no', 'nah', and 'nope'.
    - Check if the `response` is included in the `negatives` array using the `includes` method.
    - Return the result of the `includes` method, which is a boolean indicating if the response is negative.
- **Output**: A boolean value indicating whether the input response is considered negative.


---
### getUpdatedMessage<!-- {{#callable:Promptimizer/src/populateGroundTruth.getUpdatedMessage}} -->
The `getUpdatedMessage` function generates an updated chat message by appending user feedback and error descriptions to a message history and querying a chat controller for a response.
- **Inputs**:
    - `messages`: An array of message objects representing the conversation history.
    - `errorMessage`: A string containing the error message or description to be addressed in the updated message.
- **Control Flow**:
    - Retrieve a prompt object by calling `AbstractPrompt.findOneByName` with a predefined prompt name.
    - Invoke the `chat` method of `chatController` with the retrieved prompt, an extended message array including the latest message and error description, and an undefined third argument.
    - Return the result of the `chat` method call.
- **Output**: The function returns a promise that resolves to the response from the `chatController.chat` method, which is an updated message object.


