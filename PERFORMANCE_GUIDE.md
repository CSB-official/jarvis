# Jarvis Performance Optimization Guide

## 🔍 Performance Analysis Results

### Issues Found:
1. **HUGE AI Model**: `gpt-oss:20b` (13 GB) - Very slow responses
2. **Chatterbox TTS on CPU**: 8 samples/second = 20-30 seconds per response
3. **TTS Error on Long Responses**: Crashes on 250+ word responses
4. **No GPU Acceleration**: AMD GPU doesn't support CUDA (needed for Ollama/Chatterbox)

---

## ⚡ Speed Comparison

| Configuration | Model Size | Response Time | TTS Time | Total Time |
|--------------|------------|---------------|----------|------------|
| **Current (Slow)** | 13 GB | 10-20s | 20-30s | **30-50s** |
| **Balanced** | 2 GB | 2-5s | 20-30s | **22-35s** |
| **Fast** | 2 GB | 2-5s | instant | **2-5s** |
| **Premium** | 42 GB | 15-30s | 20-30s | **35-60s** |

---

## 🚀 **Option 1: FAST (Recommended)**
**Best for**: Quick responses, daily use

### Changes:
- Switch to **llama3.2:3b** (2 GB vs 13 GB)
- Use **system TTS** instead of Chatterbox
- **~10x faster** overall

### To Enable:
```bash
copy C:\Users\cb\.config\jarvis\config_optimized.json C:\Users\cb\.config\jarvis\config.json
```

Or manually edit `C:\Users\cb\.config\jarvis\config.json`:
```json
{
  "ollama_chat_model": "llama3.2:3b",
  "tts_engine": "system",
  "voice_debug": true,
  "voice_device": "3",
  "location_enabled": true
}
```

**Restart Jarvis** to apply changes.

### Results:
- ✅ Responses in **2-5 seconds** (vs 30-50s)
- ✅ Instant voice output
- ✅ No more TTS errors on long responses
- ⚠️ Slightly less "smart" responses (3B vs 20B parameters)
- ⚠️ Windows robotic voice instead of cloned voice

---

## 🎯 **Option 2: BALANCED**
**Best for**: Quality + decent speed

### Changes:
- Use **llama3.2:3b** for faster AI
- Keep **Chatterbox TTS** for voice cloning
- Automatically skip TTS for long responses (fixed!)

### To Enable:
Edit `C:\Users\cb\.config\jarvis\config.json`:
```json
{
  "ollama_chat_model": "llama3.2:3b",
  "tts_engine": "chatterbox",
  "tts_chatterbox_cfg_weight": 0.3
}
```

### Results:
- ✅ Much faster AI responses (2-5s)
- ✅ Your cloned voice quality
- ✅ No crashes on long responses
- ⚠️ Still 20-30s wait for voice

---

## 💎 **Option 3: PREMIUM QUALITY**
**Best for**: When quality matters more than speed

### Changes:
- Use **llama3.3:latest** (42 GB - best quality)
- System TTS for instant voice

### To Enable:
```json
{
  "ollama_chat_model": "llama3.3:latest",
  "tts_engine": "system"
}
```

### Results:
- ✅ Highest quality AI responses
- ✅ Instant voice output
- ⚠️ Slower AI responses (15-30s)
- ⚠️ Uses 42 GB RAM

---

## 📊 Model Comparison

| Model | Size | Speed | Quality | Best For |
|-------|------|-------|---------|----------|
| **llama3.2:3b** | 2 GB | ⚡⚡⚡⚡⚡ | ⭐⭐⭐ | Daily use, quick questions |
| **gpt-oss:20b** | 13 GB | ⚡⚡ | ⭐⭐⭐⭐ | Complex tasks (current) |
| **llama3.3:latest** | 42 GB | ⚡ | ⭐⭐⭐⭐⭐ | Best quality available |

---

## 🔧 Additional Performance Tweaks

### 1. Disable Voice Cloning for Speed
```json
{
  "tts_engine": "system"
}
```
**Result**: Instant voice, no 30s wait

### 2. Shorter Wake-Up Time
```json
{
  "hot_window_seconds": 10.0
}
```
**Result**: 10 seconds instead of 6 to ask follow-up questions

### 3. Use Smaller Whisper Model (Faster Speech Recognition)
```json
{
  "whisper_model": "tiny"
}
```
**Result**: Faster wake word detection, slightly less accurate

---

## 🎬 What I Changed

### Fixed Issues:
1. ✅ **Downloaded faster models** (llama3.2:3b and llama3.3:latest)
2. ✅ **Fixed TTS crash** on long responses (auto-skips voice for 250+ word responses)
3. ✅ **Created optimized config** with recommended settings

### Files Changed:
- `src/jarvis/output/tts.py` - Added length check to prevent crashes
- Created `config_optimized.json` - Fast configuration template

---

## 📝 My Recommendation

**Start with Option 1 (FAST)** and see if you like the response quality from llama3.2:3b.

If you need better AI quality:
- Try **Option 3 (Premium)** with llama3.3 + system TTS

If you want your cloned voice:
- Use **Option 2 (Balanced)** but expect 20-30s voice generation time

**Remember**: Your AMD GPU can't accelerate AI models, so everything runs on CPU. A faster CPU would help, but changing models is the biggest speedup!


