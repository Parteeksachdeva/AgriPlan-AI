# AgriPlan AI - Intelligent Crop Planning Platform

## 💡 The Idea

AgriPlan AI helps Indian farmers decide **what crop to grow next** by combining the power of machine learning and conversational AI. Instead of relying on guesswork or outdated advice, farmers get data-driven recommendations that consider their specific situation—soil type, previous crops, local weather, market prices, and government subsidies.

## 🌾 The Problem

Indian farmers face a critical decision every planting season: **which crop will be most profitable?** This choice involves juggling multiple complex factors:
- Market prices fluctuate unpredictably
- Government MSP (Minimum Support Price) changes each year
- Crop rotation affects soil health
- Weather patterns are increasingly uncertain
- Subsidy schemes vary by state and crop

Making the wrong choice can mean financial loss for the entire season. Currently, farmers rely on word-of-mouth, intuition, or one-size-fits-all advice that doesn't account for their specific farm conditions.

## ✨ Our Solution

AgriPlan AI is a conversational AI platform that delivers personalized crop recommendations in simple language. Farmers can either:
- **Fill a quick farm profile** (location, soil type, land size, previous crop)
- **Ask questions naturally** like *"I grew rice in Pune last kharif, what should I grow this rabi?"*

The system then:
1. Analyzes their farm conditions using ML models trained on historical Indian agricultural data
2. Factors in current market prices from government sources (agmarknet.gov.in)
3. Identifies applicable government subsidies and MSP rates
4. Recommends the **top 5 most profitable crops** with clear explanations
5. Explains **why** each crop is suitable in farmer-friendly language using AI

## 🎯 Key Features

**1. Multi-Factor Recommendations**
- Considers soil type, weather, market prices, subsidies, and crop rotation together
- Ranks crops by profitability while accounting for risk

**2. Conversational Interface** 🗣️
- Ask questions in plain language (Hindi/English)
- No forms to fill—just chat naturally
- Get instant, context-aware answers

**3. Government Subsidy Integration** 💰
- Automatically checks MSP rates for each crop
- Identifies applicable schemes (PMFBY, state subsidies)
- Shows guaranteed vs. market-only crops

**4. Smart Crop Rotation** 🔄
- Prevents soil depletion from repeated crops
- Recommends beneficial rotation sequences
- Explains why certain crops pair well together

**5. Plain Language Explanations** 📖
- AI explains recommendations in simple terms
- No technical jargon—accessible to all farmers
- Answers follow-up questions naturally

## 🛠️ Technology Stack

**Backend:**
- Python FastAPI (REST API)
- XGBoost (crop yield prediction & recommendation)
- Anthropic Claude API (natural language explanations)
- LangChain (conversational interface)

**Data:**
- Historical crop yields (data.gov.in)
- Market prices (agmarknet.gov.in)
- Weather data (IMD historical data)
- Government subsidy database (curated)

**Frontend:**
- React dashboard
- Interactive chat interface
- Visual comparison cards

## 📊 How It Works

```
Farmer Input → Feature Engineering → ML Models → Subsidy Lookup → LLM Explanation → Recommendations
     ↓              ↓                    ↓              ↓                ↓                ↓
"What to grow   Combine soil,      Predict yield   Check MSP &     Generate clear    Top 5 crops
 after rice?"   weather, prices    & rank crops    schemes         explanations       with reasons
```

## 🎬 Example Interaction

**Farmer:** "I have 5 hectares of black soil in Pune. I grew rice last season. What should I plant this rabi?"

**AgriPlan AI:** "Great question! After rice in Pune, **wheat** is your best rabi option. Here's why:

1. **Rotation benefit**: Wheat after rice is excellent—rice enriches the soil with organic matter that wheat thrives on.
2. **MSP guarantee**: Wheat has MSP of ₹2,275/quintal this season, protecting your price.
3. **Your soil**: Black soil with good pH is ideal for wheat.

You can expect around **4.2 tons/hectare**, translating to roughly **₹1.4 lakhs income** on your 5 hectares.

Your other top options are chickpea and mustard. Want to compare?"

## 🎯 Target Users

**Primary:** Small to medium farmers (5-50 hectares) in India
**Secondary:** Agricultural cooperatives and advisors managing multiple farms

## 🚀 MVP Timeline (2 Weeks)

**Week 1:**
- Train ML models on Indian crop data
- Build conversational interface with Claude
- Set up API endpoints

**Week 2:**
- Integrate subsidy database
- Build frontend dashboard
- Testing and demo preparation

## 💪 Why This Wins

✅ **Immediately useful** - Addresses real farmer pain point  
✅ **Modern AI** - Showcases LLM + ML hybrid approach  
✅ **Conversational UX** - Natural, accessible interface  
✅ **India-specific** - MSP, state schemes, mandi prices  
✅ **Achievable scope** - Working demo in 2 weeks  
✅ **Scalable** - Can add more crops, regions, languages  

## 🌟 Future Vision

- **Mobile app** for field use
- **Multi-language support** (Hindi, Telugu, Marathi, etc.)
- **IoT integration** (soil sensors, weather stations)
- **Live mandi price feeds** via government APIs
- **Community features** (farmers share experiences)

---

**Team Size:** 4 developers  
**Duration:** 2 weeks  
**Target:** Hackathon MVP → Real-world pilot with farmer cooperative
