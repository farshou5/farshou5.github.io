# NEURAL STAIN REMAKE — RESUME AFTER RESTART
_Last updated 2026-08-25 12:30. Read this first; everything needed is here._

## One-line status
All 20 takes exist. A second pass ("the locked run") is re-rendering them with
the CHARACTER LOCK so the father and son stop changing face between takes.

## How to restart the work after a reboot

1. **Relaunch the automation browser** (it is NOT a normal Chrome window):
   ```
   "C:\Program Files\Google\Chrome\Application\chrome.exe" ^
     --remote-debugging-port=9222 ^
     --user-data-dir=C:\Users\MSI\chrome-auto ^
     https://higgsfield.ai/ai/video
   ```
   Wait for the page to fully load. He stays logged in; no captcha so far.

2. **Resume the run** — it skips whatever is already done:
   ```
   cd C:\Users\MSI\Documents\Claude\uncounted\tools
   python -u hf_pipe.py 4 5 6 7 8 9 10 11 17 18 19 20 > ..\remake_v2\resume.log 2>&1
   ```
   `remake_v2\state.json` records each take's status, so a crash loses nothing.

## Hard-won facts — do not relearn these
- **Model: Seedance 2.5. Unlimited mode ON, always.** His standing order.
  The guard in `hf_gen.submit` refuses to press Generate unless the button
  reads "Generate Unlimited" (no digits) or ends in 0.
- **Never attach reference images.** It silently turns Unlimited OFF and asks
  ~154 credits, and the upload is rejected as "Protected content".
  Consistency comes from the TEXT lock only.
- **Finished clips come back BLURRED** behind "Rights verification required".
  `hf_pipe.clear_rights()` clicks "I own rights to this content" then
  "I confirm". He knows and has not objected.
- **Long takes work** (28/29/30 s all rendered). Failures are transient —
  retry, don't shorten. Retry plan is `[dur, dur, dur, 8]`.
- **The bug that ruined pass 1:** `hf_gen.take_prompt()` dropped the global
  header rules, so DEN STAGING / CONTINUITY / weight split were written to
  FINAL-TAKES.md but never sent. `global_rules()` now prepends them. The rule
  bullets must stay ABOVE the `---` separator in FINAL-TAKES.md.
  **Always assert the rule text is in the prompt before starting a batch.**
- **The site moved:** workspace is `/ai/video`; `/create/video` redirects to a
  marketing page with no editor. Two sliders exist — the duration one is the
  one with `aria-valuemax > 10`.

## His corrections, all now permanent in FINAL-TAKES.md
1. SON is only ~10% overweight in the present-day den — a small soft belly,
   fuller face, NOT fat. He is heavy (~40%) ONLY in the future/roots vision.
2. DEN STAGING: the couch faces the TV; the TV is OFF-SCREEN BEHIND THE
   CAMERA. Never a screen on the wall behind the couch (he caught this).
3. Junk food visible on the coffee table: open pizza box, burger in a
   wrapper, donut on a plate, soda can, torn bag of chips.
4. CONTINUITY: a ketchup smear / crumb / stain must persist through the whole
   take and every cut, and the table food returns identical. He caught stains
   appearing and disappearing.
5. CHARACTER LOCK: both men pinned to exactly how they look in take 02 — son
   with a thick full dark-brown beard (never clean-shaven, never blond, never
   a teenager), father silver-white haired with a moustache (never dark).
6. The last word of a line must never be clipped — speech ends at least one
   second before the clip does.

## Delivery
- **ADB is the delivery channel**, never Telegram attachments.
- Destination album: **`/sdcard/DCIM/AIVideos`** (was Camera, then NeuralStain).
- `touch remake_v2\NO_PUSH` = hold clips on the PC and queue them in
  `pending.txt` instead of pushing. That flag is currently SET.
- Device: `192.168.50.122:5555`. If "device not found", `adb connect` first
  and address it as `-t 1`. Bash needs `export MSYS_NO_PATHCONV=1` or the
  /sdcard path gets mangled into `C:/Program Files/Git/sdcard`.

## Known outages (2026-08-25)
- **Telegram outbound was blocked** ~05:37–09:30: `api.telegram.org`
  unreachable even by IP while the rest of the internet worked. Roughly ten
  replies failed. It recovered on its own. If it happens again, ADB is the
  fallback — push a status .txt to the AIVideos album.
- **His photos arrive empty** ("(photo)" with no file, nothing in the inbox
  folder) since ~20:00 on 2026-08-24. Ask him to send images as a
  File / document instead.

## Files
```
uncounted\FINAL-TAKES.md          canonical prompts + the rule bullets
uncounted\build_final.py          rebuilds the public page from it
uncounted\tools\hf_gen.py         prompt parsing, rules injection, submit
uncounted\tools\hf_drive.py       submit + read the result from the DOM
uncounted\tools\hf_pipe.py        full pipeline + state + rights clearing
uncounted\tools\hf_run.py         adb push helper (DEST_DIR, DEV)
uncounted\remake_v2\              the clips, logs, state.json
uncounted\remake_v2\v1_nolock\    pass-1 clips, kept for comparison
uncounted\remake_v2\NEURALSTAIN_REMAKE_v1.mp4   7:41 assembly of pass 1
```
Public page: https://farshou5.github.io/neural-stain/prompts-final.html

## Next steps after the locked run finishes
1. Rebuild the assembly with `ffmpeg -f concat` (all clips are h264 1280x720
   24 fps + aac 32 kHz, so `-c copy` works).
2. Push the set to the AIVideos album when he asks.
3. Get his per-take verdict, redo what he rejects.
4. Then the voices — the emotion in the render must match the emotion in the
   line, which is his standing rule.
