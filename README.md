# Auctioneer-OctoWoW

The classic **Auctioneer** auction-house addon for 1.12 (vanilla) clients, bundled
with the two helper addons it needs and patched to load on OctoWoW.

| Addon | Version | What it does |
|---|---|---|
| Auctioneer | 3.6.1 (Platypus) | Scans the auction house and shows price info |
| EnhTooltip | 3.8.0 (Kangaroo) | Adds Auctioneer's extra lines to item tooltips |
| Stubby | 3.8.0-octo1 (Kangaroo) | Loads Auctioneer and EnhTooltip when they're needed (**patched**, see below) |

## Install

1. Download the latest zip from **Releases** (or *Code → Download ZIP*).
2. Open the zip. Copy the three folders **`Auctioneer`**, **`EnhTooltip`** and **`Stubby`**
   into your game's `Interface/AddOns/` folder.
   - The folder names must match exactly. If you used *Download ZIP*, the three folders are
     inside an extra `Auctioneer-OctoWoW-main` folder. Copy the three folders out of it,
     not the outer folder.
3. Restart the game and make sure all three are ticked in the AddOns list on the character screen.
4. At the auction house, type `/auctioneer scan` to collect prices.

## The fix

With the unpatched Stubby, some players get this error at login, and Auctioneer never loads:

```
Interface\AddOns\Stubby\Stubby.lua:630: attempt to index global 'StubbyConfig' (a nil value)
```

**Cause:** Stubby creates its settings table when the addon loads. WoW then loads the
saved settings on top of it. If that file contains `StubbyConfig = nil`, the empty value
replaces the table and Stubby crashes. On logout WoW saves the empty value again, so the
error comes back every session.

**Fix:** one line in `onLoaded()` recreates the table when it's missing, which also repairs
the saved file at the next logout. See [CHANGES.md](CHANGES.md).

You don't need to delete any saved settings. The patched Stubby fixes them on its own.

## Credits and license

Auctioneer, EnhTooltip and Stubby were written by the Auctioneer team (Norganna's AddOns,
MentalPower and contributors) and are licensed under the **GNU GPL v2**. A copy of the
license is in [LICENSE](LICENSE) and in each addon folder. This bundle is an unofficial,
community-maintained repack. It isn't affiliated with or endorsed by the original authors,
Blizzard or the OctoWoW team. Apart from the change listed in CHANGES.md, the code is the
original.
