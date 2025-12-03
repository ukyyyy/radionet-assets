**Todo**
**Changelog-Editor prüfen**: Fertigstellen und testen, dass `changelog.json` über das Dashboard gespeichert werden kann.
**`changelog.json` validieren & Backup**: Beim Speichern Validierung und automatisches Backup (`changelog.json.bak`).
**NowPlaying-Parsing testen**: Beispiele für AzuraCast / Icecast / Shoutcast prüfen (Formate, fehlende Keys, Cover-Fallback).
**Artwork-URL-Härtung**: URLs prüfen (keine API-JSON-Endpunkte), Protokoll+/CORS-Checks, relative Pfade normalisieren.
**Fallback-Artwork + lazy-loading**: Platzhaltergrafik, `loading="lazy"`, `onerror`-Fallback.
**Media Session vervollständigen**: Action-Handler für `seekto`, `previoustrack`, `nexttrack` und PlaybackState-Updates.
**Discord Rich Presence (optional)**: Lokaler Node-Bridge + Anleitung; Asset-IDs in Repo-Doku referenzieren.
**Server-side JSON-Validation**: PHP-Server prüft `json_decode` und Valide-Felder bevor Save.
**Tests & CI**: `php -l` + JSON-validate (GitHub Actions) und einfache smoke tests.
**Polling optimieren / SSE**: Polling-Rate adaptiv machen oder SSE/WS für NowPlaying erwägen.
**Accessibility & Keyboard**: ARIA-Labels, focus styles, Tastatursteuerung (Space/Enter für Play, Arrow für volume/seek).
**UX Mobile & Fullscreen**: Controls größer, remember volume/favourites, Swipe für prev/next.
**Release-Checklist**: Changelog-Einträge, OG-Image-Generierung prüfen, Version-Tag erstellen.
**Housekeeping**: Formatierung, entferne toten Code, README Dev-Setup aktualisieren.

**Ideen / Feature-Vorschläge**
**Station-Favoriten-Sync**: Optionaler Server- oder GitHub-Gist-Sync für Favoriten (falls Benutzer-Accounts fehlen).
**Shareable NowPlaying Links**: Kurzlink mit aktueller Trackinfo (z. B. `stations.php?id=X&now=...`) + OG-Preview.
**Embed-Widget**: Kleiner HTML/JS-Embed-Code für Dritseiten, responsive.
**Auto-Cover-Fetcher**: Fallback: Suche Artwork via iTunes/LastFM/Spotify-lookup wenn möglich.
**Ad Scheduler/Rotator**: Serverseitige Ads-/Jingles-Playlist-Steuerung mit `ads.json` Integration.
**Analytics**: Leichtgewichtige Hörer-Statistiken (counts per station) in JSON + Dashboard-Chart.
**Skin/Theme-Switcher**: Dark/Light + accent color presets, persisted in `localStorage`.
**Offline Caching**: Service Worker für statische Assets + basic offline landing page.

**Bekannte / Mögliche Bugs & Fix-Vorschläge**
**Broken artwork when API URL assigned**: Ursache: `nowplaying` field sometimes contains JSON endpoints not image URLs. Fix: Validate file extension / Content-Type HEAD before `img.src` assignment; fallback to `station.artwork` or placeholder.
**Changelog editor partial-save race**: Ensure `save_changelog` handler writes atomically (write to temp + rename) to avoid corruption.
**Malformed `changelog.json` after raw-edit**: Add server-side JSON syntax validation and return error to dashboard instead of overwriting file.
**NowPlaying polling overload**: If many users, server may receive many poll requests for same upstream. Consider server-side cache or proxy (cache per station for 10–30s).
**Media Session image CORS issues**: If artwork blocked, Media Session may silently fail — try to host OG/artwork on same domain or ensure CORS headers.
**Audio autoplay / user gesture restrictions**: Some browsers block autoplay; ensure play is user-initiated and gracefully show play button.
**Missing input sanitization in dashboard**: Escape outputs with `htmlspecialchars` (already used in many places) and validate POST payloads.
**PHP file write permissions**: On some systems the webserver may not have permission to write JSON files — check and document `chmod`/Windows ACL steps.

**Prioritäten (kurzfristig)**
**High**: Changelog editor finish + `changelog.json` validation & backup.
**High**: Artwork URL validation & placeholder to stop broken images.
**Medium**: Test NowPlaying parsing for common station types.
**Medium**: Media Session full handlers.
**Low**: Discord RPC bridge + optional features.

**Schnelle Test-/Run-Checks**
Lokaler Testserver starten (PowerShell):
```
php -S localhost:8000
```
Dashboard prüfen: `http://localhost:8000/dashboard.php` → Section `Changelog` testen.
NowPlaying Debug: Open `stations.php?id=...` und beobachte die Konsole / Netzwerkanfragen.

---
Notizen: Wenn du möchtest, kann ich die wichtigsten High-Priority-Punkte jetzt nacheinander abarbeiten (ich beginne mit 1) und jeweils hier kurz reporten. Soll ich direkt mit dem Changelog-Editor-Test & Backup starten? 

