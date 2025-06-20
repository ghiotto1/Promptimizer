# Purpose
This TypeScript file defines a module centered around managing and interacting with "prompts" in a system that likely involves AI models, as indicated by the enumeration of various AI model types such as OpenAI and Anthropic models. The core functionality is encapsulated in the `AbstractPrompt` class, which provides a structured way to create, validate, save, delete, and retrieve prompt instances from a MongoDB database using Mongoose. The file defines several enumerations to categorize different AI models, which are used to specify the model associated with each prompt. The `IAbstractPrompt` interface outlines the structure of a prompt, including properties like `name`, `systemPrompt`, `description`, `examples`, `model`, and `temperature`.

The `AbstractPrompt` class includes methods for saving prompts to a database, validating prompt data, deleting prompts (with archival), and generating full system prompts by integrating examples and context. The class also provides static methods for retrieving prompts by ID or name, and for listing all prompts. The use of Mongoose models (`PromptModel` and `ArchivePromptModel`) facilitates interaction with the database, ensuring that prompt data is stored and managed efficiently. This module is designed to be imported and used in other parts of the application, providing a robust API for managing prompt data in a system that likely involves AI-driven interactions.
# Imports and Dependencies

---
- `mongoose`
- `../context`
- `../shared`
- `./example`
- `lodash`


# Global Variables

---
### promptSchema
- **Type**: `Schema`
- **Description**: The `promptSchema` is a Mongoose schema definition for a prompt object, which includes fields such as `name`, `systemPrompt`, `description`, `examples`, `model`, `temperature`, and `forceJSON`. Each field has a specified type and some fields are marked as required, ensuring that any prompt object created with this schema adheres to these constraints.
- **Use**: This variable is used to define the structure and constraints of prompt documents stored in a MongoDB collection via Mongoose models.


# Classes

---
### AbstractPrompt<!-- {{#class:Promptimizer/src/models/prompts/abstract.AbstractPrompt}} -->
- **Members**:
    - `_id`: Optional identifier for the prompt.
    - `name`: Name of the prompt.
    - `systemPrompt`: System prompt text.
    - `description`: Description of the prompt.
    - `examples`: List of prompt examples.
    - `model`: Model used for the prompt, defaulting to OpenAiModelEnum.fourOmini.
    - `temperature`: Temperature setting for the prompt, defaulting to 0.
    - `forceJSON`: Boolean indicating if JSON response is forced.
- **Description**: The AbstractPrompt class represents a structured prompt with attributes such as name, description, system prompt, examples, and model configuration. It provides methods to save, validate, and delete prompts, as well as to generate a full system prompt with context and example integration. The class also includes static methods to retrieve prompts from a database by ID or name, and to list all prompts. It supports a feature to enforce JSON responses, which is reflected in the prompt generation logic.
- **Methods**:
    - [`Promptimizer/src/models/prompts/abstract.AbstractPrompt.constructor`](#AbstractPromptconstructor)
    - [`Promptimizer/src/models/prompts/abstract.AbstractPrompt.save`](#AbstractPromptsave)
    - [`Promptimizer/src/models/prompts/abstract.AbstractPrompt.validate`](#AbstractPromptvalidate)
    - [`Promptimizer/src/models/prompts/abstract.AbstractPrompt.delete`](#AbstractPromptdelete)
    - [`Promptimizer/src/models/prompts/abstract.AbstractPrompt.getFullSystemPrompt`](#AbstractPromptgetFullSystemPrompt)
    - [`Promptimizer/src/models/prompts/abstract.AbstractPrompt.getExampleText`](#AbstractPromptgetExampleText)
    - [`Promptimizer/src/models/prompts/abstract.AbstractPrompt.findOne`](#AbstractPromptfindOne)
    - [`Promptimizer/src/models/prompts/abstract.AbstractPrompt.findOneByName`](#AbstractPromptfindOneByName)
    - [`Promptimizer/src/models/prompts/abstract.AbstractPrompt.find`](#AbstractPromptfind)

**Methods**

---
#### AbstractPrompt\.constructor<!-- {{#callable:Promptimizer/src/models/prompts/abstract.AbstractPrompt.constructor}} -->
The constructor initializes an AbstractPrompt instance by validating and assigning properties from an IAbstractPrompt object.
- **Inputs**:
    - `obj`: An object of type IAbstractPrompt containing properties to initialize the AbstractPrompt instance.
- **Control Flow**:
    - The constructor first calls the validate method to ensure the input object meets required conditions.
    - It assigns the _id property from the input object to the instance's _id property.
    - The name, description, and systemPrompt properties are trimmed and assigned from the input object, defaulting to an empty string if undefined.
    - The examples property is assigned from the input object, defaulting to an empty array if undefined.
    - The model property is cast to ModelEnum and assigned from the input object.
    - The temperature property is directly assigned from the input object.
    - The forceJSON property is assigned from the input object, defaulting to false if undefined.
- **Output**: An instance of AbstractPrompt with properties initialized from the input object.
- **See also**: [`Promptimizer/src/models/prompts/abstract.AbstractPrompt`](#AbstractPrompt)  (Base Class)


---
#### AbstractPrompt\.save<!-- {{#callable:Promptimizer/src/models/prompts/abstract.AbstractPrompt.save}} -->
The `save` method persists an `AbstractPrompt` instance to the database, updating an existing record if it has an `_id`, or creating a new record if it does not.
- **Inputs**: None
- **Control Flow**:
    - Check if the instance has an `_id` property.
    - If `_id` exists, update the existing record in the `PromptModel` collection with the current instance data using `updateOne` with `upsert` set to true.
    - If `_id` does not exist, create a new record in the `PromptModel` collection using `create`, and assign the newly created record's `id` to the instance's `_id` property.
- **Output**: The method does not return any value (void), but it updates or creates a database record and potentially modifies the `_id` property of the instance.
- **See also**: [`Promptimizer/src/models/prompts/abstract.AbstractPrompt`](#AbstractPrompt)  (Base Class)


---
#### AbstractPrompt\.validate<!-- {{#callable:Promptimizer/src/models/prompts/abstract.AbstractPrompt.validate}} -->
The `validate` method checks if the provided `IAbstractPrompt` object has the required properties: `model` and `temperature`, and throws an error if any are missing.
- **Inputs**:
    - `obj`: An object of type `IAbstractPrompt` that needs to be validated for required properties.
- **Control Flow**:
    - Check if the `obj` is null or undefined; if so, throw an error indicating that a prompt is required.
    - Check if the `model` property of `obj` is missing; if so, throw an error indicating that a prompt model is required.
    - Check if the `temperature` property of `obj` is null or undefined using lodash's `isNil` function; if so, throw an error indicating that a prompt temperature is required.
- **Output**: The method does not return any value; it throws an error if validation fails.
- **See also**: [`Promptimizer/src/models/prompts/abstract.AbstractPrompt`](#AbstractPrompt)  (Base Class)


---
#### AbstractPrompt\.delete<!-- {{#callable:Promptimizer/src/models/prompts/abstract.AbstractPrompt.delete}} -->
The `delete` method removes a prompt from the database and archives it if it has been saved.
- **Inputs**: None
- **Control Flow**:
    - Check if the `_id` property is not set, and throw an error if it is not, indicating the prompt has not been saved.
    - Create a new entry in the `ArchivePromptModel` with the current prompt data, excluding the `_id`, and including the original `_id` as `promptId`.
    - Delete the prompt from the `PromptModel` using the `_id`.
- **Output**: The method returns a `Promise` that resolves to `void`, indicating the completion of the delete operation.
- **See also**: [`Promptimizer/src/models/prompts/abstract.AbstractPrompt`](#AbstractPrompt)  (Base Class)


---
#### AbstractPrompt\.getFullSystemPrompt<!-- {{#callable:Promptimizer/src/models/prompts/abstract.AbstractPrompt.getFullSystemPrompt}} -->
The `getFullSystemPrompt` method generates a complete system prompt by replacing placeholders with context values, appending example text, and optionally adding JSON mode instructions.
- **Inputs**:
    - `examples`: An array of `PromptExample` objects that provide example conversations to be included in the prompt.
    - `contexts`: An array of `IContext` objects that provide context values to replace placeholders in the system prompt.
- **Control Flow**:
    - Initialize a `contextMap` by reducing the `contexts` array, mapping each context's name to its value, and throwing an error if a context lacks a name.
    - Replace placeholders in `systemPrompt` with corresponding values from `contextMap`, throwing an error if a placeholder's name is not found in `contextMap`.
    - Generate example text using `AbstractPrompt.getExampleText` with the provided examples and `forceJSON` flag.
    - If `forceJSON` is true, append a specific instruction text about JSON mode to `fullPrompt`.
    - Combine the example text, description, and instructions into a single string, filtering out any empty parts, and return the result.
- **Output**: A string that combines example text, description, and instructions, with placeholders replaced by context values and optional JSON mode instructions.
- **See also**: [`Promptimizer/src/models/prompts/abstract.AbstractPrompt`](#AbstractPrompt)  (Base Class)


---
#### AbstractPrompt\.getExampleText<!-- {{#callable:Promptimizer/src/models/prompts/abstract.AbstractPrompt.getExampleText}} -->
The `getExampleText` method generates a formatted string representation of conversation examples, optionally appending JSON-related instructions if `forceJSON` is true.
- **Inputs**:
    - `examples`: An array of `PromptExample` objects representing conversation examples.
    - `forceJSON`: A boolean indicating whether to append JSON-related instructions to the conversation content.
- **Control Flow**:
    - Check if the `examples` array is empty; if so, return an empty string.
    - Iterate over each `example` in the `examples` array to process its `conversation` property.
    - For each `conversation`, iterate over its `messages` to construct a string for each message.
    - For each `message`, append its `content` to the result string, and if `forceJSON` is true, append a JSON-related instruction.
    - If `message.data` exists, append its JSON stringified form to the result string.
    - Join all message strings with newline characters and separate different examples with a delimiter.
- **Output**: A formatted string representing the conversation examples, with optional JSON instructions appended.
- **See also**: [`Promptimizer/src/models/prompts/abstract.AbstractPrompt`](#AbstractPrompt)  (Base Class)


---
#### AbstractPrompt\.findOne<!-- {{#callable:Promptimizer/src/models/prompts/abstract.AbstractPrompt.findOne}} -->
The `findOne` method retrieves a single prompt from the database by its ObjectId and returns it as an `AbstractPrompt` instance.
- **Inputs**:
    - `id`: An ObjectId representing the unique identifier of the prompt to be retrieved.
- **Control Flow**:
    - The method begins by attempting to find a prompt in the database using the provided ObjectId with `PromptModel.findOne({ _id: id }).exec()`.
    - If no prompt is found, the method throws an error with the message 'Prompt not found'.
    - If a prompt is found, it creates and returns a new instance of `AbstractPrompt` initialized with the retrieved model.
- **Output**: An instance of `AbstractPrompt` initialized with the data of the found prompt model.
- **See also**: [`Promptimizer/src/models/prompts/abstract.AbstractPrompt`](#AbstractPrompt)  (Base Class)


---
#### AbstractPrompt\.findOneByName<!-- {{#callable:Promptimizer/src/models/prompts/abstract.AbstractPrompt.findOneByName}} -->
The `findOneByName` method retrieves a prompt by its name from the database and returns it as an `AbstractPrompt` instance.
- **Inputs**:
    - `name`: The name of the prompt to be retrieved from the database.
- **Control Flow**:
    - The method uses `PromptModel.findOne` to search for a prompt with the specified name in the database.
    - If no prompt is found, it throws an error indicating that a prompt with the given name does not exist.
    - If a prompt is found, it creates and returns a new `AbstractPrompt` instance using the retrieved model.
- **Output**: An instance of `AbstractPrompt` initialized with the data of the found prompt model.
- **See also**: [`Promptimizer/src/models/prompts/abstract.AbstractPrompt`](#AbstractPrompt)  (Base Class)


---
#### AbstractPrompt\.find<!-- {{#callable:Promptimizer/src/models/prompts/abstract.AbstractPrompt.find}} -->
The `find` method retrieves all prompt documents from the database and returns them as instances of the `AbstractPrompt` class.
- **Inputs**: None
- **Control Flow**:
    - The method calls `PromptModel.find({})` to query all documents in the database collection associated with `PromptModel`.
    - It uses `.exec()` to execute the query and return a promise.
    - The promise resolves with the retrieved models, which are then mapped to new instances of `AbstractPrompt` using `.map((model) => new AbstractPrompt(model))`.
    - The method returns a promise that resolves with an array of `AbstractPrompt` instances.
- **Output**: A promise that resolves to an array of `AbstractPrompt` instances representing all prompt documents in the database.
- **See also**: [`Promptimizer/src/models/prompts/abstract.AbstractPrompt`](#AbstractPrompt)  (Base Class)



# Interfaces

---
### IAbstractPrompt<!-- {{#interface:Promptimizer/src/models/prompts/abstract.IAbstractPrompt}} -->
- **Members**:
    - `_id`: Optional identifier for the prompt, typically used for database purposes.
    - `name`: The name of the prompt, which is a required string.
    - `systemPrompt`: A string representing the system prompt or initial instruction.
    - `description`: A string providing a description of the prompt.
    - `examples`: An array of PromptExample objects that illustrate how the prompt can be used.
    - `model`: A string indicating the model to be used with the prompt.
    - `temperature`: A number that defines the randomness or creativity level of the model's responses.
    - `forceJSON`: An optional boolean indicating if the response should be forced into JSON format.
- **Description**: The IAbstractPrompt interface defines the structure for a prompt object used in a system that interacts with various AI models. It includes properties for identifying the prompt, such as an optional ID and a required name, as well as fields for the system prompt, description, and examples that demonstrate its usage. The interface also specifies the model to be used, the temperature setting for response variability, and an optional flag to enforce JSON-formatted responses. This interface ensures that any prompt object adheres to a consistent format, facilitating integration with AI models and databases.


# Types

---
### OpenAiModelEnum<!-- {{#data_structure:Promptimizer/src/models/prompts/abstract.OpenAiModelEnum}} -->
- **Members**:
    - `fourOmini`: Represents the GPT-4o-mini model with a specific version date.
    - `threePointFiveNew`: Represents the GPT-3.5-turbo model with a specific version date.
    - `threePointFive`: Represents another version of the GPT-3.5-turbo model.
    - `four`: Represents the GPT-4 model in preview mode with a specific version date.
    - `fourO`: Represents the GPT-4o model with a specific version date.
- **Description**: The `OpenAiModelEnum` is an enumeration that defines a set of constants representing different versions of OpenAI models. Each member of the enum corresponds to a specific model version, identified by a unique string that typically includes the model name and a version date. This enum is used to ensure that only valid model identifiers are used within the application, providing a clear and type-safe way to reference specific OpenAI models.


---
### AnthropicModels<!-- {{#data_structure:Promptimizer/src/models/prompts/abstract.AnthropicModels}} -->
- **Members**:
    - `claude3Haiku`: Represents the Claude 3 Haiku model with a specific version identifier.
    - `claude3Opus`: Represents the Claude 3 Opus model with a specific version identifier.
    - `claude35Sonnet`: Represents the Claude 3.5 Sonnet model with a specific version identifier.
- **Description**: The `AnthropicModels` enum defines a set of constants representing different versions of the Claude models, each identified by a unique string. These models are likely used for natural language processing tasks, and the enum provides a way to reference specific versions of the Claude models in a type-safe manner within the codebase.


---
### OpenSourceImageModels<!-- {{#data_structure:Promptimizer/src/models/prompts/abstract.OpenSourceImageModels}} -->
- **Members**:
    - `llava`: Represents the 'llava' open-source image model.
    - `bakllava`: Represents the 'bakllava' open-source image model.
- **Description**: The `OpenSourceImageModels` enum defines a set of string constants representing different open-source image models. It provides a standardized way to refer to these models within the codebase, ensuring consistency and reducing the likelihood of errors when specifying model names. This enum currently includes two models: 'llava' and 'bakllava', which can be used to identify and work with these specific image models in the application.


---
### OpenSourceModelEnum<!-- {{#data_structure:Promptimizer/src/models/prompts/abstract.OpenSourceModelEnum}} -->
- **Members**:
    - `Phi`: Represents the 'phi' open-source model.
    - `Dolphin`: Represents the 'dolphin-mixtral' open-source model.
    - `MixtralInstruct`: Represents the 'mixtral:instruct' open-source model.
    - `Llama3`: Represents the 'llama3.1' open-source model.
    - `Llama2`: Represents the 'llama2' open-source model.
    - `PhIndCodeLlama`: Represents the 'phind-codellama' open-source model.
    - `Openchat`: Represents the 'openchat' open-source model.
    - `OpenHermesMistral`: Represents the 'openhermes2.5-mistral' open-source model.
    - `DeepSeekLarge`: Represents the 'deepseek-llm:67b-chat-q3_K_S' open-source model.
    - `DeepSeek7b`: Represents the 'deepseek-llm:7b-chat' open-source model.
    - `Orca`: Represents the 'orca2' open-source model.
    - `Codebooga`: Represents the 'codebooga' open-source model.
    - `Vicuna13b_16k`: Represents the 'vicuna:13b-16k' open-source model.
    - `Zephyr`: Represents the 'zephyr' open-source model.
    - `StablelmZephyr`: Represents the 'stablelm-zephyr' open-source model.
- **Description**: The `OpenSourceModelEnum` is an enumeration that defines a set of constants representing various open-source models. Each member of the enum corresponds to a specific model, identified by a unique string value. This enum is used to standardize the representation of different open-source models within the application, allowing for consistent referencing and usage across the codebase.


