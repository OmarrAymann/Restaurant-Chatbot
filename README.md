#  restaurant ai ordering agent  
**tool-driven conversational system using mcp + dspy react**

an interactive restaurant assistant that talks to customers, understands intent, manages orders, notifies the kitchen, and stores everything in structured records.  
the agent does not guess actions. it calls real tools exposed through the model context protocol.

---

## ✨ what makes this project different

- real tool execution via mcp  
- conversational ordering, not form-based  
- clear separation between reasoning and actions  
- email + excel integration out of the box  
- easy to extend with new tools  

no fragile prompts.  
no hidden logic.

---

## 🧠 system overview

this system is composed of two cooperating parts:

### 🤖 ai agent
- handles conversation
- understands customer intent
- decides which tool to call
- confirms actions with the user

### 🧩 mcp server
- exposes restaurant capabilities as tools
- executes real side effects
- stays stateless and predictable

---

## 🏗️ architecture

┌────────────────────────┐
│ resAgent.py 🤖 │
│ dspy react agent │
│ │
│ • chat with guest │
│ • intent detection │
│ • order reasoning │
└───────────┬────────────┘
│ mcp (stdio)
▼
┌────────────────────────┐
│ mcp_server_res.py 🧩 │
│ fast mcp server │
│ │
│ • fetch_menu │
│ • calculate_total │
│ • create_order │
│ • send_to_kitchen │
│ • save_in_excel │
└───────────┬────────────┘
│
┌────────┼─────────┐
▼ ▼ ▼
📧 email 📊 excel 🍔 menu
kitchen orders database


---

## 🍔 sample menu

| category | item | price |
|-------|------|------|
| appetizer | garlic bread | 6.50 |
| appetizer | spicy chicken bites | 11.75 |
| main | beef burger deluxe | 17.99 |
| main | shrimp alfredo | 21.50 |
| dessert | vanilla cheesecake | 7.25 |
| drink | iced peach tea | 3.99 |

menu lives in memory and is easy to extend.

---

## 🛠️ mcp tools

| tool | responsibility |
|----|---------------|
| fetch_menu | return menu items by category |
| calculate_total | compute subtotal tax and final total |
| create_order | generate structured order object |
| send_to_kitchen | notify kitchen by email |
| save_in_excel | persist order in excel file |

each tool has a single responsibility.

---

## 🗣️ agent abilities

- understands natural conversation  
- walks users through menu choices  
- supports order edits and special requests  
- collects guest details naturally  
- confirms before final submission  
- triggers real actions only when approved  

the agent never writes files or sends emails directly.  
tools do.

---

## 🚀 getting started

### requirements

- python 3.10+
- gmail account for kitchen notifications
- llm api key

---

### installation

pip install dspy mcp openpyxl pydantic python-dotenv


---

### environment setup

create a `.env` file:

GEMINI_API_KEY=your_api_key
SENDER_EMAIL=kitchen.bot@gmail.com
SENDER_PASSWORD=your_app_password


gmail requires app passwords with 2fa enabled.

---

## ▶️ run the chatbot

cd Resturant
python resAgent.py


you will enter an interactive chat session.

---

## 💬 example conversation

you: hey, can i see what you serve
agent: sure! we have appetizers, main dishes, desserts, and drinks.
would you like to start with something specific?

you: i want a burger and iced tea
agent: great choice 👍
beef burger deluxe 17.99
iced peach tea 3.99
may i have your name and table number?

you: lina, table 3, phone 0102223344
agent: perfect lina 👌
subtotal 21.98
tax 2.20
total 24.18
should i send this to the kitchen?

you: yes
agent: order confirmed 🍽️
the kitchen has been notified.


---

## 📁 project structure

Resturant/
├── mcp_server_res.py # restaurant tools
├── resAgent.py # conversational agent
├── orders.xlsx # generated order log
├── .env # environment variables
└── README.md


---

## 🧭 agent state machine

the agent tracks progress explicitly

| state | purpose |
|----|-------|
| greet | welcome message |
| view_menu | menu exploration |
| place_order | selecting items |
| modify_order | changing selections |
| provide_info | collecting guest data |
| confirm_order | final confirmation |
| finalized | order sent |
| cancel | flow aborted |

this prevents broken conversations.

---

## 📊 excel audit trail

each order is saved with:

- order id  
- timestamp  
- guest name  
- phone  
- table or location  
- ordered items  

simple. readable. auditable.

---

## 🔧 customization ideas

- add dietary filters  
- connect to a real database  
- integrate payment flow  
- add voice ordering  
- add multi-agent kitchen routing  

the mcp boundary makes this easy.

---

## 🧪 development notes

you can run the mcp server alone:

python mcp_server_res.py


you can swap llm providers inside `resAgent.py`:

- gemini
- claude
- openai

no architecture changes needed.

---
