# NoDetours: Intelligent Travel Planning with Multi-LLM Orchestration

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-Latest-green?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![LangChain](https://img.shields.io/badge/LangChain-Integrated-orange?logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTAwIiBoZWlnaHQ9IjEwMCIgdmlld0JveD0iMCAwIDEwMCAxMDAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PC9zdmc+)](https://www.langchain.com/)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-blueviolet?logo=openai&logoColor=white)](https://openai.com/)
[![Anthropic](https://img.shields.io/badge/Anthropic-Claude-purple?logoColor=white)](https://www.anthropic.com/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue)](LICENSE)

> **Intelligent itinerary generation combining multiple LLM providers, real-time contextual APIs, and systematic evaluation frameworks. Production-grade AI system design showcasing full-stack expertise and thoughtful architecture patterns.**

## Quick Summary

NoDetours demonstrates **production-grade LLM orchestration at scale**, seamlessly integrating OpenAI (GPT-3.5/4), Anthropic Claude, and other providers through a unified interface. The system aggregates real-time travel data from 5+ APIs in parallel, generates personalized itineraries, and includes a built-in evaluation framework for systematic LLM quality assessment. This project showcases full-stack expertise (FastAPI backend, vanilla JS frontend, CLI interface) alongside thoughtful system design patterns relevant to any data-intensive AI application.

### Project Statistics
- **6** integrated LLM provider support
- **5+** external API integrations (weather, maps, search, scraping)
- **3** deployment modes: Web UI + REST API + CLI
- **Comprehensive evaluation framework** for systematic LLM benchmarking
- **Full-stack architecture** demonstrating scalability and maintainability

---

## Application Overview

NoDetours creates detailed, personalized travel plans using intelligent LLM orchestration:

- **Custom Itineraries**: AI-generated day-by-day schedules tailored to your preferences
- **Packing Lists**: Intelligent recommendations based on destination, climate, and activities
- **Budget Estimates**: Detailed cost breakdowns for different spending levels
- **Calendar Integration**: Export your itinerary directly to your calendar (ICS format)
- **Multi-Provider Support**: Choose between GPT-4, Claude, or other LLMs with automatic fallback

The system processes natural language requests like *"Help me plan a 7-day trip to Paris focusing on museums and local food"* and generates comprehensive travel recommendations enhanced with real-time data.

## Key Features

### 🎯 Intelligent Travel Planning
- **Natural Language Understanding**: Simply describe your travel plans in plain English
- **Preference Extraction**: Automatically identifies destinations, durations, interests, and budget constraints
- **Contextual Information**: Real-time weather forecasts, location details, and relevant search results
- **Multi-Modal Output**: Provides itineraries, packing lists, and detailed budget estimates
- **Calendar Export**: Download your itinerary as an ICS file for calendar integration

### 🔄 Multi-Provider LLM Orchestration
- **Flexible Provider Support**: OpenAI (GPT-3.5/4), Anthropic Claude, and extensible architecture for additional providers
- **Intelligent Fallback**: Automatic provider failover ensures reliability
- **Provider Comparison**: Built-in evaluation system compares quality across providers
- **Configuration-Driven**: Easy provider swapping via YAML configuration

### 📊 Comprehensive Evaluation Framework
- **Systematic Benchmarking**: Evaluate LLM quality across 5 metrics (accuracy, relevance, completeness, usefulness, creativity)
- **Judge LLM Pattern**: Uses separate LLM as judge for unbiased evaluation
- **Visualization Reports**: Auto-generated comparison charts and statistical analysis
- **Extensible Metrics**: Add custom evaluation criteria easily

### 🌐 Full-Stack Deployment
- **Web Interface**: Clean, intuitive UI with tabbed results display and real-time processing feedback
- **REST API**: FastAPI backend with OpenAPI documentation and async support
- **CLI Mode**: Interactive command-line interface for terminal-based usage
- **Containerized Ready**: Docker-ready with configuration for cloud deployment

## Technical Architecture

The application consists of several interconnected components forming a modular pipeline:

- **Guardrail Module**: Input validation and sanitization for safety
- **Feature Extraction**: Processes natural language input to identify travel preferences
- **Search Query Generation**: Creates optimized search queries to gather relevant information
- **Context Collection**: Aggregates data from 5+ sources concurrently (weather, maps, search, web scraping)
- **Output Generation**: Produces detailed travel plans using selected LLM
- **Multi-Provider Orchestration**: Unified interface managing multiple LLM providers
- **Web Interface**: FastAPI-based REST API with responsive HTML/JS frontend

## Repository Structure

```
nodetours/
├── api/                          # API modules for external services
│   ├── app.py                    # FastAPI backend for web application
│   ├── llm_provider.py           # Unified interface for LLM providers
│   ├── maps.py                   # Maps API for location information
│   ├── scrape.py                 # Web scraping utilities
│   ├── search.py                 # Search API wrapper
│   └── weather.py                # Weather API for forecast data
├── app/                          # Core application modules
│   ├── agent.py                  # Main Travel Planner Agent
│   └── modules/                  # Specialized modules
│       ├── context_collector.py  # Information aggregation
│       ├── guardrail.py          # Input validation
│       ├── output_generator.py   # Travel plan generation
│       ├── search_query_extractor.py  # Feature extraction
│       └── search_query_generator.py  # Query generation
├── config/                       # Configuration files
│   ├── config.yaml               # Main configuration
│   └── eval_config.yaml          # Evaluation configuration
├── eval-data/                    # Evaluation datasets
│   ├── feature_extractor_data.json
│   ├── search_query_data.json
│   └── travel_assistant_data.json
├── evaluation_runs/              # Evaluation output directory
├── static/                       # Web UI assets
│   ├── css/
│   │   └── styles.css            # Application styling
│   └── js/
│       └── app.js                # Frontend JavaScript
├── templates/                    # HTML templates
│   └── index.html                # Main application page
├── utils/                        # Utility functions
│   └── helpers.py                # Helper utilities
├── evaluator.py                  # LLM evaluation module
├── generate_report.py            # Evaluation report generator
├── LICENSE                       # Apache 2.0 license
├── main.py                       # Application entry point
├── README.md                     # Project documentation
├── requirements.txt              # Python dependencies
└── run_evaluation.py             # Evaluation pipeline script
```

## Installation and Setup

1. Clone the repository and set up your Python environment:

```bash
# Create and activate virtual environment (optional)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

2. Create a `.env` file with your API keys:

```
OPENAI_API_KEY=your_openai_key
ANTHROPIC_API_KEY=your_anthropic_key
WEATHER_API_KEY=your_weather_api_key
MAPS_API_KEY=your_maps_api_key
FIRECRAWL_API_KEY=your_firecrawl_key
```

3. Run the application:

```bash
python main.py
```

For web interface:
```bash
python main.py --config config/config.yaml
```

For CLI mode:
```bash
python main.py --config config/config.yaml --cli
```

## Usage

### Web Interface

Once the application is running, access the web interface at `http://localhost:8000` (or the configured host/port). 

1. Enter your travel plans in the text input field, e.g., "Help me plan a 3-day trip to Chicago with museums, parks, and local food"
2. Click "Create Travel Plan" to generate your personalized itinerary
3. View your results in the tabbed interface:
   - **Itinerary**: Day-by-day travel plan
   - **Packing List**: Recommendations for what to bring
   - **Budget**: Estimated costs for your trip
4. Click "Download Itinerary Calendar" to export your plans to an ICS file

### CLI Mode

When running in CLI mode:

1. Enter your travel plans when prompted
2. View your itinerary, packing list, and budget estimate in the console
3. Type 'exit' to quit

## LLM Evaluation System

NoDetours includes a comprehensive evaluation system to compare different LLM providers for travel planning:

### Evaluation Features

1. Tests multiple LLM providers (OpenAI GPT-3.5/4, Anthropic Claude, etc.)
2. Evaluates performance on various metrics (accuracy, relevance, completeness, usefulness, creativity)
3. Uses a "judge" LLM to rate responses 
4. Generates detailed reports and visualizations

### Running the Evaluation Pipeline

To run the full evaluation pipeline:

```bash
python run_evaluation.py --config config/eval_config.yaml --data eval-data/travel_assistant_data.json --sample-size 10
```

Parameters:
- `--config`: Path to the configuration file (default: config.yaml)
- `--data`: Path to the test data file (default: eval-data/feature_extractor_data.json)
- `--sample-size`: Number of test cases to sample (optional)
- `--output-dir`: Base directory for evaluation outputs (default: evaluation_runs)
- `--skip-evaluation`: Skip evaluation and use existing results file
- `--results-file`: Path to existing results file (if skipping evaluation)

### Generating Reports from Existing Results

If you already have evaluation results and just want to generate reports:

```bash
python run_evaluation.py --skip-evaluation --results-file path/to/evaluation_results.json
```

## Evaluation Metrics

The system evaluates travel plans on these metrics (scale 1-10):

- **Accuracy**: How accurately the plan addresses user requirements
- **Relevance**: How relevant the recommendations are to user preferences
- **Completeness**: How comprehensive and detailed the plan is
- **Usefulness**: How practical and helpful the information would be
- **Creativity**: How innovative and personalized the suggestions are

## Configuration

You can modify the `config.yaml` file to:

1. Add or remove LLM providers
2. Change model parameters (temperature, max tokens)
3. Configure evaluation metrics
4. Set up API providers for weather, maps, and search

Example configuration for an LLM provider:

```yaml
llm_providers:
  openai_gpt4:
    provider: "openai"
    model: "gpt-4"
    temperature: 0.7
    max_tokens: 4000
```

## Extending the System

### Adding a New LLM Provider

To add support for a new LLM provider:

1. Add the provider configuration to `config.yaml`
2. Ensure the LLMProvider class in `api/llm_provider.py` supports the new provider
3. Add the necessary API key to your `.env` file

### Adding New API Integrations

To integrate a new external service:

1. Create a new wrapper class in the `api/` directory
2. Update the `config.yaml` file with the new provider
3. Modify the `context_collector.py` to use the new service

## License

This project is licensed under the Apache License 2.0 - see the LICENSE file for details.
