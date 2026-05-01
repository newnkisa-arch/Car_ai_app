# Car_ai_app
Car content pipeline 
!pip install transformers accelerate edge-tts requests pytrends
PEXELS_API_KEY = "HDm0qDCbBUaOGi8bH57dKgsDZWXyeCB3lQTolCdaIzmNGjOD4gykR42C"
from pytrends.request import TrendReq

pytrends = TrendReq()

keywords = ["car maintenance", "driving tips", "car accident", "SUV"]

pytrends.build_payload(keywords, timeframe='now 7-d')

data = pytrends.related_queries()

print(data)
from transformers import pipeline
import random

generator = pipeline("text-generation", model="microsoft/phi-2")

hooks = [
    "Most drivers don’t know this...",
    "This mistake can destroy your car...",
    "Stop doing this immediately...",
]

topic = random.choice(keywords)
hook = random.choice(hooks)

prompt = f"""
Create a viral short video script about {topic}.

Start with: {hook}

Use:
- simple English
- emotional tone
- short sentences

Format:
Hook:
Script:
Caption:
Hashtags:
"""

output = generator(prompt, max_length=200, temperature=0.9)
text = output[0]["generated_text"]

print(text)
import requests, urllib.request

headers = {"Authorization": PEXELS_API_KEY}

url = f"https://api.pexels.com/videos/search?query={topic}&per_page=3"

res = requests.get(url, headers=headers).json()

clips = []

for i, v in enumerate(res["videos"][:3]):
    link = v["video_files"][0]["link"]
    filename = f"clip{i}.mp4"
    urllib.request.urlretrieve(link, filename)
    clips.append(filename)
    with open("list.txt", "w") as f:
    for c in clips:
        f.write(f"file '{c}'\n")

!ffmpeg -f concat -safe 0 -i list.txt -c copy merged.mp4
import edge_tts, asyncio

async def voice():
    tts = edge_tts.Communicate(full_script, voice="en-US-GuyNeural")
    await tts.save("voice.mp3")

asyncio.run(voice())
def subtitles(text):
    lines = text.split()
    
    with open("sub.srt", "w") as f:
        for i in range(0, len(lines), 4):
            start = i//4 * 2
            end = start + 2
            line = " ".join(lines[i:i+4])
            
            f.write(f"{i}\n00:00:{start:02},000 --> 00:00:{end:02},000\n{line}\n\n")

subtitles(full_script)
ffmpeg -i merged.mp4 -i face.mp4 -i voice.mp3 -filter_complex \
"[1:v]scale=320:240[face]; \
[0:v][face]overlay=W-w-10:H-h-10:enable='between(t,2,5)+between(t,10,14)',subtitles=sub.srt" \
-c:v libx264 -c:a aac -shortest final.mp4
for i in range(5):
    print(f"Generating video {i}")
    # wrap steps into function and call
