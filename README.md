# AgentsVille Trip Planner

A multi-agent travel assistant system that demonstrates advanced LLM reasoning techniques for intelligent trip planning. This project showcases role-based prompting, chain-of-thought reasoning, ReAct prompting, and feedback loops to create personalized travel itineraries.

## 🎯 Project Overview

The AgentsVille Trip Planner is an AI-powered system that helps plan the perfect vacation to the wonderful city of AgentsVille! The system uses multiple AI agents to:

1. Generate personalized day-by-day itineraries based on traveler preferences
2. Evaluate itineraries against multiple quality criteria
3. Iteratively refine plans using feedback loops and tool-assisted reasoning
4. Ensure activities match weather conditions, budget constraints, and user interests

## ✨ Key Features

### Advanced LLM Techniques

- **Role-Based Prompting**: Specialized agents act as travel planning experts
- **Chain-of-Thought Reasoning**: Step-by-step planning of itineraries
- **ReAct Prompting**: Thought → Action → Observation cycles for iterative refinement
- **Feedback Loops**: Self-evaluation and continuous improvement

### Intelligent Planning

- Weather-aware activity recommendations
- Budget-conscious itinerary generation
- Interest-based activity matching
- Multi-traveler support with diverse interests
- Automated evaluation and quality assurance

## 🏗️ Architecture

The system consists of two primary agents:

### 1. ItineraryAgent
- Generates initial travel plans using Chain-of-Thought prompting
- Considers weather forecasts, available activities, and traveler preferences
- Outputs structured `TravelPlan` objects with detailed daily itineraries

### 2. ItineraryRevisionAgent
- Refines itineraries using ReAct-based reasoning
- Uses tools to gather information and verify constraints
- Iteratively improves plans based on evaluation feedback
- Incorporates user feedback (e.g., "at least 2 activities per day")

## 📦 Installation

### Prerequisites

- Python 3.8 or higher
- OpenAI API key or Vocareum API access

### Setup

1. Clone the repository:
```bash
git clone <repository-url>
cd agentsville-trip-planner-a-multi-agent-travel-assistant-system
```

2. Create a virtual environment:
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Configure API access:
Create a `.env` file with your OpenAI API key:
```
OPENAI_API_KEY=your_api_key_here
```

## 🚀 Usage

### Running the Notebook

Open and run `project_starter.ipynb` in Jupyter Notebook or Jupyter Lab:

```bash
jupyter notebook project_starter.ipynb
```

### Basic Workflow

1. **Define Vacation Details**: Specify travelers, destination, dates, interests, and budget
2. **Review Weather & Activities**: Check available data for the trip dates
3. **Generate Initial Itinerary**: Use the `ItineraryAgent` to create a first draft
4. **Evaluate the Plan**: Run evaluation functions to check quality
5. **Refine with Feedback**: Use the `ItineraryRevisionAgent` to improve the itinerary
6. **Generate Trip Narrative**: Create a fun summary of the planned trip

### Example: Creating a Vacation

```python
from project_lib import Interest
from pydantic import BaseModel
import datetime

# Define vacation information
VACATION_INFO_DICT = {
    "travelers": [
        {
            "name": "Yuri",
            "age": 30,
            "interests": ["tennis", "cooking", "comedy", "technology"],
        },
        {
            "name": "Hiro",
            "age": 25,
            "interests": ["reading", "music", "theatre", "art"],
        },
    ],
    "destination": "AgentsVille",
    "date_of_arrival": "2025-06-10",
    "date_of_departure": "2025-06-12",
    "budget": 130,
}

# Validate with Pydantic
vacation_info = VacationInfo.model_validate(VACATION_INFO_DICT)

# Generate itinerary
itinerary_agent = ItineraryAgent(client=client, model=MODEL)
travel_plan = itinerary_agent.get_itinerary(vacation_info=vacation_info)

# Refine with feedback
revision_agent = ItineraryRevisionAgent()
refined_plan = revision_agent.run_react_cycle(
    original_travel_plan=travel_plan,
    max_steps=15
)
```

## 📁 Project Structure

```
agentsville-trip-planner-a-multi-agent-travel-assistant-system/
├── README.md                    # This file
├── requirements.txt             # Python dependencies
├── project_lib.py              # Core utility functions and data
├── project_starter.ipynb       # Main Jupyter notebook
├── .env.example                # Example environment configuration
└── LICENSE                     # Project license
```

## 🔧 Key Components

### Data Models

Built with Pydantic for robust data validation:

- **`Traveler`**: Represents a traveler with name, age, and interests
- **`VacationInfo`**: Contains travelers, destination, dates, and budget
- **`Activity`**: Represents an available activity with timing, location, price, and related interests
- **`Weather`**: Weather information for a specific date
- **`ItineraryDay`**: Daily plan with weather and activity recommendations
- **`TravelPlan`**: Complete itinerary with all days and total cost

### Evaluation Functions

The system includes comprehensive evaluation criteria:

1. **`eval_start_end_dates_match`**: Verifies dates align with vacation info
2. **`eval_total_cost_is_accurate`**: Checks cost calculations
3. **`eval_total_cost_is_within_budget`**: Ensures budget compliance
4. **`eval_itinerary_events_match_actual_events`**: Prevents hallucinated activities
5. **`eval_itinerary_satisfies_interests`**: Matches activities to traveler interests
6. **`eval_activities_and_weather_are_compatible`**: Uses LLM to check weather appropriateness
7. **`eval_traveler_feedback_is_incorporated`**: Validates user feedback integration

### Tools

The ItineraryRevisionAgent has access to four tools:

1. **`calculator_tool`**: Accurate cost calculations using numexpr
2. **`get_activities_by_date_tool`**: Retrieve activities for specific dates
3. **`run_evals_tool`**: Execute all evaluation functions on a travel plan
4. **`final_answer_tool`**: Return the finalized itinerary

### Mocked APIs

For demonstration purposes, the project includes simulated APIs:

- **`call_weather_api_mocked`**: Returns weather forecasts for AgentsVille (June 10-15, 2025)
- **`call_activities_api_mocked`**: Returns available activities by date
- **`call_activity_by_id_api_mocked`**: Retrieves specific activity details

## 🎭 Available Interests

The system supports activities matching these interests:

- Art
- Cooking
- Comedy
- Dancing
- Fitness
- Gardening
- Hiking
- Movies
- Music
- Photography
- Reading
- Sports
- Technology
- Theatre
- Tennis
- Writing

## 📊 Example Output

A successfully planned itinerary includes:

- **City**: AgentsVille
- **Dates**: Start and end dates
- **Total Cost**: Sum of all activities
- **Daily Itineraries**: For each day:
  - Date
  - Weather forecast (temperature, condition)
  - Activity recommendations with:
    - Activity details (name, time, location, description, price)
    - Reasons for recommendation based on traveler interests

## 🔍 Evaluation Results

The system provides detailed evaluation results showing:

- ✅ Success/failure status for each criterion
- Detailed failure messages for debugging
- Interest matching per traveler
- Weather compatibility checks

## 🎨 Advanced Features

### Chain-of-Thought Prompting

The ItineraryAgent uses structured prompting to:
1. Analyze weather conditions
2. Review available activities
3. Match activities to interests
4. Calculate and verify costs
5. Select balanced activities

### ReAct Cycle

The ItineraryRevisionAgent follows:
```
THOUGHT → ACTION → OBSERVATION → THOUGHT → ...
```

Until all evaluation criteria pass and the final itinerary is ready.

### Feedback Integration

The system can incorporate user feedback such as:
- "I want at least two activities per day"
- Custom budget adjustments
- Interest priority changes

## 🛠️ Development

### Customizing Vacation Details

Modify the `VACATION_INFO_DICT` in the notebook to change:
- Number of travelers
- Traveler interests
- Destination (currently only AgentsVille supported)
- Trip dates (June 10-15, 2025)
- Budget

### Adjusting Agent Behavior

Modify system prompts to change agent behavior:
- `ITINERARY_AGENT_SYSTEM_PROMPT`: Initial planning logic
- `ITINERARY_REVISION_AGENT_SYSTEM_PROMPT`: Refinement strategy
- `ACTIVITY_AND_WEATHER_ARE_COMPATIBLE_SYSTEM_PROMPT`: Weather evaluation

### Model Selection

The project supports multiple OpenAI models:

```python
class OpenAIModel(str, Enum):
    GPT_41 = "gpt-4.1"           # Strong reasoning
    GPT_41_MINI = "gpt-4.1-mini" # Balanced (default)
    GPT_41_NANO = "gpt-4.1-nano" # Fast and cheap
```

## 📝 Requirements

- `json-repair==0.47.1`: JSON parsing and repair
- `numexpr==2.11.0`: Fast numerical expression evaluation
- `openai==1.74.0`: OpenAI API client
- `pandas==2.3.0`: Data manipulation and analysis
- `pydantic==2.11.7`: Data validation
- `python-dotenv==1.1.0`: Environment variable management
- `ipykernel`: Jupyter kernel support

## 🎓 Learning Objectives

This project demonstrates:

1. **Structured Prompting**: How to craft effective system prompts with roles, tasks, and output formats
2. **Tool Usage**: Enabling LLMs to use external tools for enhanced capabilities
3. **Iterative Refinement**: Building feedback loops for continuous improvement
4. **Data Validation**: Using Pydantic for robust data structures
5. **Multi-Agent Systems**: Coordinating specialized agents for complex tasks
6. **Evaluation Design**: Creating comprehensive quality checks

## 🎉 Just for Fun!

The project includes a narrative generation feature that creates an engaging trip summary using:
- Text-to-speech conversion
- Markdown formatting
- Highlight of best activities and experiences

## 🙏 Acknowledgments

Created as part of an advanced LLM course demonstrating practical applications of:
- Role-based prompting
- Chain-of-thought reasoning
- ReAct methodology
- Multi-agent systems

---

**Ready to plan your trip to AgentsVille?** Open the notebook and start exploring! 🚀✈️🗺️
