I started recording all my meetings back in 2020 — at least, that’s what my own [notes](/notes/video-recording/) say. Video is always more accurate than memory, and clicking "Start Recording" in [OBS](https://obsproject.com) is the cheapest way to not lose some random-but-important detail.

The downsides are obvious, though: you can't quickly grasp the gist of a meeting from a video, searching through it is basically impossible, it eats disk space like it's bulking season, and it's painfully easy to accidentally capture something private. To partially compensate, I used to jot down key points in bullet form during the meeting, and later either throw them into a task tracker or write a mini-summary for myself: who I met with, what we discussed, what decisions we landed on. If I forgot something or missed details, I'd double-check the video.

But this method isn't perfect either. Even a rough outline steals focus from the actual meeting. And some "oh right, that detail matters" moments only reveal themselves when it's already too late.

![Forgot](<forgot.jpeg>)

So I ended up with a better approach: extract the meeting audio from the video, turn it into text (with a neural net), then turn that transcript into a detailed meeting summary (with another neural net). Yes, it's neural nets all the way down.

The easiest way to rip the audio is with [ffmpeg](https://www.ffmpeg.org) (a command-line utility for working with audio/video). Here's an example (mono, 16 kHz sampling rate + loudness normalization):

<pre>
ffmpeg.exe -y -i "D:\video.mkv" -vn -ac 1 -ar 16000 -af loudnorm -c:a pcm_s16le "D:\audio.wav"
</pre>

As for speech-to-text: I experimented with [Vosk](https://alphacephei.com/vosk/) + [recasepunc](https://github.com/benob/recasepunc), but... Yeah, no. Let's just say I'd rather not relive that experience. Meanwhile <s>Boromir</s> [Whisper](https://openai.com/index/whisper) (OpenAI's speech recognition model) installs in the background in about 10 minutes:

<pre>
py -3.10 -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install setuptools wheel
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
pip install openai-whisper
</pre>

Example run:

<pre>
whisper "D:\audio.wav" --model medium --language Russian --output_format txt
</pre>

The result is a plain text transcript you can shove into any chatbot and get a fairly coherent summary. Sure, you still need to proofread it — fix mistakes and hallucinations, rephrase a couple things — but it's still way better than trying to write notes live.

And that’s basically the whole method. The only thing left is writing a simple script so you don’t have to run two commands manually every time. If you’re on Windows and you can't be bothered to vibe-code, you can grab [my script](https://gist.github.com/vkostyanetsky/4f4760097f1b417cc85d71d11662a642) and tweak it.

The script finds the first .mkv in its folder, runs it through ffmpeg + Whisper, and saves the result back into the same folder. If you use it: note that it runs via [CUDA](https://developer.nvidia.com/cuda) (CPU works too, just much slower), and it stores downloaded Whisper models not in the default cache, but inside the current Python virtual environment folder.

(if you really want, you can wire up an API call or point it at some local model via [LM Studio](https://lmstudio.ai) — but for my personal setup I decided: nope, that's already too much engineering for a lazy win)