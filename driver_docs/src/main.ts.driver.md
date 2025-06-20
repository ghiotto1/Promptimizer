# Purpose
This JavaScript file is a module designed to facilitate the optimization of prompts using a genetic algorithm approach. It imports various components such as prompt models, database services, and utility functions to manage and manipulate data sets. The primary functionality revolves around creating, testing, and optimizing prompts through a series of generations, leveraging a population-based approach. The code defines several key functions: [`createPrompt`](#Promptimizer/src/maincreatePrompt) for initializing prompts, [`prepareData`](#Promptimizer/src/mainprepareData) for setting up training and test datasets, [`testOriginalPrompt`](#Promptimizer/src/maintestOriginalPrompt) for evaluating an existing prompt, and [`optimizePrompt`](#Promptimizer/src/mainoptimizePrompt) for iteratively improving prompts over multiple generations. The optimization process involves selecting parent prompts, generating offspring, applying mutations, and evaluating fitness scores to refine the prompt population.

The module is structured to be executed as a script, as evidenced by the immediately invoked asynchronous function at the end, which calls [`optimizePrompt`](#Promptimizer/src/mainoptimizePrompt) and exits the process upon completion. It interacts with a database to store and retrieve prompt data, ensuring persistence across runs. The code also includes functionality to generate summaries of the optimization process, capturing metrics such as average training and validation fitness scores for each generation. This module is designed to be a comprehensive tool for prompt optimization, providing both the infrastructure for data management and the logic for evolutionary improvement of prompts.
# Imports and Dependencies

---
- `./models/prompts/defaults`
- `./models/prompts/abstract`
- `./services/db`
- `./population`
- `fs`
- `fs/promises`
- `./inputs`
- `path`
- `./utils`


# Global Variables

---
### NUM\_GENERATIONS
- **Type**: `number`
- **Description**: `NUM_GENERATIONS` is a constant variable set to the value 50. It represents the number of generations that a genetic algorithm will iterate through during the optimization process.
- **Use**: This variable is used in a loop within the `optimizePrompt` function to control the number of iterations for generating and evolving populations.


# Interfaces

---
### OptimizationData<!-- {{#interface:Promptimizer/src/main.OptimizationData}} -->
- **Members**:
    - `trainingSet`: An array of folder names representing the training dataset.
    - `testSet`: An array of folder names representing the test dataset.
- **Description**: The `OptimizationData` interface defines a contract for objects that hold data necessary for optimization processes, specifically in the context of machine learning or data-driven tasks. It specifies that any object adhering to this interface must contain two properties: `trainingSet` and `testSet`, both of which are arrays of strings. These arrays represent folder names that are used to organize and access the training and testing datasets, respectively. This interface ensures that any implementation will have a consistent structure for handling datasets, facilitating processes such as data preparation, model training, and evaluation.


---
### GenerationSummary<!-- {{#interface:Promptimizer/src/main.GenerationSummary}} -->
- **Members**:
    - `generationNumber`: The generation number of the evolutionary process.
    - `averageTrainingFitness`: The average fitness score of the population during training for this generation.
    - `averageValidationFitness`: The average fitness score of the population during validation for this generation.
- **Description**: The `GenerationSummary` interface defines the structure for summarizing the results of a generation in an evolutionary algorithm process. It includes the generation number, the average training fitness, and the average validation fitness, providing a concise overview of the performance of a population at a specific generation.


# Functions

---
### createPrompt<!-- {{#callable:Promptimizer/src/main.createPrompt}} -->
The `createPrompt` function initializes and saves two `AbstractPrompt` instances using predefined prompt data, returning the second prompt.
- **Inputs**: None
- **Control Flow**:
    - Instantiate an `AbstractPrompt` object named `evaluator` using `evaluatorPrompt` data.
    - Call the `save` method on the `evaluator` object to persist it.
    - Instantiate another `AbstractPrompt` object named `prompt` using `bestPrompt` data.
    - Call the `save` method on the `prompt` object to persist it.
    - Return the `prompt` object.
- **Output**: The function returns the `AbstractPrompt` instance created with `bestPrompt` data.


---
### prepareData<!-- {{#callable:Promptimizer/src/main.prepareData}} -->
The `prepareData` function creates or retrieves training and test datasets for optimization, storing them in a specified directory.
- **Inputs**: None
- **Control Flow**:
    - Define the path for the optimization folder and check if it exists; create it if it doesn't.
    - Define paths for training and test set JSON files within the optimization folder.
    - Check if both training and test set files exist; if they do, read and parse them, then return the datasets.
    - If the files do not exist, shuffle the folder names from the inputs, split them into training and test sets, save these sets to their respective files, and return them.
- **Output**: An object containing `trainingSet` and `testSet`, both of which are arrays of folder names.
- **Functions called**:
    - [`Promptimizer/src/utils/index.shuffleArray`](utils/index.ts.driver.md#Promptimizer/src/utils/indexshuffleArray)


---
### testOriginalPrompt<!-- {{#callable:Promptimizer/src/main.testOriginalPrompt}} -->
The `testOriginalPrompt` function connects to a local database, prepares data, retrieves a specific prompt, creates a population with this prompt, and then tests, validates, and saves the population.
- **Inputs**: None
- **Control Flow**:
    - Instantiate a new database connection to a local database and connect to it.
    - Log a message indicating successful connection to the database.
    - Call [`prepareData`](#Promptimizer/src/mainprepareData) to retrieve training and test datasets.
    - Log a message indicating the search for a specific prompt by name.
    - Retrieve a prompt from the database using `AbstractPrompt.findOneByName` with the specified prompt name.
    - Create a new population using `Population.createPopulationWithPrompt` with the retrieved prompt and the prepared datasets.
    - Test the created population with a generation number of 0 using `originalPromptPpulation.test`.
    - Validate the created population with a generation number of 0 using `originalPromptPpulation.validate`.
    - Save the created population with a generation number of 0 using `originalPromptPpulation.save`.
- **Output**: The function does not return any value; it performs operations on the database and logs messages to the console.
- **Functions called**:
    - [`Promptimizer/src/main.prepareData`](#Promptimizer/src/mainprepareData)


---
### createPromptSummary<!-- {{#callable:Promptimizer/src/main.createPromptSummary}} -->
The `createPromptSummary` function generates a summary of generation data for a given prompt and writes it to a JSON file.
- **Inputs**:
    - `promptName`: The name of the prompt for which the summary is to be created.
- **Control Flow**:
    - Constructs the path to the run directory using the provided prompt name.
    - Initializes an empty array `summaries` to store generation summaries.
    - Attempts to read the contents of the run directory to get a list of generations.
    - Iterates over each generation directory, checking if it starts with 'generation_'.
    - For each valid generation, extracts the generation number and constructs the path to the 'population.json' file.
    - Attempts to read and parse the 'population.json' file to extract average training and validation fitness scores.
    - Appends a summary object containing the generation number and fitness scores to the `summaries` array.
    - Sorts the `summaries` array by generation number.
    - Writes the sorted summaries to a 'prompt_summary.json' file in the run directory.
    - Logs a success message indicating the summary creation path.
    - Catches and logs any errors encountered during directory reading, file reading, or summary creation.
- **Output**: The function does not return any value but writes a JSON file containing the summary of generations for the specified prompt.


---
### optimizePrompt<!-- {{#callable:Promptimizer/src/main.optimizePrompt}} -->
The `optimizePrompt` function optimizes a prompt through a genetic algorithm by iteratively evolving a population over multiple generations.
- **Inputs**: None
- **Control Flow**:
    - Connects to a local database using the `Db` class.
    - Prepares training and test data sets using the [`prepareData`](#Promptimizer/src/mainprepareData) function.
    - Attempts to find an existing prompt by name using `AbstractPrompt.findOneByName`; if not found, creates a new prompt using [`createPrompt`](#Promptimizer/src/maincreatePrompt).
    - Creates an initial population with random examples using `Population.createPopulationWithRandomExamples`.
    - Tests, validates, and saves the initial population.
    - Iterates over a specified number of generations (`NUM_GENERATIONS`), performing the following steps for each generation:
    - Selects parent pairs from the current population using `population.selectParents`.
    - Generates children from parent pairs using `population.generateChildren`.
    - Generates mutated children from the children using `population.generateMutatedChildren`.
    - Merges the current population with the children and mutated children using `Population.merge`.
    - Sorts and culls the new population to maintain a manageable size.
    - Updates the current population with the new population.
    - Calculates training fitness for the population and validates and saves it for the current generation.
- **Output**: The function does not return a value; it performs operations on the database and file system to optimize and save the prompt population.
- **Functions called**:
    - [`Promptimizer/src/main.prepareData`](#Promptimizer/src/mainprepareData)
    - [`Promptimizer/src/main.createPrompt`](#Promptimizer/src/maincreatePrompt)


