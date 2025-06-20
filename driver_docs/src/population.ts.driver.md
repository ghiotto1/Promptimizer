# Purpose
This code defines a module that implements a genetic algorithm framework for optimizing AI prompts, specifically for use with OpenAI models. The module is structured around two main classes: `Individual` and `Population`. The `Individual` class represents a single AI prompt configuration, encapsulating its prompt, training fitness, and validation fitness. It provides methods for creating new instances, randomizing examples, comparing results, testing against datasets, and performing crossover operations to generate new prompts. The `Population` class manages a collection of `Individual` instances, representing a group of potential solutions. It includes methods for generating random examples, loading examples from a training set, creating populations, merging populations, sorting, culling, selecting parents for crossover, generating children, and saving the population state.

The module is designed to be part of a larger system, as indicated by its imports from various utility and service modules, such as `chatController` for interacting with chat models and `BigQueryDataManager` for executing SQL queries. The code is structured to facilitate the evolution of AI prompts through genetic operations like crossover and mutation, with the goal of improving their performance on specified training and validation datasets. The `Population` class also includes methods for evaluating the fitness of individuals within the population, both in terms of training and validation scores, which are crucial for guiding the evolutionary process. The module is intended to be imported and used within a larger application, as evidenced by its export of the `Population` class as the default export.
# Imports and Dependencies

---
- `./models/prompts/abstract`
- `./utils`
- `./services/bigQueryClient`
- `./models/message`
- `./models/prompts/defaults`
- `./models/prompts/example`
- `lodash`
- `./additionalSystemPrompts`
- `./services/chatController`
- `fs`
- `path`


# Global Variables

---
### BATCH\_SIZE
- **Type**: ``number``
- **Description**: `BATCH_SIZE` is a constant variable set to the value of 1. It is used to determine the number of individuals processed in a single batch during operations such as testing and validation within the `Population` class.
- **Use**: This variable is used to control the batch size for processing individuals in the `Population` class methods like `test` and `validate`.


---
### MIN\_EXAMPLES
- **Type**: `number`
- **Description**: The `MIN_EXAMPLES` variable is a constant that holds the minimum number of examples that should be used in certain operations, such as generating random examples for a population. It is set to the value of 4.
- **Use**: This variable is used to ensure that at least 4 examples are included when generating random examples for a population.


---
### MAX\_EXAMPLES
- **Type**: `number`
- **Description**: The `MAX_EXAMPLES` variable is a constant that defines the maximum number of examples that can be used in certain operations or processes within the code. It is set to the value of 12, indicating that no more than 12 examples should be utilized at any given time.
- **Use**: This variable is used to limit the number of examples that can be randomly generated or processed, ensuring that operations do not exceed this predefined maximum.


---
### MIN\_CONVERSATIONS\_PER\_EXAMPLE
- **Type**: `number`
- **Description**: The `MIN_CONVERSATIONS_PER_EXAMPLE` variable is a constant that defines the minimum number of conversations that should be included in each example. It is set to a value of 1, indicating that each example must have at least one conversation.
- **Use**: This variable is used to ensure that each example generated or processed contains at least one conversation, as part of the logic for creating or validating examples.


---
### MAX\_CONVERSATIONS\_PER\_EXAMPLE
- **Type**: `number`
- **Description**: The `MAX_CONVERSATIONS_PER_EXAMPLE` variable is a constant that defines the maximum number of conversations that can be included in a single example. It is set to the value of 3, indicating that each example can contain up to three conversations.
- **Use**: This variable is used to limit the number of conversations when generating random examples in the `Population` class.


# Classes

---
### Individual<!-- {{#class:Promptimizer/src/population.Individual}} -->
- **Members**:
    - `trainingFitness`: Represents the fitness score of the individual during training.
    - `validationFitness`: Represents the fitness score of the individual during validation.
    - `prompt`: Holds the prompt associated with the individual.
- **Description**: The `Individual` class represents an entity in a genetic algorithm-like system, where each individual is associated with a prompt and has fitness scores for training and validation. It provides functionality to create new individuals, randomize examples from parent individuals, and evaluate the fitness of the individual based on test datasets. The class also supports crossover operations to generate new individuals by combining prompts from two parent individuals.
- **Methods**:
    - [`Promptimizer/src/population.Individual.constructor`](#Individualconstructor)
    - [`Promptimizer/src/population.Individual.create`](#Individualcreate)
    - [`Promptimizer/src/population.Individual.randomizeExamples`](#IndividualrandomizeExamples)
    - [`Promptimizer/src/population.Individual.compareResults`](#IndividualcompareResults)
    - [`Promptimizer/src/population.Individual.test`](#Individualtest)
    - [`Promptimizer/src/population.Individual.crossover`](#Individualcrossover)
    - [`Promptimizer/src/population.Individual.resemblenceContent`](#IndividualresemblenceContent)

**Methods**

---
#### Individual\.constructor<!-- {{#callable:Promptimizer/src/population.Individual.constructor}} -->
The constructor initializes an Individual instance with a given prompt and optional fitness values.
- **Decorators**: `@private`
- **Inputs**:
    - `prompt`: An instance of AbstractPrompt that represents the prompt associated with the Individual.
    - `trainingFitness`: A number representing the training fitness of the Individual, defaulting to 0.
    - `validationFitness`: A number representing the validation fitness of the Individual, defaulting to 0.
- **Control Flow**:
    - Assigns the provided prompt to the instance's prompt property.
    - Sets the instance's trainingFitness property to the provided trainingFitness value.
    - Sets the instance's validationFitness property to the provided validationFitness value.
- **Output**: An instance of the Individual class is created and initialized with the specified properties.
- **See also**: [`Promptimizer/src/population.Individual`](#Individual)  (Base Class)


---
#### Individual\.create<!-- {{#callable:Promptimizer/src/population.Individual.create}} -->
The `create` method instantiates a new `Individual` object using a given `AbstractPrompt`.
- **Inputs**:
    - `prompt`: An instance of `AbstractPrompt` that is used to initialize the `Individual`.
- **Control Flow**:
    - The method receives an `AbstractPrompt` as an argument.
    - It creates a new `AbstractPrompt` instance using the provided `prompt`.
    - A new `Individual` object is instantiated with the newly created `AbstractPrompt`.
    - The newly created `Individual` object is returned.
- **Output**: Returns a new `Individual` object initialized with the provided `AbstractPrompt`.
- **See also**: [`Promptimizer/src/population.Individual`](#Individual)  (Base Class)


---
#### Individual\.randomizeExamples<!-- {{#callable:Promptimizer/src/population.Individual.randomizeExamples}} -->
The `randomizeExamples` method randomly selects examples from two arrays of `PromptExample` objects, combining them into a single array.
- **Inputs**:
    - `fatherExamples`: An array of `PromptExample` objects representing the 'father' examples.
    - `motherExamples`: An array of `PromptExample` objects representing the 'mother' examples.
- **Control Flow**:
    - Determine the maximum length between `fatherExamples` and `motherExamples`.
    - Initialize an empty array `examples` to store the selected examples.
    - Iterate over the range from 0 to `maxLength`.
    - For each index, randomly select either the `fatherExample` or `motherExample` at that index.
    - If the selected example is undefined (index out of bounds), continue to the next iteration.
    - If the selected example is defined, add it to the `examples` array.
- **Output**: An array of `PromptExample` objects, randomly selected from the input arrays.
- **See also**: [`Promptimizer/src/population.Individual`](#Individual)  (Base Class)


---
#### Individual\.compareResults<!-- {{#callable:Promptimizer/src/population.Individual.compareResults}} -->
The `compareResults` method evaluates the similarity between expected and actual responses using a chat-based evaluation model and returns an explanation and score.
- **Inputs**:
    - `input`: The input string that was used to generate the actual response.
    - `expectedResponse`: The expected response string that the actual response is compared against.
    - `actualResponse`: The actual response string generated that needs to be evaluated.
    - `expectedJson`: An array of expected JSON objects that represent the expected data structure.
    - `actualJson`: An array of actual JSON objects that represent the actual data structure.
- **Control Flow**:
    - Retrieve the evaluator prompt using `AbstractPrompt.findOneByName` with a predefined prompt name.
    - Construct an evaluation input string that includes the input, expected response, actual response, and JSON snippets of both expected and actual data.
    - Invoke the `chatController.chat` method with the evaluator prompt and evaluation input to get a response from the chat model.
    - Extract the `explanation` and `score` from the response data.
    - Check if `score` or `explanation` is `nil` using lodash's `_.isNil` function, log an error, and throw an error if they are.
    - Return an object containing the `explanation` and `score`.
- **Output**: An object containing an `explanation` string and a `score` number that evaluates the comparison between expected and actual results.
- **See also**: [`Promptimizer/src/population.Individual`](#Individual)  (Base Class)


---
#### Individual\.test<!-- {{#callable:Promptimizer/src/population.Individual.test}} -->
The `test` method evaluates a dataset of test cases by executing queries and comparing results to calculate a total score for a given generation.
- **Inputs**:
    - `dataset`: An array of strings representing folder names containing test case data.
    - `currentGeneration`: A number representing the current generation for which the test is being conducted.
- **Control Flow**:
    - Iterates over each folder in the dataset to create promises for processing each test case.
    - For each folder, constructs paths to input and output files and reads their contents asynchronously.
    - Checks if test results already exist for the current generation; if so, reads and returns the existing score.
    - If no existing results, sends the input to a chat controller to generate a SQL query and executes it using a BigQuery client.
    - Executes the expected SQL query derived from the expected output and compares the actual and expected results using the `compareResults` method.
    - Collects scores from all test cases and sums them up to return the total score.
- **Output**: Returns a Promise that resolves to a number representing the total score of all test cases for the current generation.
- **Functions called**:
    - [`Promptimizer/src/utils/index.extractJson`](utils/index.ts.driver.md#Promptimizer/src/utils/indexextractJson)
    - [`Promptimizer/src/utils/index.promiseAllWithErrors`](utils/index.ts.driver.md#Promptimizer/src/utils/indexpromiseAllWithErrors)
- **See also**: [`Promptimizer/src/population.Individual`](#Individual)  (Base Class)


---
#### Individual\.crossover<!-- {{#callable:Promptimizer/src/population.Individual.crossover}} -->
The `crossover` method generates a new `Individual` by combining prompts from two parent `Individuals` based on a specified resemblance and randomizes examples for the new prompt.
- **Inputs**:
    - `individual`: An `Individual` object representing the other parent in the crossover process.
    - `resemblence`: A `Resemblence` enum value indicating whether the child should resemble the father, mother, or be an equal combination of both.
- **Control Flow**:
    - Retrieve the 'Prompt Crossover' prompt using `AbstractPrompt.findOneByName`.
    - Define a helper function [`resemblenceContent`](#IndividualresemblenceContent) to determine the resemblance description based on the `resemblence` parameter.
    - Create a `messages` array containing JSON stringified prompts from both parents and a resemblance description.
    - Log the creation of a child with the specified resemblance.
    - Invoke `chatController.chat` with the crossover prompt and messages to generate a response, handling any errors by returning an error message.
    - Log the creation of the child and proceed to randomize examples using `randomizeExamples` method.
    - Create a new `AbstractPrompt` with the response content and randomized examples.
    - Log the completion of example randomization.
    - Return a new `Individual` created with the new prompt.
- **Output**: A new `Individual` object created with a prompt that is a combination of the parent prompts and randomized examples.
- **Functions called**:
    - [`Promptimizer/src/population.Individual.resemblenceContent`](#IndividualresemblenceContent)
- **See also**: [`Promptimizer/src/population.Individual`](#Individual)  (Base Class)


---
#### Individual\.resemblenceContent<!-- {{#callable:Promptimizer/src/population.Individual.resemblenceContent}} -->
The `resemblenceContent` function returns a string describing how a child prompt should resemble its parent prompts based on the specified resemblance type.
- **Inputs**: None
- **Control Flow**:
    - Check if the `resemblence` variable is equal to `Resemblence.Father` and return a string indicating the child prompt should resemble the father prompt more closely.
    - Check if the `resemblence` variable is equal to `Resemblence.Mother` and return a string indicating the child prompt should resemble the mother prompt more closely.
    - If neither condition is met, return a string indicating the child prompt should be an equal combination of both parent prompts.
- **Output**: A string describing the resemblance of the child prompt to its parent prompts based on the `resemblence` type.
- **See also**: [`Promptimizer/src/population.Individual`](#Individual)  (Base Class)



---
### Population<!-- {{#class:Promptimizer/src/population.Population}} -->
- **Members**:
    - `individuals`: An array of Individual objects representing the members of the population.
    - `trainingSet`: An array of strings representing the training dataset.
    - `validationSet`: An array of strings representing the validation dataset.
    - `initialPopulationSize`: The initial size of the population.
    - `originalPrompt`: An AbstractPrompt object representing the original prompt used to create the population.
    - `averageTrainingScore`: The average training score of the population.
    - `averageValidationScore`: The average validation score of the population.
- **Description**: The Population class manages a collection of Individual objects, each representing a potential solution or member of the population. It provides functionality to create, sort, cull, and evolve the population through methods like generating children, merging populations, and testing individuals against training and validation datasets. The class also supports loading examples from datasets and saving the population state to the filesystem. It is designed to facilitate evolutionary algorithms by managing the lifecycle and evaluation of a population of solutions.
- **Methods**:
    - [`Promptimizer/src/population.Population.constructor`](#Populationconstructor)
    - [`Promptimizer/src/population.Population.generateRandomExamples`](#PopulationgenerateRandomExamples)
    - [`Promptimizer/src/population.Population.loadExamplesFromTrainingSet`](#PopulationloadExamplesFromTrainingSet)
    - [`Promptimizer/src/population.Population.createPopulationWithRandomExamples`](#PopulationcreatePopulationWithRandomExamples)
    - [`Promptimizer/src/population.Population.createPopulationWithPrompt`](#PopulationcreatePopulationWithPrompt)
    - [`Promptimizer/src/population.Population.create`](#Populationcreate)
    - [`Promptimizer/src/population.Population.merge`](#Populationmerge)
    - [`Promptimizer/src/population.Population.sort`](#Populationsort)
    - [`Promptimizer/src/population.Population.cull`](#Populationcull)
    - [`Promptimizer/src/population.Population.selectParents`](#PopulationselectParents)
    - [`Promptimizer/src/population.Population.selectIndividual`](#PopulationselectIndividual)
    - [`Promptimizer/src/population.Population.generateChildren`](#PopulationgenerateChildren)
    - [`Promptimizer/src/population.Population.generateMutatedChildren`](#PopulationgenerateMutatedChildren)
    - [`Promptimizer/src/population.Population.save`](#Populationsave)
    - [`Promptimizer/src/population.Population.update`](#Populationupdate)
    - [`Promptimizer/src/population.Population.calculateTrainingFitness`](#PopulationcalculateTrainingFitness)
    - [`Promptimizer/src/population.Population.test`](#Populationtest)
    - [`Promptimizer/src/population.Population.validate`](#Populationvalidate)

**Methods**

---
#### Population\.constructor<!-- {{#callable:Promptimizer/src/population.Population.constructor}} -->
The constructor initializes a Population instance with specified parameters and sets initial scores to zero.
- **Inputs**:
    - `originalPrompt`: An instance of AbstractPrompt representing the original prompt for the population.
    - `individuals`: An array of Individual objects representing the members of the population.
    - `trainingSet`: An array of strings representing the training dataset.
    - `validationSet`: An array of strings representing the validation dataset.
    - `initialPopulationSize`: A number indicating the initial size of the population.
- **Control Flow**:
    - Assigns a new AbstractPrompt instance to this.originalPrompt using the provided originalPrompt.
    - Sets this.individuals to the provided array of Individual objects.
    - Assigns the provided trainingSet array to this.trainingSet.
    - Assigns the provided validationSet array to this.validationSet.
    - Sets this.initialPopulationSize to the provided initialPopulationSize value.
    - Initializes this.averageTrainingScore to 0.
    - Initializes this.averageValidationScore to 0.
- **Output**: There is no return value as this is a constructor for initializing a Population instance.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.generateRandomExamples<!-- {{#callable:Promptimizer/src/population.Population.generateRandomExamples}} -->
The `generateRandomExamples` method generates a random set of `PromptExample` instances by combining messages from a given list of examples.
- **Inputs**:
    - `allExamples`: An array of `PromptExample` objects from which random examples will be generated.
- **Control Flow**:
    - Determine a random number of examples to generate, between `MIN_EXAMPLES` and `MAX_EXAMPLES`.
    - Initialize an empty array `examples` to store the generated examples.
    - For each example to be generated, determine a random number of conversations to include, between `MIN_CONVERSATIONS_PER_EXAMPLE` and `MAX_CONVERSATIONS_PER_EXAMPLE`.
    - For each conversation, select a random example from `allExamples` and clone its messages.
    - Combine the cloned messages into a new `PromptExample` and add it to the `examples` array.
    - Return the array of generated `PromptExample` instances.
- **Output**: An array of `PromptExample` objects, each containing a random combination of messages from the input examples.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.loadExamplesFromTrainingSet<!-- {{#callable:Promptimizer/src/population.Population.loadExamplesFromTrainingSet}} -->
The `loadExamplesFromTrainingSet` method loads prompt examples from a specified training set by reading input and output files, creating chat messages, and returning a list of `PromptExample` instances.
- **Inputs**:
    - `prompt`: An instance of `AbstractPrompt` whose examples will be loaded and updated.
    - `trainingSet`: An array of strings representing the names of training examples to be loaded.
- **Control Flow**:
    - Initialize `allExamples` with a copy of the current examples from the `prompt` and clear the `prompt` examples.
    - Iterate over each `trainingExample` in the `trainingSet`.
    - For each `trainingExample`, construct the file paths for the input and output text files.
    - Read the content of the input and output files as strings.
    - Create a `ChatMessage` array with the input as a user message and the output as an assistant message.
    - Instantiate a `PromptExample` with the created messages and add it to `allExamples`.
- **Output**: Returns an array of `PromptExample` instances containing the loaded examples from the training set.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.createPopulationWithRandomExamples<!-- {{#callable:Promptimizer/src/population.Population.createPopulationWithRandomExamples}} -->
The `createPopulationWithRandomExamples` method generates a new population of individuals based on a given prompt, training set, and validation set, either by loading from existing data or creating new individuals with random examples.
- **Inputs**:
    - `prompt`: An instance of AbstractPrompt that serves as the base prompt for generating the population.
    - `trainingSet`: An array of strings representing the training data set used to generate examples.
    - `validationSet`: An array of strings representing the validation data set used to evaluate the population.
- **Control Flow**:
    - Check if a directory for the prompt exists and contains any generations.
    - If generations exist, load the latest generation's population from a JSON file.
    - If no valid population is found, load examples from the training set.
    - Iterate over system prompts, generating random examples and creating new individuals.
    - Create a new Population instance with the generated individuals and sort it.
- **Output**: A new Population instance containing individuals generated from the prompt and examples, sorted by training fitness.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.createPopulationWithPrompt<!-- {{#callable:Promptimizer/src/population.Population.createPopulationWithPrompt}} -->
The `createPopulationWithPrompt` method creates a new `Population` instance using a given prompt and datasets for training and validation.
- **Inputs**:
    - `prompt`: An instance of `AbstractPrompt` that serves as the basis for creating the population.
    - `trainingSet`: An array of strings representing the training dataset.
    - `validationSet`: An array of strings representing the validation dataset.
- **Control Flow**:
    - Create an array `individuals` containing a single `Individual` created from the provided `prompt`.
    - Call the `Population.create` method with the `prompt`, `individuals`, `trainingSet`, and `validationSet` to create a new `Population` instance.
    - Sort the newly created `Population` instance using its `sort` method.
    - Return the sorted `Population` instance.
- **Output**: A sorted `Population` instance containing the initial individual created from the provided prompt.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.create<!-- {{#callable:Promptimizer/src/population.Population.create}} -->
The `create` method instantiates a new `Population` object using the provided prompt, individuals, training set, and validation set.
- **Inputs**:
    - `originalPrompt`: An instance of `AbstractPrompt` representing the original prompt for the population.
    - `individuals`: An array of `Individual` objects that make up the population.
    - `trainingSet`: An array of strings representing the training dataset for the population.
    - `validationSet`: An array of strings representing the validation dataset for the population.
- **Control Flow**:
    - A new `Population` object is created by calling its constructor with the provided `originalPrompt`, `individuals`, `trainingSet`, `validationSet`, and the length of the `individuals` array as the initial population size.
    - The newly created `Population` object is returned.
- **Output**: A `Population` object initialized with the provided parameters.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.merge<!-- {{#callable:Promptimizer/src/population.Population.merge}} -->
The `merge` method combines multiple `Population` instances into a single `Population` instance by aggregating their individuals and retaining the properties of the first population in the list.
- **Inputs**:
    - `populations`: An array of `Population` instances to be merged.
- **Control Flow**:
    - Check if the `populations` array is empty and throw an error if it is.
    - Create a new `Population` instance using the original prompt, training set, validation set, and initial population size from the first population in the array.
    - Aggregate all individuals from the provided populations into a single array using `_.cloneDeep` and `flat`.
    - Return the newly created `Population` instance.
- **Output**: A new `Population` instance that contains all individuals from the input populations and retains the properties of the first population in the list.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.sort<!-- {{#callable:Promptimizer/src/population.Population.sort}} -->
The `sort` method sorts the individuals in a Population instance by their training fitness in descending order.
- **Inputs**: None
- **Control Flow**:
    - The method accesses the `individuals` array of the Population instance.
    - It sorts the `individuals` array using the `sort` method, comparing the `trainingFitness` of each `Individual` object in descending order.
    - The method returns the `Population` instance itself after sorting.
- **Output**: The method returns the `Population` instance with its `individuals` array sorted by training fitness in descending order.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.cull<!-- {{#callable:Promptimizer/src/population.Population.cull}} -->
The `cull` method reduces the population size to its initial size by removing excess individuals.
- **Inputs**: None
- **Control Flow**:
    - Log the current population size before culling.
    - Check if the current number of individuals exceeds the initial population size.
    - If it does, slice the individuals array to retain only up to the initial population size.
    - Log the population size after culling.
- **Output**: Returns the modified Population instance with the culled individuals.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.selectParents<!-- {{#callable:Promptimizer/src/population.Population.selectParents}} -->
The `selectParents` method selects pairs of parent indices from a population based on their fitness for genetic algorithm operations.
- **Inputs**:
    - `population`: An instance of the Population class containing individuals with fitness values.
- **Control Flow**:
    - Calculate the total fitness of all individuals in the population.
    - Compute cumulative fitness values for the population.
    - Define a helper function [`selectIndividual`](#PopulationselectIndividual) to select an individual index based on a random fitness value.
    - Iterate to select pairs of parents, ensuring they are different, using random fitness values and the [`selectIndividual`](#PopulationselectIndividual) function.
    - If unable to select a different parent after 10,000 iterations, log fitness values and throw an error.
    - Store the selected parent pairs in an array.
- **Output**: An array of tuples, each containing two indices representing selected parent pairs.
- **Functions called**:
    - [`Promptimizer/src/population.Population.selectIndividual`](#PopulationselectIndividual)
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.selectIndividual<!-- {{#callable:Promptimizer/src/population.Population.selectIndividual}} -->
The `selectIndividual` function selects an index from a cumulative fitness array based on a random number input.
- **Inputs**:
    - `rand`: A random number used to select an index from the cumulative fitness array.
- **Control Flow**:
    - Iterates over the cumulativeFitness array.
    - Checks if the input random number is less than the current cumulative fitness value.
    - Returns the current index if the condition is met.
    - If no index is found, returns the last index of the cumulativeFitness array.
- **Output**: The index of the selected individual from the cumulative fitness array.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.generateChildren<!-- {{#callable:Promptimizer/src/population.Population.generateChildren}} -->
The `generateChildren` method creates a new generation of individuals by performing crossover operations on parent pairs and evaluating their fitness.
- **Inputs**:
    - `parentPairs`: An array of tuples, where each tuple contains two indices representing parent individuals in the current population.
    - `currentGeneration`: The current generation number, used for testing the fitness of the new individuals.
- **Control Flow**:
    - Initialize an empty array `childrenPromises` to store promises for generating children.
    - Iterate over each pair of parents in `parentPairs`.
    - For each pair, perform three crossover operations with different resemblances: None, Father, and Mother.
    - For each crossover result, test the child's fitness using the `test` method and store the fitness score in `trainingFitness`.
    - Push the resulting promises for each child into the `childrenPromises` array.
    - Log the number of children being generated.
    - Use [`promiseAllWithErrors`](utils/index.ts.driver.md#Promptimizer/src/utils/indexpromiseAllWithErrors) to resolve all promises in `childrenPromises`, handling any errors with [`handlePromiseAllWithErrorsError`](utils/index.ts.driver.md#Promptimizer/src/utils/indexhandlePromiseAllWithErrorsError).
    - Log the number of successfully generated children.
    - Create a new `Population` instance with the generated children and return it.
- **Output**: A new `Population` instance containing the generated children with updated fitness scores.
- **Functions called**:
    - [`Promptimizer/src/utils/index.promiseAllWithErrors`](utils/index.ts.driver.md#Promptimizer/src/utils/indexpromiseAllWithErrors)
    - [`Promptimizer/src/utils/index.handlePromiseAllWithErrorsError`](utils/index.ts.driver.md#Promptimizer/src/utils/indexhandlePromiseAllWithErrorsError)
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.generateMutatedChildren<!-- {{#callable:Promptimizer/src/population.Population.generateMutatedChildren}} -->
The `generateMutatedChildren` method creates a new population by selecting two random individuals from the given population, mutating their examples, testing their fitness, and returning a new population with these mutated individuals.
- **Inputs**:
    - `population`: An instance of the Population class representing the current population of individuals.
- **Control Flow**:
    - Select two random indices from the population's individuals.
    - Clone the individuals at the selected indices to create two children.
    - Load examples from the training set for each child's prompt.
    - Generate random examples for each child's prompt using the loaded examples.
    - Map over the children to test their fitness asynchronously and update their training fitness.
    - Use [`promiseAllWithErrors`](utils/index.ts.driver.md#Promptimizer/src/utils/indexpromiseAllWithErrors) to handle the asynchronous operations and catch any errors.
    - Create a new Population instance with the mutated children and return it.
- **Output**: A Promise that resolves to a new Population instance containing the mutated children.
- **Functions called**:
    - [`Promptimizer/src/utils/index.promiseAllWithErrors`](utils/index.ts.driver.md#Promptimizer/src/utils/indexpromiseAllWithErrors)
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.save<!-- {{#callable:Promptimizer/src/population.Population.save}} -->
The `save` method saves the current population to the filesystem, organizing it by generation number and including the prompts and their scores.
- **Inputs**:
    - `generation`: The generation number used to organize the saved population data.
- **Control Flow**:
    - Log the number of individuals in the population and the generation number.
    - Define the base directory for saving the population data.
    - Define the directory paths for the prompt and generation based on the base directory and generation number.
    - Check if the base, prompt, and generation directories exist, and create them if they do not.
    - Define the path for the population JSON file within the generation directory.
    - Write the current population data to the population JSON file.
    - Return the current population instance.
- **Output**: The method returns the current instance of the `Population` class.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.update<!-- {{#callable:Promptimizer/src/population.Population.update}} -->
The `update` method replaces the current population's individuals with a deep copy of the individuals from a new population.
- **Inputs**:
    - `newPopulation`: An instance of the Population class containing the new set of individuals to update the current population with.
- **Control Flow**:
    - The method uses lodash's `cloneDeep` function to create a deep copy of the `individuals` array from the `newPopulation` argument.
    - The deep-copied array is then assigned to the `individuals` property of the current Population instance.
- **Output**: The method does not return any value; it updates the `individuals` property of the current Population instance in place.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.calculateTrainingFitness<!-- {{#callable:Promptimizer/src/population.Population.calculateTrainingFitness}} -->
The `calculateTrainingFitness` method computes the average training fitness score for all individuals in the population.
- **Inputs**: None
- **Control Flow**:
    - The method initializes the `averageTrainingScore` by summing up the `trainingFitness` of each individual in the `individuals` array using the `reduce` function.
    - It then divides the total sum by the number of individuals to calculate the average training score.
- **Output**: The method updates the `averageTrainingScore` property of the `Population` instance with the computed average training fitness score.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.test<!-- {{#callable:Promptimizer/src/population.Population.test}} -->
The `test` method evaluates the training performance of each individual in the population for a given generation and calculates the average training score.
- **Decorators**: `@async`
- **Inputs**:
    - `currentGeneration`: The current generation number for which the individuals are being tested.
- **Control Flow**:
    - Log the number of individuals in the population.
    - Initialize `averageTrainingScore` to 0.
    - Iterate over the individuals in batches defined by `BATCH_SIZE`.
    - For each batch, asynchronously test each individual using their `test` method with the `trainingSet` and `currentGeneration`.
    - Log the scores obtained for the batch.
    - For each individual in the batch, update their `trainingFitness` with the score obtained and add the score to `averageTrainingScore`.
    - Calculate the average training score by dividing `averageTrainingScore` by the total number of individuals.
    - Log the average training score.
- **Output**: The method does not return any value; it updates the `trainingFitness` of each individual and the `averageTrainingScore` of the population.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)


---
#### Population\.validate<!-- {{#callable:Promptimizer/src/population.Population.validate}} -->
The `validate` method evaluates the validation fitness of each individual in the population and calculates the average validation score.
- **Decorators**: `@async`
- **Inputs**:
    - `currentGeneration`: The current generation number used for testing individuals.
- **Control Flow**:
    - Log the number of individuals in the population.
    - Initialize `averageValidationScore` to 0.
    - Iterate over individuals in batches of size `BATCH_SIZE`.
    - For each batch, test individuals using the `validationSet` and `currentGeneration`, and await their validation scores.
    - Log the validation scores for the batch.
    - For each individual in the batch, assign the validation score to `validationFitness` and add it to `averageValidationScore`.
    - Calculate the average validation score by dividing `averageValidationScore` by the total number of individuals.
    - Log the average validation score.
- **Output**: The method does not return any value; it updates the `validationFitness` of each individual and the `averageValidationScore` of the population.
- **See also**: [`Promptimizer/src/population.Population`](#Population)  (Base Class)



# Interfaces

---
### TestResult<!-- {{#interface:Promptimizer/src/population.TestResult}} -->
- **Members**:
    - `input`: A string representing the input data for the test.
    - `expectedOutput`: A string representing the expected output for the test.
    - `expectedJSON`: An object representing the expected JSON structure for the test.
    - `actualOutput`: A string representing the actual output produced by the test.
    - `actualJSON`: An object representing the actual JSON structure produced by the test.
    - `results`: An object containing the explanation and score of the test results.
- **Description**: The `TestResult` interface defines the structure for storing the results of a test, including both the input and expected output, as well as the actual output and a comparison of the expected and actual results. It is used to encapsulate the data necessary to evaluate the performance of a test, including a detailed explanation and a score indicating the success of the test.


# Types

---
### Resemblence<!-- {{#data_structure:Promptimizer/src/population.Resemblence}} -->
- **Members**:
    - `Father`: Represents a resemblance to the father.
    - `Mother`: Represents a resemblance to the mother.
    - `None`: Represents no specific resemblance to either parent.
- **Description**: The `Resemblence` enum defines a set of constants that represent the resemblance of an object to either a 'Father', 'Mother', or 'None'. This type is used to specify the degree or type of resemblance an object should have, particularly in the context of genetic algorithms or similar processes where crossover operations might result in offspring that resemble one parent more than the other, or neither.


