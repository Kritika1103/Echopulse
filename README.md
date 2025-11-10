# Echopulse
AI powered voice automation
🧠 Project Overview: EchoPulse

Tech Stack:
- Node.js + Express for backend
- Twilio for telephony
- ElevenLabs for voice synthesis
- OpenAI (optional) for smart replies
- Cursor IDE for development

---

📁 Project Structure

`
echopulse/
├── .env
├── index.js
├── twilioClient.js
├── elevenLabsClient.js
├── utils/
│   └── audioHandler.js
├── package.json
`

---

📦 Step 1: Setup

`bash
mkdir echopulse && cd echopulse
npm init -y
npm install express axios dotenv twilio
`

---

🔐 Step 2: Environment Variables (.env)

`env
PORT=3000
TWILIOACCOUNTSID=yourtwiliosid
TWILIOAUTHTOKEN=yourtwiliotoken
TWILIOPHONENUMBER=yourtwilionumber
ELEVENLABSAPIKEY=yourelevenlabskey
AGENTID=youragent_id
AGENTPHONEID=youragentphone_id
`

---

🚀 Step 3: Main Server (index.js)

`js
const express = require('express');
const { makeOutboundCall } = require('./twilioClient');
require('dotenv').config();

const app = express();
app.use(express.json());

app.post('/call', async (req, res) => {
  const { to } = req.body;
  try {
    const result = await makeOutboundCall(to);
    res.json({ success: true, result });
  } catch (err) {
    res.status(500).json({ success: false, error: err.message });
  }
});

app.listen(process.env.PORT, () => {
  console.log(EchoPulse running on port ${process.env.PORT});
});
`

---

📞 Step 4: Twilio Client (twilioClient.js)

`js
const twilio = require('twilio');
const axios = require('axios');
require('dotenv').config();

const client = twilio(process.env.TWILIOACCOUNTSID, process.env.TWILIOAUTHTOKEN);

async function makeOutboundCall(toNumber) {
  const response = await axios.post('https://api.elevenlabs.io/v1/convai/twilio/outbound-call', {
    agentid: process.env.AGENTID,
    agentphonenumberid: process.env.AGENTPHONE_ID,
    to_number: toNumber
  }, {
    headers: {
      'xi-api-key': process.env.ELEVENLABSAPIKEY,
      'Content-Type': 'application/json'
    }
  });

  return response.data;
}

module.exports = { makeOutboundCall };
`

---

🧪 Step 5: Test the Call

Use Cursor IDE or Postman to send a POST request:

`http
POST /call
Content-Type: application/json

{
  "to": "+91xxxxxxxxxx"
}
`

---

✅ Features You Can Add

- Conversation memory using OpenAI
- Call status tracking
- Voice customization
- Multi-language support
