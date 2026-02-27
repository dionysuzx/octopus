# Sync Call

Sync a call's video and transcript after the automated pipeline uploads assets to forkcast.

## 1. Wait for Assets

The cron job automatically uploads call assets (video, transcript, config) to forkcast. Wait for it to complete before proceeding.

## 2. Sync Video & Transcript

Livestreamed calls (acdc, acde, acdt) ship with `null` sync offsets because YouTube's live stream includes pre-roll (intro screens, countdowns) that the Zoom transcript doesn't have. You need to set them manually.

1. Open the call page on forkcast and play the YouTube video. Find the timestamp where the host first speaks (e.g., `00:03:55`).
2. Open `transcript_corrected.vtt` in `public/artifacts/{type}/{date}_{number}/`. Find the same moment (e.g., `00:06:55`).
3. Update `config.json` in the same directory:
   ```json
   {
     "sync": {
       "transcriptStartTime": "00:06:55",
       "videoStartTime": "00:03:55"
     }
   }
   ```
4. Click around the middle of the video and verify the transcript highlights the correct line. This confirms the sync is good.

Non-livestreamed calls default both offsets to `"00:00:00"` and don't need manual sync.

## 3. Push Changes

Commit and push. The deploy workflow handles the rest.

## 4. Post to Discord

Once the call is live on forkcast:

1. Go to **#allcoredevs** on the Ethereum R&D Discord.
2. Find the message posted by **acd-bot** for this call.
3. Open a thread on that message. Name the thread `ACD(CET) <num>` (e.g., `ACDE 231`).
4. In the thread, paste a link to the call on forkcast.
