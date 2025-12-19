# 🔍 Troubleshooting Edge-TTS

## Current Status

Edge-TTS works in standalone tests but not when Jarvis runs.

## What to Check

When you start Jarvis, look for these messages in the terminal:

### ✅ **If Edge-TTS is working:**
```
[TTS] Edge-TTS initialized with voice: en-US-AvaNeural
[Edge-TTS] Generating speech with voice: en-US-AvaNeural
[Edge-TTS] Speech generated successfully
```

### ❌ **If Edge-TTS is failing:**
```
[TTS] Edge-TTS initialized with voice: en-US-AvaNeural
[Edge-TTS] Generating speech with voice: en-US-AvaNeural
[Edge-TTS] ERROR: <error message>
[Edge-TTS] Falling back to system TTS
```

### ⚠️ **If Edge-TTS isn't being used at all:**
```
(No Edge-TTS messages at all)
```

## Next Steps

1. **Start Jarvis** using desktop shortcut
2. **Ask a question**
3. **Copy the terminal output** and share it
4. This will show exactly what's failing

## Possible Issues

1. **Import Error**: edge-tts module not found
2. **Encoding Error**: Unicode issues (should be fixed now)
3. **Pygame Error**: Audio playback failing
4. **Network Error**: Can't reach Microsoft servers
5. **Config Error**: Wrong engine name or voice

## Quick Test

Run this to verify Edge-TTS works standalone:
```powershell
$env:PYTHONPATH = "C:\Users\cb\Documents\GitHub\jarvis\src"
.\.mamba_env\python.exe -c "import asyncio; import edge_tts; asyncio.run(edge_tts.Communicate('Test', 'en-US-AvaNeural').save('test.mp3')); print('OK')"
```

If this works but Jarvis doesn't, there's an integration issue.

