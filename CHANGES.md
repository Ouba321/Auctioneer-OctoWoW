# Changes

Changes made to the original addons, as the GPL asks. Newest first, one section per release.
Releases are numbered v1.2, v1.3 and so on. From v1.4, all three addons carry the release's
number after `-octo-` (see *Version numbers* in the README).

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
