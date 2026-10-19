# Aviator house-ads config

Plain JSON config for Aviator's in-house ad engine (interstitial / rewarded / banner),
served via GitHub Pages at:

https://vachek-hooves.github.io/11016-aviator-house-ads/config.json

The app polls this URL at runtime (cached to disk, re-fetched at most every 30 min in
release builds) — editing `config.json` and pushing is enough, **no app release needed**.

`engine_enabled` starts `false` — the app shows no ads at all until this is flipped
to `true` and at least one creative is added below.

## Adding a creative

Append an object to `creatives`:

```json
{
  "id": "promo_video_1",
  "format": "video",
  "placements": ["interstitial", "rewarded"],
  "media_url": "https://vachek-hooves.github.io/11016-aviator-house-ads/media/promo.mp4",
  "image_url": "https://vachek-hooves.github.io/11016-aviator-house-ads/media/promo_poster.png",
  "icon_url": "https://vachek-hooves.github.io/11016-aviator-house-ads/media/icon.png",
  "title": "App name",
  "body": "Short pitch / call to action.",
  "cta_text": "Install",
  "bundle_id": "com.example.other.app",
  "weight": 1
}
```

- `format`: `video` | `image` | `text`
- `placements`: any of `interstitial`, `rewarded`, `banner` — one creative can target several
- `bundle_id`: Play Store package name the CTA opens (`market://details?id=...`)
- `store_url`: optional fallback if the device has no Play Store app
- `weight`: relative odds among active creatives of the same placement (ignored for banner,
  which just rotates every active one every 30s)
- `active_from_epoch` / `active_to_epoch`: unix seconds, `0` = no bound

Image sizes: banner ~1440×320 (crop-scaled into an 80dp bar), interstitial/rewarded poster
1080×1920 (9:16), icon 192×192 square. Video: 9:16 mp4/H.264.

Never use a real third-party brand/trademark in a creative's title, body or image — Play
Console rejection risk for both the advertised app and this one.
