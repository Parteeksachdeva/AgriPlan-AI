# Requirements Document: AgriPlan AI — Intelligent Crop Planning Platform

## 1. Introduction

AgriPlan AI is an AI-driven agricultural decision-support platform that helps farmers decide what crop to grow next. The system analyzes multiple real-world factors — current market prices, government subsidies, land conditions, previous crops grown, weather patterns, locality, and regional demand — and synthesizes them into actionable, personalized recommendations.

The platform uses different AI technologies for different purposes: machine learning models for yield prediction and crop scoring, and large language models for natural language explanations and conversational interaction.

### 1.1 Purpose

Empower farmers with data-driven crop selection decisions by combining ML-based predictions with real-world agricultural, economic, and environmental factors — presented through an intuitive, conversational interface.

### 1.2 Scope

**MVP (1-2 week sprint, team of 4):**

- ML models for yield prediction and multi-factor crop recommendation (Scikit-learn / XGBoost)
- Decision factors: market prices, government subsidies (MSP, schemes), soil/land conditions, previous crop history, weather, locality, demand
- LLM-powered natural language explanations and reasoning
- Conversational query interface for farmers (LangChain)
- Government subsidy and scheme information integration
- Simple model persistence (file-based, joblib)
- Feature engineering pipeline covering all decision factors
- Indian agriculture context as primary target (extensible to other regions)

**Future scope:**

- IoT sensor integration for real-time land condition monitoring (soil moisture, nutrients, pH)
- Deep learning models (TensorFlow/PyTorch) for weather pattern analysis
- Real-time satellite imagery for crop health monitoring
- Mobile application for field use
- Production ML infrastructure (MLflow, model registry, automated retraining)
- Multi-language support (Hindi, regional languages)
- Mandi (market) price live feeds via government APIs

### 1.3 Team & Timeline

| Week                   | Focus                                                        | Owner(s)      |
| ---------------------- | ------------------------------------------------------------ | ------------- |
| **Week 1 (Days 1-3)**  | Project setup, data collection, feature engineering pipeline | Dev 1 + Dev 2 |
| **Week 1 (Days 1-3)**  | LLM integration scaffold, prompt templates, subsidy data     | Dev 3         |
| **Week 1 (Days 1-3)**  | API endpoints, UI wireframe, farm profile forms              | Dev 4         |
| **Week 1 (Days 4-5)**  | Yield predictor + crop recommender model training            | Dev 1 + Dev 2 |
| **Week 1 (Days 4-5)**  | Conversational interface (LangChain), subsidy lookup tool    | Dev 3         |
| **Week 1 (Days 4-5)**  | Frontend integration, dashboard, recommendation cards        | Dev 4         |
| **Week 2 (Days 6-8)**  | End-to-end integration, fallback logic, data validation      | All           |
| **Week 2 (Days 9-10)** | Testing, bug fixes, demo polish, documentation               | All           |

### 1.4 Tech Stack

- **Backend**: Python 3.11+, FastAPI
- **ML**: Scikit-learn, XGBoost, pandas, NumPy
- **LLM**: Anthropic Claude API (via `anthropic` SDK)
- **Orchestration**: LangChain
- **Data**: CSV/Parquet files (Indian agricultural datasets, government subsidy data)
- **Model Storage**: joblib serialization to local filesystem
- **Database**: SQLite (dev) or PostgreSQL
- **Frontend**: Streamlit or React

## 2. Glossary

- **Yield_Predictor**: ML component that forecasts crop yields based on historical, environmental, and land data
- **Crop_Recommender**: ML component that ranks crops by predicted profitability considering market, subsidies, land, weather, and demand
- **LLM_Explainer**: LLM component that generates farmer-friendly explanations incorporating subsidy info and local context
- **Conversational_Interface**: Natural language query system powered by LangChain that lets farmers ask questions in plain language
- **Subsidy_Lookup**: Data service that retrieves applicable government schemes, MSP rates, and incentives for a crop/region
- **Baseline_Model**: Existing statistical method (SciPy optimization, Monte Carlo) used as fallback
- **MSP**: Minimum Support Price — government-guaranteed price for certain crops
- **Farmer**: End user of the AgriPlan AI platform

## 3. Requirements

### 3.1 Multi-Factor Crop Recommendation

**User Story:** As a farmer, I want crop recommendations that consider my land, local market, government subsidies, weather, and what I grew last season — so that I choose the most profitable and suitable crop.

**Priority:** P0 (Must have)

#### Acceptance Criteria

1. THE Crop_Recommender SHALL incorporate the following decision factors into its scoring model:
   - **Land conditions**: soil type, soil pH, soil quality score, available acreage
   - **Previous crop**: last crop grown (for crop rotation benefits/penalties)
   - **Market prices**: current mandi/market rates for candidate crops
   - **Government subsidies**: applicable MSP, subsidy schemes, and incentive programs
   - **Weather**: seasonal forecast, avg temperature, expected rainfall
   - **Locality**: state/district, agro-climatic zone
   - **Demand**: regional demand indicators for candidate crops
2. WHEN a farmer provides their farm profile, THE Crop_Recommender SHALL return a ranked list of top 5 crops with profitability scores
3. EACH recommendation SHALL include: estimated yield, estimated revenue (including subsidies), risk level, and the key factors driving that recommendation
4. THE Crop_Recommender SHALL penalize crops that are poor rotation choices after the farmer's previous crop
5. THE Crop_Recommender SHALL boost crops that have active government subsidy or MSP support

### 3.2 ML-Based Yield Prediction

**User Story:** As a farmer, I want accurate yield predictions for specific crops on my land — so that I can estimate revenue before planting.

**Priority:** P0 (Must have)

#### Acceptance Criteria

1. WHEN a crop type and farm parameters are provided, THE Yield_Predictor SHALL return a yield estimate (tons/hectare) with confidence intervals
2. THE Yield_Predictor SHALL use features including: soil quality, weather conditions, locality, planting season, historical yield averages, and fertilizer usage
3. THE Yield_Predictor SHALL support at least 5 major crops (e.g., rice, wheat, cotton, sugarcane, soybeans)
4. WHEN insufficient data is available, THE system SHALL fall back to the existing statistical baseline and indicate the fallback in the response
5. THE Yield_Predictor SHALL expose feature importance scores for each prediction

### 3.3 Government Subsidy & Scheme Integration

**User Story:** As a farmer, I want to know which government subsidies and support schemes apply to each recommended crop — so that I can factor in guaranteed prices and incentives.

**Priority:** P0 (Must have)

#### Acceptance Criteria

1. THE system SHALL maintain a dataset of government agricultural schemes including: MSP rates, crop insurance (PMFBY), input subsidies, and state-level incentive programs
2. WHEN generating crop recommendations, THE system SHALL include applicable subsidies and their estimated financial impact per crop
3. THE Subsidy_Lookup SHALL filter schemes by crop type, state/district, and farmer eligibility criteria
4. THE system SHALL clearly distinguish between MSP-backed crops and market-only crops in recommendations

### 3.4 LLM-Generated Natural Language Explanations

**User Story:** As a farmer, I want recommendations explained in clear, simple language — so that I understand why a crop is being suggested and what subsidies apply.

**Priority:** P0 (Must have)

#### Acceptance Criteria

1. WHEN a recommendation is generated, THE LLM_Explainer SHALL produce a farmer-friendly explanation using the Anthropic Claude API
2. THE explanation SHALL cover: why this crop suits the farmer's land, relevant market conditions, applicable subsidies, weather suitability, and rotation benefits
3. WHEN the LLM API is unavailable, THE LLM_Explainer SHALL fall back to template-based explanations
4. THE LLM_Explainer SHALL use configurable prompt templates loaded from files

### 3.5 Conversational Query Interface

**User Story:** As a farmer, I want to ask questions in plain language — like "What should I grow after rice?" or "Which crops have MSP this season?" — and get useful answers.

**Priority:** P1 (Should have)

#### Acceptance Criteria

1. WHEN a farmer submits a natural language query, THE Conversational_Interface SHALL parse the intent and route to the appropriate backend service (yield prediction, crop recommendation, subsidy lookup)
2. THE Conversational_Interface SHALL support queries about: crop recommendations, yield estimates, subsidy information, market prices, and crop rotation advice
3. WHEN a query is ambiguous, THE Conversational_Interface SHALL ask clarifying questions (e.g., "Which state is your farm in?")
4. THE Conversational_Interface SHALL maintain conversation context across multiple turns within a session

### 3.6 Feature Engineering Pipeline

**User Story:** As a developer, I want consistent feature engineering that combines all decision factors — so that models receive properly formatted inputs during both training and inference.

**Priority:** P0 (Must have)

#### Acceptance Criteria

1. THE feature pipeline SHALL combine and transform data from multiple sources: farm profile, weather data, market prices, subsidy data, and crop history
2. THE feature pipeline SHALL handle missing values using configurable strategies (mean imputation, forward fill, regional defaults)
3. THE feature pipeline SHALL validate feature schemas before passing data to models
4. THE feature pipeline SHALL encode categorical features (soil type, locality, crop type) consistently between training and inference

### 3.7 Fallback & Graceful Degradation

**User Story:** As a user, I want the system to always return useful results — even when ML models or external APIs are temporarily unavailable.

**Priority:** P0 (Must have)

#### Acceptance Criteria

1. WHEN ML models are unavailable or fail, THE system SHALL fall back to existing statistical methods automatically
2. WHEN the LLM API is unavailable, THE system SHALL use template-based explanations
3. THE system SHALL indicate in every response which method was used (ML, statistical baseline, or template)
4. WHEN both ML and statistical methods are available, THE system SHALL allow side-by-side comparison

### 3.8 API Endpoints

**User Story:** As a frontend developer, I want clean REST API endpoints — so that I can build the user interface.

**Priority:** P0 (Must have)

#### Acceptance Criteria

1. THE system SHALL expose `POST /api/v1/recommend/crops` accepting a farm profile (land, location, previous crop, season) and returning ranked recommendations with subsidy info
2. THE system SHALL expose `POST /api/v1/predict/yield` accepting crop type and farm parameters, returning yield estimate with confidence intervals
3. THE system SHALL expose `POST /api/v1/chat` accepting natural language queries and returning conversational responses
4. THE system SHALL expose `GET /api/v1/subsidies` accepting crop type and state, returning applicable government schemes
5. THE system SHALL expose `GET /api/v1/models/status` returning loaded models and system health
6. All endpoints SHALL return consistent JSON response formats with appropriate HTTP status codes

### 3.9 Dashboard & Visualization

**User Story:** As a farmer, I want a visual dashboard — so that I can see crop comparisons, yield charts, and subsidy breakdowns at a glance.

**Priority:** P1 (Should have)

#### Acceptance Criteria

1. THE UI SHALL display crop recommendations as comparison cards showing yield, revenue, subsidies, and risk
2. THE UI SHALL display yield predictions with confidence intervals as charts
3. THE UI SHALL include a chat panel for the conversational interface
4. THE UI SHALL include a farm profile form capturing: location (state/district), land size, soil type, previous crop, and irrigation availability

## 4. Non-Functional Requirements

### 4.1 Performance

- Crop recommendation responses SHALL return within 3 seconds (excluding LLM explanation)
- LLM explanations SHALL return within 10 seconds
- The system SHALL support at least 10 concurrent users

### 4.2 Reliability

- The system SHALL not crash when external APIs (Claude, weather) are unavailable
- All errors SHALL return structured JSON responses with fallback indicators

### 4.3 Data

- The system SHALL use publicly available Indian agricultural datasets (crop yields, MSP rates, soil data)
- Market price data SHALL be sourced from government open data portals or curated CSVs
- Subsidy/scheme data SHALL be maintained as a versioned JSON/CSV dataset

### 4.4 Developer Experience

- The project SHALL include a `README.md` with setup and run instructions
- The project SHALL be runnable with `docker-compose up` or equivalent
- Environment variables SHALL be documented in `.env.example`
