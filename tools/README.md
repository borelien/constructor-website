# tools

Not served. Source files for the generated images in `public/`.

- `og.html` → `public/og-image.png` (1200×630)
- `icon.html` → `public/android-chrome-512x512.png`, then downscaled to 192 and 180 (apple-touch-icon) and packed into `favicon.ico`

Regenerate with headless Chrome and macOS `sips`:

```shell
CHROME="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
"$CHROME" --headless=new --disable-gpu --hide-scrollbars --virtual-time-budget=3000 \
  --window-size=1200,630 --screenshot="$PWD/public/og-image.png" "file://$PWD/tools/og.html"
"$CHROME" --headless=new --disable-gpu --hide-scrollbars --virtual-time-budget=1000 \
  --window-size=512,512 --screenshot="$PWD/public/android-chrome-512x512.png" "file://$PWD/tools/icon.html"
sips -Z 192 public/android-chrome-512x512.png --out public/android-chrome-192x192.png
sips -Z 180 public/android-chrome-512x512.png --out public/apple-touch-icon.png
python3 -c "from PIL import Image; Image.open('public/android-chrome-512x512.png').save('public/favicon.ico', sizes=[(16,16),(32,32),(48,48)])"
```
