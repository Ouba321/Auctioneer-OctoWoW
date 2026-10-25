# Changes

Changes made to the original addons, as the GPL asks. Newest first, one section per release.
Releases are numbered v1.2, v1.3 and so on, then v1.9.1, v1.9.2 until the finished v2.0. From
v1.4, all three addons carry the release's number after `-octo-` (see *Version numbers* in the
README).

## v1.9.3 (2026-10-25)

### Auctioneer 3.6.2-octo-v1.9.3

- The Post Auctions undercut slider moves the prices for an item you already have listed too (a
  player's report). When your auction was the cheapest, the price stayed at your own unless
  undercutting the next one came out higher, and always when the next one was yours as well, so
  the slider did nothing. Now it's priced against the cheapest auction that isn't yours, with the
  slider's undercut, or against the market price when nobody else sells it.
  `AucStatistic.lua`: `findLowestOtherAuction()`; `getHSP()` uses it for its second price when
  the Post tab passes an undercut and the cheapest auction is yours. `AuctionFramePost.lua`,
  `AuctionFramePost_UpdatePriceModels()`: uses that price whenever you're the cheapest.
  Tooltips and everything else that doesn't pass an undercut are unchanged.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.9.3`.

### EnhTooltip and Stubby 3.8.0-octo-v1.9.3

- `EnhTooltip/EnhTooltip.toc` and `Stubby/Stubby.toc`: version changed to `3.8.0-octo-v1.9.3`, to
  match the release. No code changes.

## v1.9.2 (2026-10-25)

### Auctioneer 3.6.2-octo-v1.9.2

- Search Auctions (`AuctionFrameSearch.lua`), from a player's screenshot:
  - The tab starts on the **Item** search type, which lists every auction for a name. It started
    on **Bids**, which lists bargains only (ending soon, at least 30% under market, 50s profit by
    default), so a search for an item usually found nothing and looked broken.
  - A Bids or Buyouts search for an item that finds auctions, but none passing its settings, says
    so under the list ("3 auctions found, none a bargain by these settings";
    `AuctionFrameSearch_NoBargainText()`), instead of "Nothing found in your scans."
  - The list fills in page by page while the item's scan runs ("Scanning the AH for Iron Bar...
    5 found so far"), not only when it ends. `AHScanning.lua`: `itemScanPage()` after each page of
    an Item Scan; `AuctionFrameSearch_RefreshDuringItemScan()`.
  - Search and Save are the same width, side by side and as wide as the item box above them.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.9.2`.

### EnhTooltip and Stubby 3.8.0-octo-v1.9.2

- `EnhTooltip/EnhTooltip.toc` and `Stubby/Stubby.toc`: version changed to `3.8.0-octo-v1.9.2`, to
  match the release. No code changes.

## v1.9.1 (2026-10-25)

### Auctioneer 3.6.2-octo-v1.9.1

- Shift+Right-click on a bag item fills the side panel's Item Scan box on every tab again. On
  Search Auctions it went to that tab's search box, the same as v1.9's new plain right-click.
  `AucUtil.lua`, `containerFrameItemButtonOnClick()`.
- Search Auctions (`AuctionFrameSearch.lua`), from a player's feedback:
  - Every search type with an item name (Bids, Buyouts and Item) shows your saved scans at once
    and scans the auction house for the name, then runs again on what it found
    (`AuctionFrameSearch_Search()`). Before, only Item searches scanned, with an empty list
    meanwhile. A Bid or Buyout pressed during the scan waits for it, as the bid manager doesn't
    start while a scan runs.
  - Saved searches: a **Save** button next to each Search button (`AuctionFrameSearch_SetupLayout()`,
    `AuctionFrameSearch_SaveCurrentSearch()`), named after the item, or the search type and
    category (`AuctionFrameSearch_SavedSearchName()`). The top row is the Saved Searches dropdown,
    now drawn normally and showing the search picked, and **Delete** (was Remove), which deletes
    that one; the name box and the top Save button are hidden. The list ticks the search shown
    by name, not by row number, so saving or deleting doesn't move the tick.
  - The line under the results is shorter ("12 auctions, seen 5 min to 3 h ago", "Nothing found
    in your scans.") and limited to 375 pixels, so it no longer runs into the Bid button.
- `Auctioneer.lua`: the `search-scan` help and glossary lines follow the new behaviour.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.9.1`.
- Version numbers: releases now count v1.9.1, v1.9.2 and so on; v2.0 is kept for the finished
  version (README, *Version numbers*).

### EnhTooltip and Stubby 3.8.0-octo-v1.9.1

- `EnhTooltip/EnhTooltip.toc` and `Stubby/Stubby.toc`: version changed to `3.8.0-octo-v1.9.1`, to
  match the release. No code changes.

## v1.9 (2026-10-25)

### Auctioneer 3.6.2-octo-v1.9

- Search Auctions, from the user's feedback:
  - A plain right-click on a bag item puts its name in the item search box of the search type
    showing (Shift+Right-click already did), marked as picked so it's searched exactly.
    `AucUtil.lua`, `containerFrameItemButtonOnClick()`.
  - Category Restriction has subcategories, in a submenu on each category.
    `AuctionFrameSearch.lua`: `AuctionFrameSearch_CategoryDropDown_Initialize(level)` builds both
    levels and ticks rows itself; the choice is kept on the dropdown (`AuctionFrameSearch_SetCategory()`,
    `AuctionFrameSearch_CategoryFilter()`), since the game's selection by row number ticked the
    wrong rows across levels. `AucFilter.lua`: `categoryMatches()` checks a subcategory by the
    item's `GetItemInfo` subclass, in the bid, buyout and plain filters.
  - Saved searches: saving without a name used the item's name, or asks for one, instead of
    saving under "", which showed as a second blank row and made the list look a row off; an
    old unnamed one is listed as "(no name)". Saved categories keep their subcategory ("6/2";
    old numbers still load). Loading a saved Item search initialized the category dropdown's
    label instead of the dropdown and stopped with an error; fixed. A dropdown never opened no
    longer breaks saving (`%d` of nil).
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.9`.

### EnhTooltip and Stubby 3.8.0-octo-v1.9

- `EnhTooltip/EnhTooltip.toc` and `Stubby/Stubby.toc`: version changed to `3.8.0-octo-v1.9`, to
  match the release. No code changes.

## v1.8 (2026-10-25)

### Auctioneer 3.6.2-octo-v1.8

- The Search Auctions tab, reworked. `AuctionFrameSearch.lua`:
  - Item searches with one name run a quiet Item Scan for it first and show the results when it
    ends (`AuctionFrameSearchPlain_SearchButton_OnClick()`, `AuctionFrameSearch_RefreshAfterItemScan()`),
    with an empty list meanwhile; not while another scan runs or for a name scanned in the last
    minute. New setting `search-scan` (`AucCore.lua`, default on; `AucCommand.lua`;
    `/auctioneer-help`; `/auc-glossary`).
  - Best deals first: Item searches show Bid Per and Buyout Per and sort cheapest per item first,
    Bid and Buyout searches most profit first (they were sorted by time left, and the Buyout
    search by % less, smallest first). Sorting by buyout, an auction without one counts as the
    highest price.
  - The line under the list gives the number of results and how long ago they were seen
    (`AuctionFrameSearch_ShowResultsAge()`), and each result's tooltip when it was seen.
  - Auctions an unfinished scan hasn't seen again (the snapshot's "dirty" mark) are greyed out,
    with "Possibly gone" in their tooltip.
- `AucFilter.lua`: `nameMatches()` replaces the name check in the bid, buyout and plain filters.
  It matches as plain text, so brackets in a typed name no longer break the search, and exactly
  for a name picked from the suggestions (a new `exact` argument).
- `AucSuggest.lua`: a picked name is remembered (`isPicked()`); on Browse it lists only that item,
  using the one-item filter from v1.7.
- `AHScanning.lua`: `itemScanDone()` tells Post Auctions and Search Auctions when an Item Scan
  ends or is stopped; `itemScannedRecently()` remembers when each name was last scanned to the end.
- `AucScanner.lua`: the suggestion setup is skipped when `AucSuggest.lua` isn't loaded (after
  `/reload` instead of a restart), so the rest of the AH setup, the Search and Post tabs
  included, still happens.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.8`.

### EnhTooltip and Stubby 3.8.0-octo-v1.8

- `EnhTooltip/EnhTooltip.toc` and `Stubby/Stubby.toc`: version changed to `3.8.0-octo-v1.8`, to
  match the release. No code changes.

## v1.7 (2026-10-25)

### Auctioneer 3.6.2-octo-v1.7

- After the Post Auctions -> Browse search, Browse lists only the slotted item: the game's name
  search also finds longer names ("Iron Bar" finds Dark Iron Bar). `AHScanning.lua`:
  `browseFilterUpdate()`, a Stubby hook before `AuctionFrameBrowse_Update`, has Blizzard's own
  code draw the list while `GetNumAuctionItems`, `GetAuctionItemInfo`, `GetAuctionItemTimeLeft`
  and `GetSelectedAuctionItem` answer for the matching auctions only, puts them back, then sets
  each row's ID so that clicks, tooltips, links, Bid and Buyout use the real auction's index.
  If the drawing fails it shows the full list instead. `AucScanner.lua`, `configureAH()`: wraps
  `QueryAuctionItems` so any search for another name ends the filter, and adds the **Show all**
  button.
- Item names are suggested as you type in the Browse Name box and the Search Auctions name
  boxes, with this character's profession reagents first. New file `AucSuggest.lua` (in
  `Auctioneer.xml`'s script list), attached in `configureAH()`. `Auctioneer.lua`: new Stubby boot
  code "Reagents" reads the profession windows (`TRADE_SKILL_SHOW`/`UPDATE`, `CRAFT_SHOW`/`UPDATE`)
  and saves each profession's reagents, weighted by recipe colour, in Stubby's per-character
  settings, so it works while Auctioneer isn't loaded.
- `/auctioneer-help` lists every `/auctioneer` command with what it does, grouped by topic and
  including this bundle's own; `/auctioneer-help <word>` shows only the lines with that word.
  `Auctioneer.lua`: new Stubby boot code "Help", so it works before Auctioneer is loaded.
  `AucCommand.lua`, `chatPrintHelp()`: the original `/auctioneer help` ends with a pointer to it.
- `name-suggest` in `AucCore.lua` (default on), `AucCommand.lua`
  (`/auctioneer name-suggest on|off`) and `/auc-glossary`.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.7`.

### EnhTooltip and Stubby 3.8.0-octo-v1.7

- `EnhTooltip/EnhTooltip.toc` and `Stubby/Stubby.toc`: version changed to `3.8.0-octo-v1.7`, to
  match the release. No code changes.

## v1.6 (2026-10-25)

### Auctioneer 3.6.2-octo-v1.6

- Clicking the Browse tab from Post Auctions with an item in the slot searches Browse for that
  item, so the suggested price can be checked against what's for sale (the user's idea).
  `AuctioneerUI.lua`, `AuctioneerUI_AuctionFrameTab_OnClickHook()`: on a click on the tab
  itself, not a switch made by code like opening the AH or an Alt+Right-click search.
  `AHScanning.lua`: `showItemOnBrowse()` queues the same name search the end of an Item Scan
  uses; if that item is being scanned it waits for the scan, and another scan running skips it.
- `post-browse` in `AucCore.lua` (default on), `AucCommand.lua`
  (`/auctioneer post-browse on|off`) and `/auc-glossary`.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.6`.

### EnhTooltip and Stubby 3.8.0-octo-v1.6

- `EnhTooltip/EnhTooltip.toc` and `Stubby/Stubby.toc`: version changed to `3.8.0-octo-v1.6`, to
  match the release. No code changes.

## v1.5 (2026-10-25)

### Auctioneer 3.6.2-octo-v1.5

- The side scan panel's **Stop** button reacts to clicks again: it looked usable during a scan,
  but nothing happened. The panel takes the mouse, so clicks on it don't fall through to the
  game world, and the Stop button and progress bar, defined inside the panel, could end up below
  it once the panel was moved onto the AH window. The panel then took every click on Stop, which
  v1.4's fix couldn't help with. `AucScanPanel.lua`: `keepControlsOnTop()`, run every frame from
  `onUpdate()` and once from `configureAH()` in `AucScanner.lua`, keeps the panel's controls
  above it.
- The Browse tab's price column has a **Buyout** header next to **Current Bid**, so the list
  can be sorted by buyout too. `AucScanner.lua`, `configureAH()`: creates
  `AuctioneerBrowseBuyoutSort` from Blizzard's `AuctionSortButtonTemplate`; clicking it calls
  `SortAuctionItems("list", "buyout")`. `browseSortHeaderUpdate()`, hooked after
  `AuctionFrameBrowse_Update`, splits the column between the two headers and points the arrow.
- Browse lists start sorted lowest price first when the AH opens. `AucScanner.lua`:
  `applyBrowseSort()`, called from `auctHouseShow()`, sorts on the new setting `browse-sort`
  (`buyout` by default, `bid` or `off`), with the other price breaking ties. The 1.12 client
  sorts the page of auctions it has loaded, not the server's whole list.
- `browse-sort` in `AucCore.lua`, `AucCommand.lua` (`/auctioneer browse-sort buyout|bid|off`)
  and `/auc-glossary`.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.5`.

### EnhTooltip and Stubby 3.8.0-octo-v1.5

- `EnhTooltip/EnhTooltip.toc` and `Stubby/Stubby.toc`: version changed to `3.8.0-octo-v1.5`, to
  match the release. No code changes.

## v1.4 (2026-10-24)

### Auctioneer 3.6.2-octo-v1.4

- The side scan panel's **Stop** button works in every case. `AHScanning.lua`, `stopScanBy()`:
  - It also cancels a scan that was asked for but never started. Before, Stop was lit for those
    and did nothing, because it only acted on a running scan.
  - After stopping a scan, the Browse tab says "Scan stopped. Resume carries on where it
    stopped." instead of keeping the last "Scanning ... page" line, so it looked like nothing
    happened. A stopped Item Scan shows its last page as a normal list.
  - Stopping the Post Auctions tab's Item Scan refreshes that tab's prices from the pages it got
    through, instead of leaving "Scanning the auction house for this item..." there.
- `AHScanning.lua`, `startAuctionScan()`: a scan with no category ticked now ends the request
  after "no category selected". It used to stay open, which lit Stop and fed the results of
  normal searches to the scanner.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.4`.

### EnhTooltip and Stubby 3.8.0-octo-v1.4

- One version number for the whole bundle: EnhTooltip and Stubby now show the release's number
  in the AddOns list too, like Auctioneer. `EnhTooltip/EnhTooltip.toc`: version changed from
  `3.8.0` to `3.8.0-octo-v1.4`. `Stubby/Stubby.toc`: version changed from `3.8.0-octo1` to
  `3.8.0-octo-v1.4`. No code changes in either.

## v1.3 (2026-10-24)

Auctioneer 3.6.2-octo-v1.3. EnhTooltip 3.8.0 and Stubby 3.8.0-octo1 unchanged.

- Putting an item in the Post Auctions slot first runs an Item Scan for it, so its suggested
  prices are up to date (idea from a guild mate). `AuctionFramePost.lua`:
  `AuctionFramePost_AutoItemScan()` from `SetAuctionItem()`, skipped while a scan runs, within
  60 seconds of the last one for the same item, or with the new setting `post-scan` off;
  `AuctionFramePost_RefreshAfterItemScan()` refreshes prices and the auction list when it's done.
  `AHScanning.lua`: `requestItemScan(name, quiet)`; a quiet Item Scan doesn't search the Browse
  tab afterwards, and the end of an Item Scan calls the Post tab's refresh.
- `post-scan` in `AucCore.lua` (default on), `AucCommand.lua` (`/auctioneer post-scan on|off`)
  and `/auc-glossary`.
- `AucScanner.lua`, `finishedAuctionScanHook()`: clears the cheapest-auction cache after removing
  sold-out auctions, so a sold-out competitor doesn't stay "cheapest".
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.3`.

## v1.2 (2026-10-24)

Auctioneer 3.6.2-octo-v1.2. EnhTooltip 3.8.0 and Stubby 3.8.0-octo1 unchanged.

- The AddOns list now shows the bundle's version. WoW 1.12 shows an addon's `## Notes`, not its
  `## Version`, and the notes still had the original `[3.6.1 (Platypus)]` and `[3.8.0]`.
  `Auctioneer/Auctioneer.toc` now says `[3.6.2-octo-v1.2 (Platypus)]`, `Stubby/Stubby.toc`
  `[3.8.0-octo1]`. No code changes.
- Renamed: the bundle's Auctioneer is now numbered 3.6.2-octo-v1.x instead of 3.6.1-octoN.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.2-octo-v1.2`.

## Before v1.2

Before v1.2, each addon was numbered on its own rather than by release: Auctioneer 3.6.1-octo1
to 3.6.1-octo10, and Stubby 3.8.0-octo1.

### Auctioneer 3.6.1-octo10 (2026-10-24)

- Pricing for profit on the Post Auctions tab. `Auctioneer/AucStatistic.lua`,
  `determinePrice()`: with the tab's per-auction undercut, a cheapest competitor priced above
  market is undercut like any other instead of the price dropping to the market price, capped
  at the new setting `pct-maxover` (default 200% over market). `AuctionFramePost.lua`: when
  you're already the cheapest, it prices against the cheapest other seller. Tooltips and the
  Blizzard Auctions tab keep the original rules.
- `determinePrice()`: with no auction data at all, the vendor price x(1 + pct-markup) is used
  through `Auctioneer.API.GetVendorSellPrice`, so it no longer needs Informant.
- Fixed the price model note showing in black on the Post Auctions tab when the slider wasn't at
  the saved undercut setting. `Auctioneer/AucUtil.lua`, `getWarnColor()`: the undercut and markup
  messages are recognised by their text before the percentage, anything unrecognised is white,
  and a misdeclared colour variable is fixed.
- `pct-maxover` in `AucCore.lua` (default), `AucCommand.lua` (`/auctioneer pct-maxover <percent>`)
  and `/auc-glossary`.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.1-octo10`.

### Auctioneer 3.6.1-octo9 (2026-10-24)

- The side scan panel looks like part of the auction house: it's drawn with pieces of the AH
  window's own Browse textures (title bar with riveted ends, borders, stone background and a
  dark status pane with the results list's bevelled edge), `buildArt()` in `AucScanPanel.lua`,
  instead of a tooltip-style backdrop. It's as tall as the AH window (200 x 433), level with its
  title bar, and the status lines and controls are laid out to fit.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.1-octo9`.

### Auctioneer 3.6.1-octo8 (2026-10-24)

- Fixed `AHScanning.lua:696: too many upvalues (limit=32)`, which kept `AHScanning.lua` from
  loading in octo7. WoW's Lua 5.0 lets a function use at most 32 of the locals around it, and
  `startAuctionScan()` used 34; its hook setup now lives in `installScanHooks()`. Every Lua file
  and every XML script in the bundle now compiles with Lua 5.0.3, the version WoW 1.12 uses.
- Side scan panel: a nearly opaque dark background (tooltip style), so the game world no longer
  shows through.
- README: after updating, restart the game fully; `/reload` doesn't load new files, which is why
  octo7 also reported `attempt to index field 'ScanPanel' (a nil value)`.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.1-octo8`.

### Auctioneer 3.6.1-octo7 (2026-10-24)

- **Side scan panel.** New `Auctioneer/AucScanPanel.lua` and `AuctioneerScanPanel` in
  `Auctioneer/Auctioneer.xml`, attached to the right of the AH window in `configureAH()`
  (`AucScanner.lua`). It shows what's being scanned, page and category progress with a bar,
  speed, time left, auctions seen and how the last scan ended, and holds Scan, Resume, a new
  Stop button and the Item Scan/Search box and button. The Browse tab's bottom row is back to
  Blizzard's layout (the octo5 and octo6 squeeze is gone).
- `Auctioneer/AHScanning.lua`: the panel drives the scan every frame (`driveScan()`), so scans
  keep going on the Bids, Auctions, Search Auctions and Post Auctions tabs. They used to move
  only while the Browse tab's Search button was polling `CanSendAuctionQuery`. While scanning,
  the Search button stays clickable and `AuctionFrameBrowse_Search` stops the scan first
  (`searchStopsScan()`), keeping the resume point, then runs the search once the AH takes
  queries. `getScanStatus()` and `stopScanBy()` serve the panel.
- `Auctioneer/AucUtil.lua`: Shift+Right-click fills the panel's Item Scan/Search box on every AH
  tab, except Search Auctions, where it fills that tab's item search box when it has one.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.1-octo7`.

### Auctioneer 3.6.1-octo6 (2026-10-24)

- Item Scan/Search shows each page in the Browse list as it's scanned
  (`Auctioneer.AuctionFrameBrowse.ItemScanUpdate` in `AHScanning.lua`), with Bid and Buyout
  disabled until it's done. Starting any scan closes an open buyout confirmation.
- The Undercut slider moves the prices in every case: `getHSP()`'s per-auction undercut now also
  replaces pct-nocomp and pct-undermkt, and the Post tab always passes the slider value.
- Shift+Right-click fills the item search box on the Search Auctions tab too (`AucUtil.lua`),
  and shows the start of a long name where the client supports it.
- Bottom row of the Browse tab: the Item Scan/Search box is 96 px with a smaller font, the Item
  Scan button 70 px, and the Bid label 2 px lower.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.1-octo6`.

### Auctioneer 3.6.1-octo5 (2026-10-24)

- **Item Scan/Search.** `Auctioneer/Auctioneer.xml`: new `BrowseItemScanBox` and
  `BrowseItemScanButton`. `Auctioneer/AHScanning.lua`: `RequestItemScan()` scans every page of
  the name search (`QueryAuctionItems` with the name), as a whole-AH-style scan that never
  saves or clears the resume point, then searches for that name so the results show
  (`ItemScanButtonUpdate()`). `Auctioneer/AucScanner.lua`: such a scan only marks and cleans up
  snapshot auctions whose name contains the search (`snapNameContains()`), is cleaned up
  whatever the number of auctions, and skips the "finish" logout/quit setting.
- Bottom row of the Browse tab squeezed to fit (`configureAH()` in `AucScanner.lua`): your gold
  moves left while the Browse tab is shown, Scan and Resume (renamed from Resume Scan) are
  narrower, the Bid box's gold field is 40 px instead of 58, and Bid and Buyout are 70 px wide.
- `Auctioneer/AucUtil.lua`, `containerFrameItemButtonOnClick()`: right-click puts an item in
  the Post Auctions slot, and Shift+Right-click on the Browse tab fills the Item Scan/Search
  box. Both are handled before other addons' right-click features (LazyPig, aux).
- **Undercut slider** on the Post Auctions tab (`AuctionFramePost.xml`,
  `AuctionFramePost_SetUndercut()`), for the current auction only. `Auctioneer/AucStatistic.lua`:
  `getHSP()` takes an optional undercut percentage, which bypasses the HSP cache.
- **Deposit** on the Post Auctions tab: OctoWoW's formula instead of the original 1.12 one, and
  vendor prices without Informant. New `Auctioneer/VendorPrices.lua` (9,068 items, generated
  from Auctioneer's unloaded `ItemData.lua` and `BasePrices.lua`). `Auctioneer/AucAPI.lua`:
  `getVendorSellPrice()` falls back to prices learned from the game's post slot, then that
  table; `newAuction()` in `AucScanner.lua` records the learned prices.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.1-octo5`.

### Auctioneer 3.6.1-octo4 (2026-10-24)

- New `/auc-glossary` command: lists the shorthand Auctioneer shows (HSP, BO, Prof, min,
  curBid, the `pct-` settings and more), and `/auc-glossary <word>` lists only the terms
  containing that word. `Auctioneer/Auctioneer.lua`: the command is Stubby boot code, like
  `/auctioneer`, so it also works before Auctioneer is loaded once it has been registered.
  It's also run immediately when Auctioneer loads.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.1-octo4`.

### Auctioneer 3.6.1-octo3 (2026-10-23)

- Fixed empty Starting Price and Buyout Price on the **Post Auctions** tab for items with no
  buyout data, for example a reagent only ever seen in bid-only auctions. This bug is in the
  original Auctioneer too. `Auctioneer/AuctionFramePost.lua`,
  `AuctionFramePost_UpdatePriceModels()`: when there was no suggested price, it fell back to
  the average buyout without checking that any buyouts had been seen, so it computed 0/0. That
  "not a number" ended up in the price boxes, which real Windows and Wine display differently.
  It now checks first, like the rest of Auctioneer already did, so the boxes show 0 with the
  note "No data for HSP".
- `Auctioneer/AuctionFramePost.lua`: the price suggestions and "remember this price" use the
  posted item's own key instead of looking the item up again by name in the bags, which could
  pick a different item and failed with a Lua error when nothing was found.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.1-octo3`.

### Auctioneer 3.6.1-octo2 (2026-10-23)

- Subcategories can be ticked for scanning. `Auctioneer/AucScanner.lua`,
  `filterButtonSetType()`: subcategory rows of the opened category get a checkbox (they had
  none). `Auctioneer/Auctioneer.xml`: the checkbox click handles subcategories, and ticking a
  category's own checkbox makes it whole again.
- `Auctioneer/AHScanning.lua`: a category with some subcategories unticked is scanned one
  ticked subcategory at a time (the subcategory is passed to `QueryAuctionItems`). Settings
  are `scan-class<category>-<subcategory>`, ticked unless set to `off`. Ticking a subcategory
  of an unticked category ticks the category with only that subcategory, and unticking the
  last one unticks the category. Resume positions, finished sets and the status text include
  the subcategory. Every category ticked whole is still a whole-AH scan.
- `Auctioneer/AucScanner.lua`, `finishedAuctionScanHook()`: where only some subcategories of
  a category were scanned, an unseen auction is only removed if its item's subcategory, from
  `GetItemInfo`, is one that was scanned. Unknown items are kept.
- `Auctioneer/Auctioneer.toc`: version changed to `3.6.1-octo2`.

### Stubby 3.8.0-octo1 (2026-10-21)

- `Stubby/Stubby.lua`, `onLoaded()`: added
  `if not StubbyConfig then StubbyConfig = {} end` before the first use of `StubbyConfig`.
  This fixes `Stubby.lua:630: attempt to index global 'StubbyConfig' (a nil value)`, which
  happened when the saved settings file contained `StubbyConfig = nil`.
- `Stubby/Stubby.lua`: added a note about the modification to the header comment.
- `Stubby/Stubby.toc`: version changed from `3.8.0` to `3.8.0-octo1`.

### Auctioneer 3.6.1-octo1 (2026-10-21)

- Scans can be resumed. `Auctioneer/AHScanning.lua`: the scan position is saved in
  `AuctionConfig.scanResume` after every page. Resuming continues from there, starting with
  the last saved page again in case auctions shifted. The position is cleared when a scan
  finishes or a new one starts, and can't be resumed if it's older than 12 hours or the
  categories selected for scanning have changed.
- `Auctioneer/Auctioneer.xml`: new **Resume Scan** button (`BrowseResumeScanButton`), greyed
  out when there's nothing to resume, with a tooltip naming the page. `Auctioneer/AucScanner.lua`
  places it next to **Scan**, which still always starts a fresh scan. To make room, it moves
  Blizzard's "Bid" label and bid price box on the Browse tab 78 px to the right.
- `Auctioneer/AucCommand.lua`: `/auctioneer scan resume` does the same as the button.
- Fixed `AHScanning.lua:155: bad argument #2 to 'format' (string expected, got nil)`. This bug
  is in the original Auctioneer too. When only some categories are selected, starting a scan
  while one was already running (typing `/auctioneer scan`, or the AH being shown again) moved
  the running scan on to the next category, and past the last one to category 0, which has no
  name. `startAuctionScan()` now does nothing while a scan is running, and Resume Scan won't
  use a saved category that doesn't exist.
- If a category scan still reaches a category with no name, it stops cleanly instead of
  raising that error, and prints its last steps to chat so the cause can be found. The saved
  position stays, so Resume Scan carries on.
- Fixed the auction house closing itself when you press Scan. The real cause of the error
  above: `Auctioneer/AucUtil.lua`, `protectAuctionFrame()`, the "protect window while
  scanning" step (on by default), used `SetDoublewideFrame(nil)` to take the AH out of the
  game's window management. That call hides the window it clears, so the AH closed, which
  stopped the scan while it was starting. It now clears the slot without hiding the window.
- `Auctioneer/AHScanning.lua`, `startAuctionScan()`: if the scan is stopped while it's
  starting, the start now ends there too instead of carrying on with category 0.
- Resuming follows changes to which categories are ticked. `Auctioneer/AHScanning.lua`
  keeps the categories a scan covers and has finished, and saves them with the resume
  position. Resume continues the saved category from the saved page while it's still ticked,
  then the lowest ticked category not finished yet (`nextIndex()` now picks that instead of
  the next higher number, which gives the same order for a normal scan). A whole-AH scan
  still only resumes with every category ticked. Resume positions saved by the earlier
  version are understood: the categories before the saved one count as finished.
- `Auctioneer/AucScanner.lua`: on resume, only categories ticked since the scan started get
  the "possibly gone" marks (`invalidateAHSnapshot()` takes an optional set of categories).
  At the end, `finishedAuctionScanHook()` only removes unseen auctions from categories the
  scan finished, plus category 0 as before. A category unticked halfway keeps its auctions.
- `Auctioneer/Auctioneer.xml`: the Resume Scan tooltip names the category and page.
- `Auctioneer/AucScanner.lua`, `auctionStartHook()`: a resumed scan keeps the "possibly gone"
  marks from when it first started instead of marking everything again. Auctions not seen yet
  stay marked. Auctions already seen before the break are kept when the scan finishes, since
  the resumed scan doesn't go back to their pages.
- `Auctioneer/Auctioneer.toc`: version changed from `3.6.1` to `3.6.1-octo1`.

## Repack

- Removed leftover `.svn` and `.DS_Store` folders and files from `EnhTooltip`.
- Auctioneer 3.6.1 and EnhTooltip 3.8.0 are otherwise unchanged.
