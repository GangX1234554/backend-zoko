const express = require("express");
const axios = require("axios");
const app = express();
app.use(express.json());

const VOICEFLOW_API_KEY = "your_voiceflow_key";
const VOICEFLOW_PROJECT = "your_project_id";

app.post("/zoko-webhook", async (req, res) => {
  const incoming = req.body;

  const userId = incoming.phone; // user phone number
  const userMsg = incoming.message; // text sent by user

  try {
    // Send message to Voiceflow
    const response = await axios.post(
      `https://general-runtime.voiceflow.com/state/${userId}/interact`,
      {
        request: {
          type: "text",
          payload: userMsg
        }
      },
      {
        headers: {
          Authorization: `Bearer ${VOICEFLOW_API_KEY}`,
          "Content-Type": "application/json"
        }
      }
    );

    // Extract the reply from Voiceflow
    const reply = response.data?.[0]?.payload?.message;

    // Send the reply back to the user via Zoko
    await axios.post("https://api.zoko.io/v1/messages/send", {
      phone: userId,
      type: "text",
      text: reply
    }, {
      headers: {
        Authorization: `Bearer your_zoko_api_key`,
        "Content-Type": "application/json"
      }
    });

    res.sendStatus(200);
  } catch (err) {
    console.error("Error:", err.response?.data || err.message);
    res.sendStatus(500);
  }
});

app.listen(3000, () => console.log("Webhook running on port 3000"));
