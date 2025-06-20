# Purpose
This code file defines a module that provides functionality for handling and transforming chat messages, specifically in the context of a generative AI service. The module includes utility functions and an abstract class, indicating that it is intended to be imported and extended by other parts of an application. The [`transformMessageContent`](#Promptimizer/src/services/GenerativeAIServiceClienttransformMessageContent) function processes a `ChatMessageWithRole` object by ensuring its content is non-empty, appending any associated data in a formatted manner, and normalizing whitespace. The [`transformSenderToRole`](#Promptimizer/src/services/GenerativeAIServiceClienttransformSenderToRole) function converts a `ChatMessage` object into a `ChatMessageWithRole` by mapping the sender to a predefined role, such as "user," "assistant," or "system."

The module also defines a constant `MESSAGE_LIMIT`, which likely serves as a constraint for message handling, and an abstract class `GenerativeAIServiceClient`. This class outlines the structure for service clients that interact with a generative AI system, requiring implementations of methods for generating embeddings from prompts and sending chat messages. The presence of this abstract class suggests that the module is part of a larger framework for building AI-driven chat applications, providing a standardized interface for extending AI service capabilities.
# Imports and Dependencies

---
- `../models/message`
- `../models/aiChatLog/shared`


# Global Variables

---
### MESSAGE\_LIMIT
- **Type**: `number`
- **Description**: `MESSAGE_LIMIT` is a constant numeric value set to 100, representing the maximum number of messages allowed or processed in a given context.
- **Use**: This variable is used to enforce a limit on the number of messages, likely to control resource usage or maintain performance.


# Classes

---
### GenerativeAIServiceClient<!-- {{#class:Promptimizer/src/services/GenerativeAIServiceClient.GenerativeAIServiceClient}} -->
- **Description**: The `GenerativeAIServiceClient` is an abstract class that defines the structure for a client interacting with a generative AI service. It mandates the implementation of two methods: `embeddings`, which takes a string prompt and returns a promise resolving to an array of numbers representing the embeddings, and `sendRequest`, which takes a `SendChatMessageRequest` and returns a promise resolving to an object containing a `ChatMessage`. This class serves as a blueprint for creating specific clients that can communicate with generative AI models.


# Interfaces

---
### ChatMessageWithRole<!-- {{#interface:Promptimizer/src/services/GenerativeAIServiceClient.ChatMessageWithRole}} -->
- **Members**:
    - `role`: Specifies the role of the message sender, which can be 'user', 'assistant', or 'system'.
    - `content`: Holds the textual content of the chat message.
    - `data`: Optional field that can contain any additional data related to the message.
- **Description**: The ChatMessageWithRole interface defines the structure for chat message objects, specifying a role to indicate the sender type ('user', 'assistant', or 'system'), the message content as a string, and an optional data field for any additional information. This interface is used to ensure that chat messages have a consistent format, which is crucial for processing and transforming messages within chat applications.


# Functions

---
### transformMessageContent<!-- {{#callable:Promptimizer/src/services/GenerativeAIServiceClient.transformMessageContent}} -->
The `transformMessageContent` function processes a `ChatMessageWithRole` object by ensuring its content is a string, appending any data as a JSON string, and cleaning up whitespace before returning the modified message.
- **Inputs**:
    - `message`: A `ChatMessageWithRole` object containing a role, content, and optional data to be transformed.
- **Control Flow**:
    - Check if `message.content` is falsy and set it to an empty string if so.
    - If `message.data` exists, append its JSON string representation to `message.content` within code block markers.
    - Delete the `data` property from the `message` object.
    - Normalize whitespace in `message.content` by replacing multiple spaces with a single space, tabs with spaces, and reducing multiple newlines to two newlines.
    - Return the modified `message` object.
- **Output**: A `ChatMessageWithRole` object with transformed content and no data property.


---
### transformSenderToRole<!-- {{#callable:Promptimizer/src/services/GenerativeAIServiceClient.transformSenderToRole}} -->
The function `transformSenderToRole` converts a `ChatMessage` sender into a role and returns a `ChatMessageWithRole` object.
- **Inputs**:
    - `message`: A `ChatMessage` object containing a sender and content.
- **Control Flow**:
    - Check if the `message.sender` is 'AI Assistant' or 'assistant', and assign the role 'assistant' if true.
    - Check if the `message.sender` is 'system', and assign the role 'system' if true.
    - If neither condition is met, assign the role 'user'.
    - Return an object with the determined role and the message content, defaulting to an empty string if content is undefined.
- **Output**: A `ChatMessageWithRole` object containing a role ('user', 'assistant', or 'system') and the message content.


