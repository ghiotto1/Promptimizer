# Purpose
This TypeScript file defines a narrow set of types and an enumeration used for handling chat messages in an application. It includes an interface, `SendChatMessageRequest`, which specifies the structure of a request to send a chat message, including properties like `systemPrompt`, `model`, `temperature`, and an array of `ChatMessage` objects. Additionally, it defines an enumeration, `AiChatTypeEnum`, which categorizes different types of AI chat interactions, such as "image", "embeddings", and "chat". This file is likely part of a larger system dealing with AI-driven chat functionalities, providing a structured way to manage and categorize chat message requests and types.
# Imports and Dependencies

---
- `../message`
- `../shared`


# Interfaces

---
### SendChatMessageRequest<!-- {{#interface:Promptimizer/src/models/aiChatLog/shared.SendChatMessageRequest}} -->
- **Members**:
    - `systemPrompt`: A string representing the initial prompt or instruction for the chat system.
    - `model`: A string specifying the AI model to be used for generating chat responses.
    - `temperature`: A number indicating the randomness or creativity level of the AI's responses.
    - `messages`: An array of ChatMessage objects representing the conversation history or context.
- **Description**: The SendChatMessageRequest interface defines the structure for a request to send a chat message to an AI system. It includes a system prompt to guide the AI's responses, the model to be used, a temperature setting to control response variability, and a list of previous chat messages to provide context. This interface ensures that all necessary information is provided to generate a coherent and contextually relevant AI response.


# Types

---
### AiChatTypeEnum<!-- {{#data_structure:Promptimizer/src/models/aiChatLog/shared.AiChatTypeEnum}} -->
- **Members**:
    - `Image`: Represents a chat type that deals with image processing.
    - `Embeddings`: Represents a chat type that involves generating or using embeddings.
    - `Chat`: Represents a standard chat interaction type.
- **Description**: The `AiChatTypeEnum` is an enumeration that defines the different types of AI chat interactions that can occur. It includes three specific types: `Image`, for image-related processing; `Embeddings`, for operations involving embeddings; and `Chat`, for standard chat interactions. This enum provides a clear contract for categorizing and handling different AI chat functionalities within the application.


