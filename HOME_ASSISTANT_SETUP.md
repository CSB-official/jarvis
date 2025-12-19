# Home Assistant Setup for Jarvis

## Prerequisites
Your Alexa and Govee devices need to be connected to Home Assistant first.

## Quick Setup Guide

### 1. Connect Your Devices to Home Assistant

**For Alexa devices:**
- Install the [Alexa Media Player integration](https://github.com/custom-components/alexa_media_player) in Home Assistant
- Follow the authentication flow to link your Amazon account

**For Govee devices:**
- Install the [Govee integration](https://www.home-assistant.io/integrations/govee/) in Home Assistant
- Enter your Govee API key from [Govee Developer Portal](https://developer.govee.com)

### 2. Install MCP Server Integration in Home Assistant

1. Go to **Settings → Devices & services**
2. Click **Add integration**
3. Search for **"Model Context Protocol Server"**
4. Install it

### 3. Expose Entities to Jarvis

1. Go to **Settings → Voice assistants → Exposed entities**
2. Select which devices you want Jarvis to control:
   - Your Alexa devices (for volume control, announcements)
   - Your Govee lights (on/off, brightness, color)
   - Any other smart devices

**Important:** Only expose what you want Jarvis to access!

### 4. Create a Long-Lived Access Token

1. Click your **profile icon** (bottom left avatar)
2. Scroll down to **Security**
3. Click **Create token**
4. Name it "Jarvis"
5. **Copy the token** (you can't see it again!)

### 5. Install the MCP Proxy

In your terminal (in the jarvis project):
```powershell
.\.mamba_env\Scripts\pip.exe install git+https://github.com/sparfenyuk/mcp-proxy
```

### 6. Update Jarvis Config

Edit `C:\Users\cb\.config\jarvis\config.json` and add:

```json
{
  "voice_debug": true,
  "voice_device": "3",
  "tts_engine": "chatterbox",
  "tts_chatterbox_audio_prompt": "C:/Users/cb/Documents/GitHub/jarvis/sydney_sweeney_has_great_jeans__american_eagle.wav",
  "tts_chatterbox_device": "cpu",
  "tts_chatterbox_exaggeration": 0.6,
  "location_enabled": true,
  "location_auto_detect": true,
  "web_search_enabled": true,
  "mcps": {
    "home_assistant": {
      "command": "mcp-proxy",
      "args": ["http://localhost:8123/mcp_server/sse"],
      "env": {
        "API_ACCESS_TOKEN": "YOUR_LONG_LIVED_TOKEN_HERE"
      }
    }
  }
}
```

Replace `YOUR_LONG_LIVED_TOKEN_HERE` with the token from step 4.

**Note:** If your Home Assistant is not on `localhost:8123`, change the URL.

### 7. Test It!

Restart Jarvis and try:
- **"Jarvis, turn on the living room lights"**
- **"Jarvis, set bedroom lights to 50%"**
- **"Jarvis, what's the temperature in the living room?"**
- **"Jarvis, turn off all the lights"**

## Troubleshooting

**404 Not Found:**
- The MCP Server integration isn't installed in Home Assistant (step 2)

**401 Unauthorized:**
- Token is invalid or expired - create a new one (step 4)

**No tools available:**
- No entities are exposed to Jarvis (step 3)
- Restart Jarvis after exposing entities

**Command not found `mcp-proxy`:**
- Re-run step 5
- Make sure the path is on your PATH or use full path in config

## Example Voice Commands

**Lights:**
- "Turn on the kitchen lights"
- "Set living room to 30%"
- "Make the bedroom lights blue"
- "Turn off all lights"

**Climate:**
- "What's the temperature?"
- "Set thermostat to 72 degrees"

**Scenes:**
- "Run the good night scene"
- "Activate movie mode"

