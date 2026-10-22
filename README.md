# 🤖 WhatsApp AI Agent — Smart Customer Service Agent

An AI agent that replies to customers on WhatsApp automatically.  
Built with **LangGraph + Gemini (OpenRouter) + Twilio + FastAPI**

---

## ⚡ Full Setup in 30 Minutes

### Step 1 — Prerequisites (5 minutes)

Make sure you have:
- Python 3.11+
- A [Twilio](https://www.twilio.com/try-twilio) account (free)
- An [OpenRouter](https://openrouter.ai) account (free)
- [ngrok](https://ngrok.com/download) installed

---

### Step 2 — Project Setup (5 minutes)

```bash
# 1. Open the folder in VS Code
cd whatsapp_agent

# 2. Create a virtual environment
python -m venv venv

# 3. Activate the environment
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# 4. Install the dependencies
pip install -r requirements.txt
```

---

### Step 3 — Environment Variables (10 minutes)
Open the `.env` file and add the values:

#### 🔑 Twilio (free):
1. Go to [console.twilio.com](https://console.twilio.com)
2. From the home page, copy:
   - **Account SID** → `TWILIO_ACCOUNT_SID`
   - **Auth Token** → `TWILIO_AUTH_TOKEN`

#### 🤖 OpenRouter (free):
1. Go to [openrouter.ai](https://openrouter.ai)
2. Sign in → **API Keys** → **Create Key**
3. Copy the key → `OPENROUTER_API_KEY`

---

### Step 4 — Run the Server (2 minutes)

```bash
# In the first terminal:
python main.py
```

You should see:
```
🚀 Server running at: http://localhost:8000
📋 Swagger Docs: http://localhost:8000/docs
🔗 Webhook URL: http://localhost:8000/webhook
```

---

### Step 5 — ngrok for the Webhook (3 minutes)

```bash
# In the second terminal:
ngrok http 8000
```

You will see a message like:
```
Forwarding   https://abc123.ngrok-free.app -> http://localhost:8000
```

**Copy the URL** `https://abc123.ngrok-free.app`

---

### Step 6 — Twilio Sandbox Setup (5 minutes)

1. Go to [Twilio Console → Messaging → WhatsApp Sandbox](https://console.twilio.com/us1/develop/sms/settings/whatsapp-sandbox)
2. In the **"WHEN A MESSAGE COMES IN"** field, enter:
   ```
   https://abc123.ngrok-free.app/webhook
   ```
3. Click **Save**

#### Connect your phone to the Sandbox:
1. Open WhatsApp on your phone
2. Send the message shown in the Sandbox (for example: `join bright-elephant`)
3. To the number shown on the page
4. You will receive a confirmation message ✅

---

### ✅ Start Testing!

Now send any message from WhatsApp (the agent replies in Arabic):
- `Hello` or `مرحبا` → greeting + main menu
- `1` → browse products
- `2` → track orders
- `ORD-1001` → order details
- `ORD-1002` → order in shipping
- `3` → FAQs
- `ما سياسة الإرجاع؟` (What is the return policy?) → direct answer

---

## 📁 Project Structure

```
whatsapp_agent/
├── main.py                 # Entry point — FastAPI + Webhook
├── .env                    # 🔑 Environment variables template
├── requirements.txt        # 📦 Dependencies
│
├── agent/
│   ├── graph.py            # 🧠 LangGraph — builds the graph
│   ├── nodes.py            # ⚙️  Logic for each node
│   ├── state.py            # 📊 State definition
│   └── prompts.py          # 💬 System prompts
│
├── whatsapp/
│   ├── base.py             # 🔌 Abstract adapter
│   └── twilio_adapter.py   # 📱 Twilio implementation
│
├── tools/
│   └── store_tools.py      # 🛒 Store search tools
│
└── data/
    ├── products.json        # 🛍️ Product data
    ├── orders.json          # 📦 Order data
    └── faqs.json            # ❓ Frequently asked questions
```

---

## 🔄 Switching Platforms (Adapter Pattern)

To change the WhatsApp provider, change only one line in `main.py`:

```python
# Current:
whatsapp = TwilioAdapter()

# Future (for example):
whatsapp = GreenAPIAdapter()
whatsapp = MetaCloudAdapter()
```

---

## 📦 Demo Orders

| Order ID | Status           |
|----------|------------------|
| ORD-1001 | ✅ Delivered     |
| ORD-1002 | 🚚 In transit    |
| ORD-1003 | ⏳ Processing    |
| ORD-1004 | 📦 Shipped       |

---

## 🛠️ Troubleshooting

| Problem | Solution |
|---------|----------|
| The agent does not reply | Check that ngrok is running and the webhook is updated in Twilio |
| OpenRouter error | Make sure the API key is correct and the model is available for free |
| Twilio auth error | Check the Account SID and Auth Token |
| The phone does not receive messages | Resend the Sandbox join message |
