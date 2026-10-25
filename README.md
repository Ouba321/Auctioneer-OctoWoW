# Auctioneer-OctoWoW

The classic **Auctioneer** auction-house addon for 1.12 (vanilla) clients, bundled
with the two helper addons it needs and patched to load on OctoWoW. Current release: **v1.9.7**.

| Addon | Version | What it does |
|---|---|---|
| Auctioneer | 3.6.2-octo-v1.9.7 (Platypus) | Scans the auction house and shows price info (**side scan panel**, **resumable scans**, **subcategory scans**, **Item Scan/Search**, **cheapest-first Browse sorting**, **item name suggestions**, a reworked **Search Auctions** tab, **cancelling several auctions at once**, Post Auctions improvements and **`/auc-glossary`**, see below) |
| EnhTooltip | 3.8.0-octo-v1.9.7 (Kangaroo) | Adds Auctioneer's extra lines to item tooltips |
| Stubby | 3.8.0-octo-v1.9.7 (Kangaroo) | Loads Auctioneer and EnhTooltip when they're needed (**patched**, see below) |

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

Each release has one number, like **v1.9.7**, used everywhere:

- on GitHub: the release is tagged `v1.9.7`, and its zip is `Auctioneer-OctoWoW-v1.9.7.zip`
- in the game's AddOns list: all three addons show `-octo-v1.9.7`. The part before it is the
  addon's own version: 3.6.2 for Auctioneer, 3.8.0 for EnhTooltip and Stubby.
- in [CHANGES.md](CHANGES.md): the heading `v1.9.7`

Releases count up as v1.9.1, v1.9.2 and so on. **v2.0** is kept for the finished version, so
the 1.x numbers mean it's still being worked on.

**Which version do I have?** Look at the end of each addon's description in the AddOns list
on the character screen. For v1.9.7 that's `[3.6.2-octo-v1.9.7 (Platypus)]` for Auctioneer and
`[3.8.0-octo-v1.9.7 (Kangaroo)]` for EnhTooltip and Stubby. If the three don't show the same
version, some folders are left over from an older download: copy all three again.

- v1.2 and v1.3 only showed the version on Auctioneer, e.g. `[3.6.2-octo-v1.3 (Platypus)]`.
- Before v1.2, Auctioneer still showed the original `[3.6.1 (Platypus)]`. For those, at the
  auction house type
  `/script ChatFrame1:AddMessage("V: "..tostring(Auctioneer and Auctioneer.Core and Auctioneer.Core.Constants.FilterDefaults["pct-maxover"]))`:
  `V: 200` means 3.6.1-octo10, `V: nil` means older.

## All commands

Type **`/auctioneer-help`** in chat for every `/auctioneer` command with what it does, grouped by
topic (scanning, bargains, selling, pricing, Browse, tooltips, setup), including the ones this
bundle adds. **`/auctioneer-help scan`** shows just the lines containing "scan". The original
`/auctioneer help` still works, but it predates this bundle's commands.

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
- At the auction house, Shift+Right-click fills the panel's box, on every tab. (On the **Search
  Auctions** tab a plain right-click fills that tab's own item search box.) If you use LazyPig,
  its Shift+Right-click split doesn't happen while the auction house is open; it still works
  everywhere else.

## Sorting the Browse list

The Browse tab's price column has two headers: **Current Bid** and **Buyout**. The original only
had Current Bid. Click a header to sort by it, and click it again to flip the order.

When you open the auction house, the list starts sorted by **buyout, lowest first**, with the
lowest bid deciding between equal buyouts. Clicking a header changes the sort until you next
open the auction house.

- `/auctioneer browse-sort bid` starts with the lowest bid first instead, `/auctioneer
  browse-sort off` leaves the game's own order, and `/auctioneer browse-sort buyout` goes back.
  `/auctioneer browse-sort` shows the setting.
- **Only the page on screen is sorted.** The 1.12 game sorts the 50 auctions it has loaded. It
  can't ask the server for a sorted list, so the cheapest auction may be on page 2.
- It sorts each auction's whole price, not the price per item: a stack of 20 for 1g comes after
  a single one for 10s.

**For every auction of an item, cheapest per item first:** run an **Item Scan** for it, then on
the **Search Auctions** tab pick the search type **Item**, type the name and search. That list
holds every auction Auctioneer saw, from all pages. Switch its price column to *Buyout Per* to
sort by the price per item.

## Item name suggestions

Type two or more letters in the Browse tab's **Name** box, or in a name box on the **Search
Auctions** tab, and up to eight item names drop down under it: names that start with the
letters, or have a word that does ("Li" finds Linen Cloth and Bolt of Linen Cloth). They come
from every item Auctioneer has seen at your auction house.

- **Click** a name to search for it. **Tab** fills in the top name, **Enter** searches what you
  typed, as before.
- **A picked name means exactly that item.** Picking Iron Bar lists Iron Bars only, not Dark Iron
  Bars, on Browse (with a **Show all** button) and on Search Auctions. A name you type yourself
  finds every item containing it, like the game's own search.
- **Your profession's reagents come first.** Auctioneer reads your recipes whenever a profession
  window is open, also away from the auction house, and ranks the reagents of recipes that still
  give skill points: orange first, then yellow, then green, shown in that colour with the
  profession's name. A tailor at skill 50 typing "Bo" gets Bolt of Linen Cloth at the top; at 200
  it's Bolt of Mageweave. Open your profession windows once after installing so it knows them.
- **Then names that start with the letters, then items for your level**, nearest your level
  first, with the level shown (in red when it's above yours). Then the rest, the items you've
  seen most often first.
- Only recipes you've learned count, and recipes under a collapsed heading in the profession
  window are skipped. Trade goods like cloth and bars have no level in the game's data, so
  without the profession they're ordered by how often they're seen.
- `/auctioneer name-suggest off` turns it off.

## Search Auctions tab

This tab looks through what your scans saw, and checks the live auction house for a named item.
It starts on the **Item** search type, which lists every auction for a name. **Bids** and
**Buyouts** list only bargains: auctions you could resell for a profit by their settings. When one
of those finds auctions of the item but none that qualify, the line under the list says so
("3 auctions found, none a bargain by these settings").

- **Searches check the live auction house.** With an item name, every search type lists what
  your saved scans have at once, so a recent scan fills it straight away. It then scans the
  auction house for that name, and the list fills in page by page as the scan goes ("Scanning the
  AH for Iron Bar... 5 found so far"). A Bid or Buyout pressed meanwhile waits for the scan.
  Skipped while another scan runs, and for a name scanned to the end in the last minute. Without
  an item name a search covers your saved scans, since checking the whole auction house means a
  full scan. `/auctioneer search-scan off` searches only your saved scans.
- **Best deals first.** Item searches show prices per item (Bid Per, Buyout Per) and list the
  cheapest first; auctions without a buyout come last. Bid and Buyout searches list the most
  profit first. Before, all of them were sorted by time left. Clicking a header still re-sorts.
- **How old the results are.** The line under the list says how many auctions there are and when
  they were seen ("12 auctions, seen 5 min to 3 h ago"), and each auction's tooltip
  says when it was seen.
- **Possibly gone** auctions, ones an unfinished scan hasn't seen again, are greyed out, and their
  tooltip says so.
- **Names.** A name picked from the suggestions finds only that item. A typed name finds every
  item containing it, and brackets or dashes in it are just letters (a bracket used to break the
  search).
- **Right-click** an item in your bags to put its name in the item search box. Like a picked
  suggestion, it finds exactly that item. Shift+Right-click puts the name in the side panel's
  Item Scan box instead, as on every tab.
- **Category Restriction** has subcategories: hover a category and pick one from the list that
  opens beside it, like Trade Goods > Parts or Weapon > Two-Handed Swords. A subcategory name two
  categories share shows its category too ("Armor: Miscellaneous"). An item the game hasn't
  cached yet isn't counted in a subcategory.
- **Saved searches:** **Save**, next to each **Search** button, saves the search with all its
  settings. It's named after its item ("Citrine", or "Citrine (Bids)" for a Bid search), or with
  no item after its type and category ("Buyouts: Weapon"); saving it again replaces it. Pick one
  in the **Saved Searches** list at the top to fill it all in, and **Delete** removes the one
  shown. A search saved without a name in an older version shows as **(no name)**.

## Auctions tab: cancelling several at once

- **Shift-click** your auctions to mark several, and Shift-click one again to unmark it. The
  one you had selected counts as the first. A plain click goes back to selecting one.
- **Alt-click** marks every auction from the last one you clicked to this one: click the first,
  Alt-click the last. It counts down the whole list, so you can scroll in between. On a Mac
  keyboard that's Cmd if the game gets Cmd as Alt (WoW only knows Shift, Ctrl and Alt), or
  Option.
- **Cancel Auction** then reads **Cancel 3 Auctions**, and one warning covers them all: the
  deposits you lose, and what bids on them cost you (5% of each bid, as for one auction). One
  selected auction gets the same warning, with its deposit.
- **Deposits:** the game doesn't say what a posted auction's deposit was, so Auctioneer notes it
  when you post, from either tab, and the warning shows exactly that. For an auction posted with
  an older version it's worked out from the item's vendor price and time left: exact when it's
  Very Long (a 24 hour auction), otherwise "at least" the smallest it could have been, since the
  auction may have started longer.
- They're cancelled one at a time, a moment apart, each one found again by its details, since
  the list renumbers after every cancel. If the auction house closes halfway, the rest stay up
  and the chat says how many.
- A mark goes away when its auction changes (it gets a bid, or sells), and leaving the tab clears
  the marks. After Shift- or Alt-clicking, or when the list has moved, Cancel Auction does
  nothing until you click an auction again, so it can't cancel one you unmarked or one that
  moved into the selected row.

## Post Auctions tab

- **Right-click** an item in your bags to put it in the Post Auctions slot (dragging still works,
  and so does Auctioneer's Alt+Left-click).
- **Putting an item in the slot scans the auction house for it first** (an Item Scan), so the
  suggested prices use what's for sale right now. The note says "Scanning the auction house for
  this item...", the side scan panel shows the progress, and the prices and the list of current
  auctions refresh when it's done. It's skipped while another scan is running, and for an item
  scanned in the last minute (posting several stacks scans once). `/auctioneer post-scan off`
  turns it off.
- **Clicking the Browse tab** from Post Auctions with an item in the slot searches Browse for it,
  so you can check the suggested price against what's actually for sale. If the item is still
  being scanned, Browse shows it once the scan is done. It's skipped while another scan runs,
  since Browse is busy then. `/auctioneer post-browse off` turns it off.
  - Browse then lists **only that item**. The game's name search also finds longer names, so
    "Iron Bar" brings up Dark Iron Bars too; those are hidden. Scrolled to the end of the list,
    the line under it says how many, with a **Show all** button. Paging through the item keeps it that way; any other
    search shows everything again. Clicking, bidding and buying work on the rows as shown.
- **Undercut by** slider (1-100%) under the auction list. The starting and buyout prices follow
  it as you drag. It only applies to the item in the slot; the next item starts at your setting
  again (`/auctioneer pct-underlow`, default 5%). The suggested buyout, made for profit:

  | Situation | Suggested buyout |
  |---|---|
  | Someone sells it cheaper than market | their price minus the slider % |
  | The cheapest seller is **above** market (e.g. it sold out at 50s, someone relists at 1g) | **their price** minus the slider %, so 95s, not 47s |
  | ...but absurdly high | at most 3x the market price (`/auctioneer pct-maxover`, default 200% over) |
  | You're already the cheapest (one or more of yours at the bottom) | the cheapest auction that *isn't yours* minus the slider %, even if that's below your own listing; with nobody else selling it, market price minus the slider % |
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
