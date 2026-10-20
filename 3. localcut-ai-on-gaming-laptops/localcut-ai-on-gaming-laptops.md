# Your gaming laptop can make AI videos now

Placeholder for a follow-up to [Generate AI videos locally for free](../2.%20generate-ai-videos-locally-for-free/generate-ai-videos-locally-for-free.md). The plan is to make the same film again on a current gaming laptop and show what LocalCut AI does with a bigger card. Nothing below has been tested yet.

Other titles to consider:

- I made the same AI film on a 2021 laptop and a 2026 one
- Your gaming laptop is a video studio

## The pitch

A gaming laptop already has what local AI video needs: an NVIDIA card with 12 to 24 GB, 32 GB of RAM and a fast SSD. The first article's film came from a 2021 RTX 3080 laptop chip with 8 GB. This one shows what a current machine changes: how long each stage takes, which models the first run offers and whether the newer open models fit.

## The test

- The same prompt and settings as article 2: 16:9, 60 seconds, Cinematic, Review steps, the default voice.
- A fresh install of the latest LocalCut AI release, Set up all three and the recommended models.
- Note what the first run picks for the card.
- Time every stage the way the raindrop run did: setup, script, keyframes, draft clips, final pass and Generate to MP4.
- Run it plugged in, then once on battery.
- Then try at least one newer model through `localcut workflow import`: MiniMax H3 for the clips or MiniMax Music 3 for the music, if the licence allows publishing the result.

## Numbers to beat

From article 2, on an RTX 3080 Laptop GPU with 8 GB:

- keyframes about 20 s each as drafts (SDXL), about 30 s at final quality
- draft clips about 2 min each at 768x448 (LTX-Video 0.9)
- final clips about 7 min each at 1152x672, 64 min for the whole final pass
- 1 h 46 min from Generate to the finished MP4, hand edits included
- 2 h 54 min for the first setup, about 35 GB at 3.5 MB/s

## Laptops to borrow or rent

Check the current lineup before writing. In the RTX 50 series the laptop GPUs are the 5090 with 24 GB, the 5080 with 16 GB, the 5070 Ti with 12 GB and the 5070 with 8 GB. The 16 and 24 GB machines are the interesting ones, because the first run offers Wan 2.2 from 16 GB.

## Open questions

- Does a laptop's power limit slow a long final pass compared to a desktop card with the same memory?
- What does MiniMax H3 need in practice on 24 GB? Does its own sound replace the ACE-Step bed or fight the narration?
- Which licences allow publishing the result? H3 has its own community licence, Qwen-Image 2.1 is non-commercial and Music 3 has a community licence with conditions.

## Images to make

- the same frame from both laptops, side by side
- a stage-by-stage timing chart
- the first run's machine and models steps on the new card
