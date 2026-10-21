# Generate AI videos locally for free

*LocalCut AI is a free, open-source desktop app that turns one prompt into a finished video on your own graphics card. I made this 58-second film with it on my laptop. This is how you can make yours.*

![Ten seconds of One raindrop's journey: a golden drop, a mountain stream, mossy stones, a waterfall, the sea, rising mist and a storm cloud, with the film's own captions](https://raw.githubusercontent.com/hanzlamateen/medium/master/2.%20generate-ai-videos-locally-for-free/images/one-raindrops-journey.gif)

*Ten seconds of "One raindrop's journey", one shot from each of its eight scenes. The full film runs 58 seconds.*

This film started as one sentence:

> A 60-second nature film that follows one raindrop from a storm cloud, down a mountain stream and a waterfall, out to the sea, and back up into the clouds. Macro close-ups, golden light, calm narration.

From that, my laptop wrote a script and drew a picture for each of the eight scenes. It animated every picture into a moving clip, read the narration aloud, wrote a music bed, timed a caption to every spoken word and cut it all into an MP4. Nothing went to a cloud service. There was no account to make and no credit to spend. The graphics card was the 8 GB RTX 3080 in my laptop, a 2021 chip that's mid-range by today's standards.

I didn't leave it entirely alone. I asked for the script again once, trimmed five of the eight narration lines by hand and had two of the pictures drawn again, one of them twice. Everything else in the film is what the app made. From pressing Generate to the finished file took 1 hour 46 minutes.

Watch the full film, with sound:

https://youtu.be/hm0veJXzsj8

The app is LocalCut AI. It aims for CapCut's ease of use with ComfyUI's power underneath. You type one line and get a finished cut. When the cut isn't right, you can change any step of it. I started building it in July. Its first release, 0.1.0, came out on 20 October 2026 for Windows and Linux, open source under Apache-2.0. The rest of this post walks you through your first video, with the screenshots from the session that made this film.

## What happens when you press Generate

LocalCut splits a video into stages and gives each one to an open model running on your machine. On an 8 GB NVIDIA card, these are the models it picks:

- Qwen3 8B writes the script, in Ollama. The script is a screenplay split into scenes, each with its narration and a description of the shot.
- Stable Diffusion XL draws a keyframe for every scene, in ComfyUI.
- LTX-Video animates each keyframe into a few seconds of video, also in ComfyUI. On a 16 GB card the final pass can use Wan 2.2 instead.
- Kokoro reads the narration in one of 54 stock voices, which you can audition before you pick one.
- ACE-Step writes the music bed.
- faster-whisper listens to the narration and times every word for the captions.
- FFmpeg cuts it together. It lowers the music under the voice, burns in the captions and writes the MP4.

![The eight keyframes of the film in a grid: a storm cloud, a golden drop, a mountain stream, a drop on mossy stones, a waterfall, a drop on the sea, a drop in the mist and a towering cloud](https://raw.githubusercontent.com/hanzlamateen/medium/master/2.%20generate-ai-videos-locally-for-free/images/keyframes.jpg)

*The eight keyframes Stable Diffusion XL drew for the film. LTX-Video animated each one into its clip.*

Most of it renders twice. First comes a quick draft for you to judge. When you're happy with it, a final pass renders it again at full quality.

## Why run it on your own machine

A render costs you nothing, so you can redo a scene until it's right. Your prompts and everything made from them stay on your own machine. There's no account to make and the app sends no telemetry. It goes online only when you ask it to: to download a model or a program, or to check for an update. The models are open and the app's library has alternatives for most stages.

You pay in time and disk instead. The first setup downloads about 35 GB. After that, a minute of video takes about an hour and a half of rendering on a laptop GPU. If you'd rather pay for speed on one step, you can add your own key for Anthropic, OpenAI, Google or fal.ai and use a cloud model for only the steps you choose. Nothing in the pipeline needs one.

## What you need

- Windows 10 (version 1903 or later) or Windows 11, or Ubuntu 22.04 or later. Other x64 Linux distributions work if they have glibc 2.35 or newer. There's a macOS build for Apple silicon too, but it's unsigned and nobody has tried it yet.
- An NVIDIA graphics card with 8 GB of memory, which is what I test on. AMD and Intel cards get their own ComfyUI build, but nobody has tried those on real hardware yet. With no graphics card at all, Linux still renders on the processor, where a keyframe takes minutes instead of seconds. Windows has no such build, so everything ComfyUI would make comes out as a stand-in there.
- 16 GB of RAM.
- About 40 GB of free disk.
- Time for the first setup. On my 3.5 MB/s connection, everything took just under three hours to download.

## Make your first video

### 1. Download and install

Get the installer for your system from the [latest release](https://github.com/echem-labs/local-cut-ai/releases/latest):

- Windows: `LocalCut.AI.Setup.0.1.0.exe`. It installs for your user only and asks for no admin rights.
- Linux: `LocalCut.AI-0.1.0.AppImage`, or `localcut-desktop_0.1.0_amd64.deb` for Debian and Ubuntu.
- macOS: `LocalCut.AI-0.1.0-arm64.dmg`, untested.

The installers aren't code-signed yet, so Windows SmartScreen stops the installer the first time you run it. Choose More info, then Run anyway. On Linux, make the AppImage executable and run it, or install the deb:

```bash
chmod +x LocalCut.AI-0.1.0.AppImage
./LocalCut.AI-0.1.0.AppImage

# or, on Debian and Ubuntu
sudo apt install ./localcut-desktop_0.1.0_amd64.deb
```

The release carries a `SHA256SUMS.txt` as well. `sha256sum -c SHA256SUMS.txt --ignore-missing` checks whatever you downloaded against it.

### 2. Let it read your machine

The first launch opens a short setup that reads your hardware and picks a model for every stage to fit it. My laptop got "Every stage can run locally".

### 3. Set up the programs

LocalCut does its work in three free programs it doesn't ship itself: FFmpeg, Ollama and ComfyUI. On Windows and Linux, the Set up all three button downloads a pinned build of each one and checks its size and SHA-256 before keeping it in `~/.localcut/programs`. It needs no admin rights and writes nothing outside `~/.localcut`. From then on, LocalCut starts Ollama and ComfyUI when the app opens and stops them when it closes. If you already run any of the three, it finds yours and uses it.

![The Programs step with FFmpeg 8.1.3, Ollama 0.40.4 and ComfyUI 0.40.0 all set up by LocalCut and every stage of the pipeline marked ready](https://raw.githubusercontent.com/hanzlamateen/medium/master/2.%20generate-ai-videos-locally-for-free/images/programs-step.png)

*The Programs step after Set up all three. The row of dots at the top shows which stages can run.*

### 4. Pick the models

Next comes the model list, one per stage, already ticked. For my card that came to 25 GB of downloads, plus the 5 GB script model that arrives with Ollama's setup. Untick anything you don't want, or open the full library and swap one. The library also holds bigger models for bigger cards: FLUX.1 schnell for keyframes, Qwen3 14B for scripts and Wan 2.2 for clips.

![The Models step listing one model per stage with its size: qwen3-8b-q4, sdxl-base-1.0, ltx-video-0.9-i2v, kokoro-82m, ace-step-v1-3.5b and faster-whisper-base-en, above a button that reads Download and continue (25 GB)](https://raw.githubusercontent.com/hanzlamateen/medium/master/2.%20generate-ai-videos-locally-for-free/images/models-step.png)

*The models LocalCut picked for an 8 GB NVIDIA card.*

You don't have to wait for the downloads, because they keep going in the background. A stage whose model hasn't arrived yet comes out as a stand-in. Before every render, a check tells you which stages those are.

### 5. Describe the video

On the home screen, type your idea and set four things under it:

- The shape: 16:9 for YouTube, 9:16 for Shorts, Reels and TikTok, or square.
- The length: 30 seconds, 60 seconds, 2 minutes or your own.
- The look: Cinematic, Documentary, Animated, Anime, Watercolor or Retro film.
- The mode. Auto runs straight through to the MP4. Review steps stops for you at the script and again at the storyboard.

![The home screen with the raindrop prompt typed in, 16:9, 60 seconds, the Cinematic style and Review steps picked, above the quick tools](https://raw.githubusercontent.com/hanzlamateen/medium/master/2.%20generate-ai-videos-locally-for-free/images/home.png)

*The home screen. The quick tools under the prompt each make one thing (a script, a thumbnail, a voiceover, an image, a music track or a clip) with no project to set up.*

Keep Review steps, the default, for your first few videos. Stuck for an idea? "Start from a template" under the prompt has three to try. Then press Generate, or Ctrl+Enter.

### 6. Read the script

In Review steps, the app writes the script and waits for you. My first script came back as four 15-second scenes, too few pictures to carry a minute. So I typed a note into the prompt panel under the storyboard, scoped to the script:

> Make it 8 shorter scenes of about 7 seconds each, one macro close-up per scene in golden light: the drop forming in the storm cloud, falling, landing in a mountain stream, riding the stream over mossy stones, going over the waterfall, reaching the sea, rising as mist in the sun, and back into the storm cloud where it began. Keep the narration calm, about 160 words in all.

The rewrite took a little over a minute. I made the film with a build from the day before the release. The release itself asks the model for scenes of about seven seconds, so you may not need a note like mine.

The narration decides how long each scene runs, which makes the word count the film's length. My rewrite had 207 words, about 70 seconds. I trimmed five lines by hand to 173 words, which came out at 58 seconds. Each scene's narration is in its Details panel, under Voice. The Change voice button there lets you audition the stock voices.

You can ask for that kind of change in plain words too, but check the result. I first asked for "about 160 words in all" and the 8B model cut the script to 100. Ctrl+Z put it back.

![The Details panel for scene 8 on its Voice tab: the narration text, the voice brief "calm narration" and a Change voice button](https://raw.githubusercontent.com/hanzlamateen/medium/master/2.%20generate-ai-videos-locally-for-free/images/script-checkpoint.png)

*Scene 8's narration at the script checkpoint, in its Details panel.*

When the script reads right, press Approve script.

### 7. Approve the storyboard

Approving the script starts the keyframes, about 20 seconds each on my laptop. The narration and the music render alongside them in less time. Then the app stops again, at the storyboard.

![The storyboard checkpoint: eight keyframes waiting for approval, with the narration and the music already laid out on the timeline below](https://raw.githubusercontent.com/hanzlamateen/medium/master/2.%20generate-ai-videos-locally-for-free/images/storyboard-checkpoint.png)

*The storyboard checkpoint. The narration and music are already on the timeline.*

This is the cheap moment to fix a picture, because a clip can only animate what its keyframe shows. Two of mine missed. Scene 1 showed a field of droplets with no cloud. Scene 7 showed a teardrop sitting on wet ground with no mist. In a scene's Details panel, under Image, the New take button draws the picture again with a new seed. The Takes row below it switches back to any earlier take. Scene 1 took two more tries and scene 7 one.

![Two pairs of keyframes. Top: a field of droplets on a dark surface, then a grey cloud bank with one small drop at its centre. Bottom: a glassy teardrop on wet ground in the rain, then a single drop hovering over a misty surface](https://raw.githubusercontent.com/hanzlamateen/medium/master/2.%20generate-ai-videos-locally-for-free/images/keyframe-retakes.jpg)

*First takes on the left, the takes I kept on the right. Top: scene 1, the drop forming in the storm cloud. Bottom: scene 7, the drop rising as mist.*

The same panel can skip the generated picture altogether. "Use my photo instead" animates the clip from an image of your own.

Keep the keyframe in mind when you edit motion, too. If you change a scene's motion prompt, describe what its picture shows. A prompt that names something the picture doesn't have makes the video model morph the subject toward it.

### 8. Watch the drafts, then make the final video

Approving the storyboard renders a draft clip for every scene. Each took about two minutes on my laptop, at 768 by 448 pixels. The draft cut was ready 15 minutes after I approved.

A draft is for deciding, so look for the wrong subject, not for detail. Movement is harder to judge, because the final pass renders every clip again and the motion can come out differently. My draft of scene 2 lost its drop before the shot ended. The final clip kept it. When you're happy, press Create final video. It renders the pictures and clips again at full quality. On my laptop the final clips came out at 1152 by 672 and took about 7 minutes each. The whole final pass took 64 minutes.

![The Player view: the finished film in a large monitor, the storyboard of final scenes beside it, the timeline below and scene 6's camera movement open in the Details panel](https://raw.githubusercontent.com/hanzlamateen/medium/master/2.%20generate-ai-videos-locally-for-free/images/player.png)

*The finished film in the Player view, with scene 6's camera movement open in Details.*

### 9. Export it

The Download MP4 button saves the film. Publish kit, next to it, writes a title, a description and hashtags from your script and suggests a thumbnail. It uploads nothing: it's text for you to paste wherever you post. The project menu, behind the three dots at the top, holds the rest:

- captions burned into the picture, or as a separate .srt file
- whether the music drops under the voice and whether cuts land on the beat
- frame rate and resolution
- a handoff to a pro editor, as .otio for Premiere or DaVinci Resolve or as .fcpxml for Final Cut Pro, at the same length as the MP4

## Change anything after the first cut

A project has three views: Storyboard, Player and Flowchart. The Details panel sits beside each of them and edits any step. The Flowchart draws the project the way the engine sees it, every stage of every scene and what feeds it.

![The Flowchart view zoomed in on scene 1: music, keyframe and narration nodes wired into clip nodes, with scene 1's keyframe selected and the Details panel showing its three takes, take 2 in use](https://raw.githubusercontent.com/hanzlamateen/medium/master/2.%20generate-ai-videos-locally-for-free/images/flowchart.png)

*The Flowchart view of the same project, 29 nodes in all, zoomed in on scene 1's keyframe. Details shows its three takes, with take 2 in use.*

The prompt panel takes edits in plain words, for the whole video or for one scene: "make scene 2 darker", "crossfade everything", "remove scene 3". The script model turns your request into an edit you confirm. Only the parts the edit touches render again. Every edit goes through the same checks in the engine, whichever view it came from. The project menu has undo and redo, plus save points.

## Better hardware, better models

Everything in this film came from a mid-range machine and the models picked to fit it: Stable Diffusion XL from 2023 and LTX-Video 0.9 from 2024. A current card with 16 GB or more gets more out of the same release. With 32 GB of RAM beside it, the first run offers Wan 2.2 for the final clips and Qwen3 14B for the script. The stages that run on the graphics card get faster too. I haven't had one to test on yet.

The open models have also moved on since 0.1.0's library was picked. Three whose weights came out between August and September look like the next step for their stages. All three have open weights that run in ComfyUI:

- MiniMax H3, also called Hailuo 3.0, makes 4 to 15 seconds of video with its own stereo sound, from text, a first frame or a first and last frame. It's a 33-billion-parameter model. The build ComfyUI recommends is a 19.5 GB file before its text encoder, so it needs far more than 8 GB.
- Qwen-Image 2.1 generates and edits images with 7 billion parameters, small enough for a consumer card. Its licence allows non-commercial use only.
- MiniMax Music 3 writes complete songs of up to five minutes, with vocals from your lyrics. Its model card says it fits in 24 GB at full precision, or on an 8 GB card by streaming its layers.

None of the three is in LocalCut's library yet. I haven't run them through it either. LocalCut can already import a ComfyUI workflow as the template for a stage (`localcut workflow import`), which is where to start if you have the card for them. The ComfyUI that LocalCut set up on my laptop was 0.40.0, released in October after all three models. Read each model's licence before you use what it makes.

I'll try LocalCut on a current gaming laptop with a 16 or 24 GB card and write that up separately.

## Remote engines and automation

The desktop app talks to its engine only over HTTP and WebSocket, so the engine doesn't have to be on the same machine. You can run it headless on a box with a big graphics card, using the Docker Compose file in the repository:

```bash
LOCALCUT_TOKEN="$(openssl rand -base64 32)" \
  LOCALCUT_ADVERTISE=gpu-box.local \
  docker compose -f deploy/docker-compose.yml up -d
```

The engine prints a pairing code. Paste it into Settings > Remote engine on your laptop and the app pins the box's certificate from then on. Close the laptop mid-render and the box keeps going.

From a source checkout, the `localcut` command drives a running engine. Its exit codes are made for scripts: 0 when it worked, 1 when the operation failed and 2 when it couldn't reach the engine.

```bash
cd engine
export LOCALCUT_TOKEN="$(openssl rand -hex 24)"
uv run localcut serve --backend local,mock   # leave this running

# in a second terminal, with the same LOCALCUT_TOKEN
uv run localcut create "how honeybees make honey"   # prints the project id
uv run localcut render <project-id>
uv run localcut export <project-id> --out honeybees.mp4
```

`localcut mcp` serves the same engine to MCP hosts such as Claude and goose. An agent can create, render, edit and export, but it can't put a cloud model on a project, so it can't spend your API keys.

## How it's built

The desktop app is Electron and React. The engine is Python. It holds each project as a graph and runs the renders from a job queue. It accepts a change only as a validated patch, which is how the canvas, the Details panel, the plain-words editor and an MCP agent all get the same checks. Each program download is pinned to one exact build and checked against its SHA-256. CI runs the engine's tests on Linux, macOS and Windows. Before it builds any installer, the packaging workflow renders a short video on Ubuntu 22.04.

## What 0.1.0 doesn't do yet

- The installers aren't signed. LocalCut AI has applied to SignPath Foundation for Windows signing.
- Nobody has tested the macOS build. It isn't signed or notarized. Nothing on a Mac sets itself up in one click either.
- Only NVIDIA cards have been tested.
- Cloning a real person's voice, which needs you to confirm you have their permission, works only in a build from source. The 54 stock voices work everywhere.
- It's slow. On an 8 GB laptop GPU, budget about an hour and a half of rendering for a minute of video.
- The 8B script model is uneven about length. Read the script before you approve it.

## Try it

LocalCut AI 0.1.0 is on the [release page](https://github.com/echem-labs/local-cut-ai/releases/latest). The source is on [GitHub](https://github.com/echem-labs/local-cut-ai). If it breaks on your machine, [open an issue](https://github.com/echem-labs/local-cut-ai/issues) and say which graphics card you have and what you saw. If it makes something you like, post the prompt in [Discussions](https://github.com/echem-labs/local-cut-ai/discussions), because I want to learn which prompts work for other people. And if you think more people should be able to make a video without renting a GPU, a star on the repo helps others find it.
