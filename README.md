# CrewAI Travel Planner

This project demonstrates a sophisticated travel planning system built using the CrewAI framework. It leverages multiple AI agents, each with specialized roles and tools, to process a user's travel request and generate a detailed, personalized itinerary, including budget breakdown and local recommendations, outputted as a PDF document.

## Features

*   **Intelligent Request Analysis:** Understands free-form user travel requests, extracting key details like destination, dates, budget, dietary preferences, and interests.
*   **Comprehensive Research:** Agents research destinations, attractions, local culture, transport options, and accommodation.
*   **Dynamic Budgeting:** Utilizes a custom tool to calculate itemized trip costs and compares them against the user's budget.
*   **Personalized Itinerary Generation:** Creates a day-by-day plan, factoring in preferences, logistics, and even weather predictions.
*   **Dietary-Aware Recommendations:** Suggests restaurants and food experiences tailored to specific dietary needs (e.g., Jain).
*   **Weather and Risk Assessment:** Provides a 5-day weather forecast and flags potential travel risks.
*   **Output to PDF:** Generates a polished, readable PDF of the complete travel plan.

## Setup

To run this project, you'll need the following:

1.  **Dependencies:** Install the required Python packages:
    ```bash
    %pip install -q crewai>=0.86.0 crewai-tools>=0.17.0 "requests==2.32.4" langchain-google-genai fpdf2
    ```
2.  **API Keys:** Obtain API keys for the following services and set them as environment variables (or use Google Colab secrets):
    *   `GEMINI_API_KEY`: For the generative AI model (required).
    *   `SERPER_API_KEY`: For web search capabilities (optional, but highly recommended for research agents).
    *   `OPENWEATHER_API_KEY`: For weather forecasts (optional, for the Weather & Risk Agent).
    *   `OPENAI_API_KEY`: A dummy key might be needed if certain tools perform validation, even if an OpenAI model isn't directly used.

## How to Use

1.  **Set API Keys:** Run the cell that prompts for API keys.
2.  **Initialize LLM:** The CrewAI system uses a Gemini model, which is initialized in the notebook.
3.  **Define Tools:** Custom tools (`BudgetCalculatorTool`, `WeatherRiskTool`) and search tools (`SerperDevTool`, `WebsiteSearchTool`) are set up.
4.  **Define Agents:** Each specialized agent (User Preference Analyst, Destination Research Specialist, etc.) is defined with its role, goal, backstory, and assigned tools.
5.  **Define Tasks:** Specific tasks are created for each agent, outlining their responsibilities and expected outputs. Tasks can be chained using `context`.
6.  **Form the Crew:** All agents and tasks are assembled into a `Crew` object, specifying a sequential process.
7.  **Run the Crew:** Provide your travel request as a `user_request` string and kick off the crew execution:
    ```python
    result = await travel_crew.kickoff_async(inputs={"user_request": user_request})
    ```
8.  **Generate PDF:** The `markdown_to_pdf` function will convert the final output into a `final_itinerary.pdf` file.

## CrewAI Agents Overview

| Agent | Tools | Purpose |
|---|---|---|
| **User Preference Analyst** | LLM only | Extracts structured travel requirements from the user's request (e.g., destination, dates, budget, dietary needs, interests, constraints). |
| **Destination Research Specialist** | SerperDevTool + WebsiteSearchTool | Researches destinations, attractions, best seasons, entry requirements, and local etiquette. |
| **Budget Planner** | Custom `BudgetCalculatorTool` | Calculates itemized trip costs and checks them against the target budget. |
| **Transport and Accommodation Finder** | SerperDevTool + WebsiteSearchTool | Finds realistic flight/train/bus and hotel options matching budget and preferences. |
| **Itinerary Planner** | WebsiteSearchTool | Builds a coherent day-by-day plan from all collected information. |
| **Local Food and Dining Planner** | SerperDevTool + WebsiteSearchTool | Recommends restaurants and food experiences based on dietary preferences. |
| **Weather and Travel Risk Analyst** | Custom `WeatherRiskTool` | Fetches a 5-day forecast and flags basic travel risks, providing packing suggestions. |
| **Quality Reviewer** | LLM only | Reviews the complete travel plan for consistency, realism, and produces the final polished document. |

## Output

The final output is a detailed `final_itinerary.pdf` file, structured into sections like Trip Overview, Destination Highlights, Transport & Stay, Budget Breakdown, Day-by-Day Itinerary, and Weather & Packing Notes.
