

## Capture ME screenshots (all 16 treatments)

From the workspace root (`S:\Personal\nozarim1\oTree`):

1. Activate the virtual environment (PowerShell):

```powershell
& .\env\Scripts\Activate.ps1
```

2. Install Playwright once (if not installed yet):

```powershell
pip install playwright
playwright install chromium
```

3. Start your oTree server (example):

```powershell
otree devserver
```

4. In a second terminal (venv activated), run the screenshot script:

```powershell
python .\scripts\capture_me_screenshots.py --base-url http://127.0.0.1:8000/ --app ME --out screenshots
```

Notes:
- The script now defaults to `--treatments 16`, so you do not need to pass it.
- By default, `BotScreening` is captured only once (treatment 1) to speed up runs.
- To capture every page in every treatment, pass: `--capture-once-labels ""`
- Add `--headed` if you want to see the browser while screenshots are captured.

## Reuse this screenshot script in another experiment

### What you can keep as-is
- Session/link creation logic (`/demo/<app>` and `/InitializeParticipant/...`).
- Screenshot saving/folder logic (`screenshots/<APP>_treatments/treatment_XX/...`).
- Generic form autofill loops in `fill_fields()`.

### What should be changed first
1. Run command with the app name and URL:

```powershell
python .\scripts\capture_me_screenshots.py --base-url http://127.0.0.1:8000/ --app YOUR_APP --out screenshots
```

2. Update field defaults in `scripts/capture_me_screenshots.py` (only if your forms use different names):
	- `TEXT_VALUES`
	- `NUMBER_VALUES`
	- `RADIO_VALUES`

3. If your survey page URLs/labels are different, adjust `resolve_page_label()`.
	- It currently has ME-specific detection for `Survey1/Survey2/Survey3` using field names.

4. If you do not have a bot/recaptcha page, you can ignore `fast_forward_bot_screening()`.
	- If you have a different blocking page, adapt this function to that page.

### Optional knobs (no code edits)
- Number of treatments: `--treatments N`
- Max page steps per treatment: `--max-steps N`
- Capture some pages only once: `--capture-once-labels "BotScreening,SomeOtherPage"`
- See browser while running: `--headed`


