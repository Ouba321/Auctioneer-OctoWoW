# Auctioneer-OctoWoW

The classic **Auctioneer** auction-house addon for 1.12 (vanilla) clients, bundled
with the two helper addons it needs and patched to load on OctoWoW. Current release: **v1.4**.

| Addon | Version | What it does |
|---|---|---|
| Auctioneer | 3.6.2-octo-v1.4 (Platypus) | Scans the auction house and shows price info (**side scan panel**, **resumable scans**, **subcategory scans**, **Item Scan/Search**, Post Auctions improvements and **`/auc-glossary`**, see below) |
| EnhTooltip | 3.8.0-octo-v1.4 (Kangaroo) | Adds Auctioneer's extra lines to item tooltips |
| Stubby | 3.8.0-octo-v1.4 (Kangaroo) | Loads Auctioneer and EnhTooltip when they're needed (**patched**, see below) |

## Install

1. Download the latest zip from **Releases** (or *Code → Download ZIP*).
2. Open the zip. Copy the three folders **`Auctioneer`**, **`EnhTooltip`** and **`Stubby`**
   into your game's `Interface/AddOns/` folder.
   - The folder names must match exactly. If you used *Download ZIP*, the three folders are
     inside an extra `Auctioneer-OctoWoW-main` folder. Copy the three folders out of it,
     not the outer folder.
3. Restart the game and make sure all three are ticked in the AddOns list on the character screen.
   When updating, restart the game fully too: `/reload` doesn't pick up files that are new in
   the update, and the addon then fails with errors about missing parts.
4. At the auction house, type `/auctioneer scan` to collect prices.

## Version numbers

Each release has one number, **v1.4**, **v1.5** and so on, used everywhere:

- on GitHub: the release is tagged `v1.4`, and its zip is `Auctioneer-OctoWoW-v1.4.zip`
- in the game's AddOns list: all three addons show `-octo-v1.4`. The part before it is the
  addon's own version: 3.6.2 for Auctioneer, 3.8.0 for EnhTooltip and Stubby.
- in [CHANGES.md](CHANGES.md): the heading `v1.4`

Every release gets the next number, however small. The first number only goes up for a big
change, like saved data having to be reset.

**Which version do I have?** Look at the end of each addon's description in the AddOns list
on the character screen. For v1.4 that's `[3.6.2-octo-v1.4 (Platypus)]` for Auctioneer and
`[3.8.0-octo-v1.4 (Kangaroo)]` for EnhTooltip and Stubby. If the three don't show the same
version, some folders are left over from an older download: copy all three again.

- v1.2 and v1.3 only showed the version on Auctioneer, e.g. `[3.6.2-octo-v1.3 (Platypus)]`.
- Before v1.2, Auctioneer still showed the original `[3.6.1 (Platypus)]`. For those, at the
  auction house type
  `/script ChatFrame1:AddMessage("V: "..tostring(Auctioneer and Auctioneer.Core and Auctioneer.Core.Constants.FilterDefaults["pct-maxover"]))`:
  `V: 200` means 3.6.1-octo10, `V: nil` means older.

## What do HSP, BO and the rest mean?

Type **`/auc-glossary`** in chat for a list of the shorthand Auctioneer uses, like HSP, BO,
Prof and the `pct-` settings. **`/auc-glossary hsp`** shows just the terms containing "hsp",
and `/auc-glossary pct` shows all the percentage settings.

It works anywhere in the world, but only after Auctioneer has loaded once with this version,
which happens the first time you open the auction house.

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

## Resumable scans

A full scan of a busy auction house can take hours. With the original Auctioneer, closing the
auction house, logging out or getting disconnected for being AFK meant the next scan started
over at page 1.

This bundle remembers where a scan stopped. In the side scan panel (see below), next to **Scan**,
there's a **Resume** button that continues from that page. Hover over it to see which page.

- **Scan** works like before: a fresh scan from page 1.
- **Resume** (or `/auctioneer scan resume`) continues the unfinished scan. It's greyed out
  when there's nothing to continue.
- Price history from the pages already scanned was saved as it came in, so a cut-off scan
  never loses data.
- You can change which categories are ticked before resuming. Resume finishes the
  category it was in, from the page it was on, then goes through the other ticked categories
  it hasn't finished yet, lowest number first. That includes categories you ticked since. If
  you unticked the category it was in, it skips the rest of that one. The tooltip shows the
  category and page it will continue from.
- A scan of the whole auction house (every category ticked) goes page by page instead of by
  category, so it can only be resumed while every category is still ticked.
- At the end of a scan, auctions that weren't seen again are only removed from categories the
  scan actually finished. A category you unticked halfway keeps its auctions.
- A position older than 12 hours can't be resumed, since most of those auctions are gone by
  then.
- The totals Auctioneer prints at the end of a resumed scan (new, old, gone auctions) only
  count the part after the resume.

## Scanning only some subcategories

In the auction house's category list, the checkbox next to a category chooses whether it's
scanned. Open a category and its subcategories get checkboxes too:

- **Tick one subcategory of an unticked category** to scan just that one. For example, tick
  Two-Handed Swords under Weapon to scan only two-handed swords.
- **Untick subcategories of a ticked category** to leave them out.
- Unticking the last subcategory unticks the category. Ticking the category's own checkbox
  makes it whole again, with all subcategories.

Subcategories are scanned one at a time, so resuming works at that level too: the Resume
tooltip shows e.g. "Weapon - Two-Handed Swords, page 3". With everything ticked, a scan still
goes through the whole auction house in one go.

At the end of a scan that left out some subcategories of a category, an auction that wasn't
seen again is only removed once the game confirms its item belongs to a subcategory that was
scanned. If it can't tell yet, the auction is kept.

## Side scan panel

A panel sits against the right edge of the auction house window, drawn in the same style and
as tall as the window. It shows the scan:

- what's being scanned (the whole auction house, a category or subcategory, or an item)
- page *x* of *y*, and category *x* of *y* when scanning categories, with a progress bar
- auctions per second, about how long is left, and how many auctions it has seen
- when it's done or stopped: whether it finished, was stopped, or was stopped for your search

It also holds the scan controls: **Scan**, **Resume**, **Stop**, and the **Item Scan/Search**
box and button. **Stop** ends the scan, and the Browse tab and the panel both say it stopped;
Resume carries on from there. The Browse tab's bottom row is back to Blizzard's normal layout.

**While a scan runs, the other tabs stay usable:** Bids, Auctions, Search Auctions and Post
Auctions. Before, a scan only moved while the Browse tab was open. The Browse tab itself can't
be used alongside a scan: the auction house has one list of results, and the scan is reading
it. **Searching on the Browse tab stops the scan** and runs your search; Resume continues the
scan afterwards.

## Item Scan/Search

The side scan panel has an **Item Scan/Search** box with an **Item Scan** button. Type an item
name, or **Shift+Right-click** an item in your bags to fill it in, then press **Item Scan** or
Enter. Auctioneer scans every page of the auction house for that name, showing each page in the
Browse list as it goes ("Item Scan: page 2 of 3"), then shows the first page like a normal
search.

- While it scans, **Bid** and **Buyout** are off: the list keeps changing, and a click could
  otherwise hit a different auction than the one you picked.
- It's the same name search as Blizzard's Name box, so "Linen" also finds "Bolt of Linen Cloth".
- It doesn't touch an unfinished full scan: Resume still continues that one afterwards.
- Only auctions with a matching name are checked for being gone.
- At the auction house, Shift+Right-click fills the panel's box. On the **Search Auctions** tab
  it fills that tab's item search box instead. If you use LazyPig, its Shift+Right-click split
  doesn't happen while the auction house is open; it still works everywhere else.

## Post Auctions tab

- **Right-click** an item in your bags to put it in the Post Auctions slot (dragging still works,
  and so does Auctioneer's Alt+Left-click).
- **Putting an item in the slot scans the auction house for it first** (an Item Scan), so the
  suggested prices use what's for sale right now. The note says "Scanning the auction house for
  this item...", the side scan panel shows the progress, and the prices and the list of current
  auctions refresh when it's done. It's skipped while another scan is running, and for an item
  scanned in the last minute (posting several stacks scans once). `/auctioneer post-scan off`
  turns it off.
- **Undercut by** slider (1-100%) under the auction list. The starting and buyout prices follow
  it as you drag. It only applies to the item in the slot; the next item starts at your setting
  again (`/auctioneer pct-underlow`, default 5%). The suggested buyout, made for profit:

  | Situation | Suggested buyout |
  |---|---|
  | Someone sells it cheaper than market | their price minus the slider % |
  | The cheapest seller is **above** market (e.g. it sold out at 50s, someone relists at 1g) | **their price** minus the slider %, so 95s, not 47s |
  | ...but absurdly high | at most 3x the market price (`/auctioneer pct-maxover`, default 200% over) |
  | You're already the cheapest | priced against the cheapest *other* seller, not your own lower price |
  | Someone dumps it far below market (over 30% under) | market price minus the slider %: they're not chased down (`/auctioneer broker` finds them to buy out) |
  | Nobody else sells it | market price minus the slider % |
  | No price data at all | the vendor price x4 (`pct-markup`) |

  "Market price" is the median buyout from your scans. These rules use what your last scan saw,
  so for a fresh look at one item, run an **Item Scan** for it first. The note under the price
  model says which rule applied ("Undercutting by 5%" in yellow, and so on).
- **Deposit** shows the real cost instead of "unknown", using OctoWoW's formula (the one its own
  Auctions tab uses: 2.5% of the stack's vendor price per 2 hours, plus 5% for every item the
  stack is short of a full one). Vendor prices come from Auctioneer's own list of about 9,000
  items, plus every item you place in the game's own post slot. An item missing from both
  still shows "unknown".

## Credits and license

Auctioneer, EnhTooltip and Stubby were written by the Auctioneer team (Norganna's AddOns,
MentalPower and contributors) and are licensed under the **GNU GPL v2**. A copy of the
license is in [LICENSE](LICENSE) and in each addon folder. This bundle is an unofficial,
community-maintained repack. It isn't affiliated with or endorsed by the original authors,
Blizzard or the OctoWoW team. Apart from the changes listed in CHANGES.md, the code is the
original.
