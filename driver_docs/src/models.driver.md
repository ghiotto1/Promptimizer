## Folders
- **[aiChatLog](models/aiChatLog.driver.md)**: The `aiChatLog` folder in the `Promptimizer` codebase contains TypeScript files that define Mongoose models and interfaces for logging and managing chat interactions with various AI systems, including Anthropic, OpenAI, and open-source models, along with shared utilities for handling chat message requests and AI chat types.
- **[context](models/context.driver.md)**: The `context` folder in the `Promptimizer` codebase contains an `index.ts` file that defines a Mongoose-based model for managing `Context` objects, including operations for saving, deleting, and retrieving context data.
- **[message](models/message.driver.md)**: The `message` folder in the `Promptimizer` codebase contains the `index.ts` file, which defines the `ChatMessage` interface with properties for an optional ID, sender, content, and additional data.
- **[prompts](models/prompts.driver.md)**: The `prompts` folder in the `Promptimizer` codebase contains TypeScript files that define classes and configurations for modeling, managing, and evaluating AI prompts, including `AbstractPrompt` for database operations, default configurations for stock-related prompts, and `PromptExample` for conversation modeling.

## Files
- **[shared.ts](models/shared.ts.driver.md)**: The `shared.ts` file defines a type alias `ObjectId` that can be either a `mongoose.Types.ObjectId` or a `string`.
