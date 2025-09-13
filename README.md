# hbw-ai

Grounding - short briefing document

This provides a detailed overview of the "HBW AI Assistant with Knowledge Grounding" application, highlighting its main themes, functionalities, and key technical details derived from the provided source code.

Main Themes

The core themes of this application revolve around enhanced user interaction with AI, knowledge grounding for reliable information, and robust system logging for performance and debugging. It aims to deliver a helpful, professional, and accurate AI assistant specifically tailored for "Habits for a Better World" (HBW) related queries.

Most Important Ideas/Facts

AI Assistant for HBW: The primary purpose of the application is to function as an "HBW AI Assistant" that is "Grounded in trusted HBW sources." This indicates a focus on providing accurate and context-specific information related to the "Habits for a Better World" initiative, including "programs, meet-ups, challenges, or healthy swaps."


Knowledge Grounding Mechanism: A crucial feature is the "knowledge_search" function, which retrieves relevant articles from a Supabase table (website_scrape) based on the user's query. This process ensures that the AI's responses are "using only information found in the CONTEXT provided," thereby preventing hallucinations and maintaining factual accuracy. The system is configured to retrieve "up to global_max_articles" (defaulting to 2) relevant titles.


LLM Gateway Integration: The AI assistant communicates with an external Large Language Model (LLM) Gateway. The call_gateway and call_gateway_BYOM functions are responsible for sending prompts and receiving responses from the LLM. The application uses openai/gpt-oss-120b as its global_model and sets a global_temperature of 0.4, indicating a preference for more deterministic and less creative responses.


Conversation and System Logging: Extensive logging is implemented to monitor and track interactions.
log(session_id, message): Records "System log message[s]" for internal processes like gateway calls and errors.
log_query(session_id, user_input, ai_response, global_messages, total_input_tokens, total_output_tokens, total_tokens): Logs detailed conversation data, including user input, AI response, the full message history, and token usage, into the habits_conversation_logs Supabase table. This is critical for analytics, debugging, and understanding AI performance.






Supabase Integration for Data Management: Supabase acts as the backend database for storing various types of data:
website_scrape: Contains the knowledge base articles (title, detail) used for grounding.
habits_system_logs: Stores system-level events and errors.
habits_conversation_logs: Stores detailed conversation history.
habits_prompts: Stores resolution prompts, allowing for configurable AI behavior.


Streamlit User Interface: The front-end is built using Streamlit, providing an interactive chat interface. Key UI elements include:

A chat window displaying conversation history.
Suggestion buttons (e.g., "What is the habits for a better world about?", "what is plant based diet?", "Tell me about ocean advocacy?").
A st.chat_input for user queries.
A "Thinking..." spinner for user feedback during AI processing.
A "Reset Conversation" button in the sidebar.

Resolution Prompt Customization: The "resolve_query" function dynamically fetches the resolution_prompt from the habits_prompts table based on global_resolution_prompt_version. This allows administrators to easily update and fine-tune the AI's guiding instructions without code changes, ensuring the assistant maintains a "polite, professional and conversational manner" and can handle off-topic queries with "a clean joke (maybe a pun on what they said) then playfully guide them back to talking about Habits for a Better World."

Token Usage Tracking: The application tracks input_tokens and output_tokens for each LLM call. This is important for cost management and understanding the computational resources consumed by the AI, and this data is logged via log_query.

Error Handling: The call_gateway and knowledge_search functions include try-except blocks to catch requests.exceptions.RequestException, json.JSONDecodeError, and TypeError, logging these errors to the system logs. In case of an AI processing failure, a generic error message "I'm sorry, I couldn't process your request at the moment. Please try again." is displayed to the user.


Session Management: Each user session is assigned a unique uuid.uuid4() key, st.session_state['key'], allowing for individual conversation histories and logging.

This briefing document provides a comprehensive overview of the HBW AI Assistant, focusing on its design principles, technical implementation, and user experience.

