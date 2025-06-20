# Purpose
This JavaScript module is designed to facilitate the interaction with an AI-based stock screener system. It defines two primary configurations: `evaluatorPrompt` and `bestPrompt`, each serving distinct roles in the AI's operation. The `evaluatorPrompt` is configured to assess the accuracy of the AI's output by comparing it to a ground truth, using a detailed scoring guide to evaluate the correctness of SQL queries and the thought process behind them. This prompt is crucial for ensuring the reliability and accuracy of the AI's financial analysis capabilities.

The `bestPrompt`, on the other hand, is configured to generate SQL queries in response to financial questions, leveraging BigQuery to fetch and analyze data. It includes a comprehensive set of instructions and constraints to guide the AI in generating accurate and efficient queries, ensuring that the AI can handle complex financial inquiries. Both prompts are part of a broader system that uses OpenAI models to process and evaluate financial data, with the module serving as a configuration file that sets up the necessary parameters and guidelines for the AI's operation.
# Imports and Dependencies

---
- `./abstract`


# Global Variables

---
### PROMPT\_NAME
- **Type**: `string`
- **Description**: The `PROMPT_NAME` variable is a constant string that holds the name 'AI Stock Screener'. It is used as an identifier or label for a specific prompt related to stock screening within the application.
- **Use**: This variable is used to assign a name to the `bestPrompt` object, which is likely utilized in contexts where the application needs to refer to or display the name of the stock screening prompt.


---
### OUTPUT\_EVALUATOR\_PROMPT\_NAME
- **Type**: `string`
- **Description**: The variable `OUTPUT_EVALUATOR_PROMPT_NAME` is a constant string that holds the name 'AI Stock Screener Output Evaluator'. It is used to identify or label a specific prompt related to evaluating the output of an AI stock screener model.
- **Use**: This variable is used as a name identifier within the `evaluatorPrompt` object to specify the prompt's purpose in evaluating AI-generated stock screener outputs.


---
### evaluatorPrompt
- **Type**: `object`
- **Description**: The `evaluatorPrompt` is a configuration object used to evaluate the output of an AI model, specifically in the context of stock screening. It contains properties such as the model to be used, temperature settings, and a detailed system prompt that guides the evaluation process. The system prompt includes instructions and a scoring guide for assessing the correctness of the model's output against a ground truth, with considerations for SQL query alignment and output quality.
- **Use**: This variable is used to configure and guide the evaluation of AI model outputs, ensuring they are assessed accurately against expected results.


---
### bestPrompt
- **Type**: `object`
- **Description**: The `bestPrompt` variable is an object that defines the configuration for an AI model prompt used to answer financial questions by generating BigQuery queries. It includes properties such as the model to use, temperature setting, whether to force JSON output, and a detailed description of the prompt's purpose and constraints. The prompt is designed to handle financial queries, generate SQL queries, and provide examples of how to structure responses.
- **Use**: This variable is used to configure and define the behavior of an AI model for generating SQL queries to answer financial questions.


