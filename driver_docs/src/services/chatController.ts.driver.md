# Purpose
The provided code defines a `ChatController` class, which is a module intended to handle chat interactions using AI models. This class is part of a broader system that involves AI-driven communication, as indicated by its dependencies on various models and a factory for generating AI clients. The primary functionality of the `ChatController` is to manage the process of sending chat messages to an AI model and handling the responses. It ensures that the necessary parameters, such as the model, temperature, and system prompt, are present before proceeding with the request. The class also includes logic to enforce JSON responses when required, retrying the request up to a specified number of attempts if the response does not conform to the expected JSON format.

The `ChatController` class is a critical component in a system that likely involves multiple AI models, as suggested by the imports of different model enums and the use of a factory pattern to create AI clients. The class encapsulates the logic for interacting with these models, including error handling and response validation. It defines a public API through its `chat` method, which is asynchronous and returns a promise of a `ChatMessage`. This method is designed to be robust, with error handling for missing parameters and a retry mechanism for ensuring valid JSON responses, highlighting its role in maintaining the integrity and reliability of the chat interactions within the system.
# Imports and Dependencies

---
- `../models/prompts/abstract`
- `../models/message`
- `../models/context`
- `./GenAIFactory`
- `lodash`


# Global Variables

---
### MAX\_ATTEMPTS\_FORCE\_JSON
- **Type**: `number`
- **Description**: `MAX_ATTEMPTS_FORCE_JSON` is a constant that defines the maximum number of attempts to retry sending a request to the AI model when the response does not contain a valid JSON, especially when the `forceJSON` mode is enabled.
- **Use**: This variable is used to limit the number of retries for generating a valid JSON response from the AI model.


# Classes

---
### ChatController<!-- {{#class:Promptimizer/src/services/chatController.ChatController}} -->
- **Description**: The `ChatController` class is responsible for managing chat interactions using AI models. It processes chat requests by validating input parameters such as the system prompt, model, temperature, and messages. The class utilizes a factory to create AI model clients and sends requests to generate responses. If the `forceJSON` mode is enabled, it ensures that the responses include valid JSON data, retrying the request if necessary. The class handles errors and retries up to a maximum number of attempts to ensure a valid JSON response is generated.


