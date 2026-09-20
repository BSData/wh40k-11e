# Codex: Space Marines 11e — BSData catalogue(s) (QA notes)

**Game system:** Warhammer 40,000 11th Edition (`sys-352e-adc2-7639-d610`, rev 14) · Built 2026-09-20.

**Files**
- `Imperium - Space Marines (11e).json` — 67 generic datasheets + 151 shared weapons + all 15 detachments + army rule.
- `chapters/Imperium - <Chapter> (11e).json` — that chapter's named characters. Each **imports the main SM
  (11e) catalogue** (`catalogueLinks`, `importRootEntries`), so it inherits the detachments, army rule and
  generic units — and doesn't redefine them (the army rule lives only in the main file to avoid an id clash).
  (Ultramarines ×8, Iron Hands / Imperial Fists / Raven Guard / Salamanders / White Scars ×2 each.)

## Status / provenance — READ FIRST
- **PRE-RELEASE & PROVISIONAL.** Datasheets transcribed (image OCR + human read) from a pre-release
  Codex: Space Marines (11e). Points are **provisional** (community points chart, not official MFM).
  For a **QA branch**, not release before the codex is out. Re-reconcile on release day.

## What's built (matches the sibling file's conventions)
- **Weapons are SHARED entries** (`type:"upgrade"`, `collective:false`, `comment:"BS2+"`, weapon-category
  links, one weapon profile); units reference them via **entryLinks** — deduped (151 in the main file).
- **85 datasheets** total — full Unit statlines, every weapon profile (all characteristics + keywords,
  split firing modes), datasheet/wargear abilities, keyword→category links, provisional points, and a
  **Combat Doctrines** infoLink on every unit.
- **Army rule** — Combat Doctrines + Transhuman Strategist (sharedProfiles).
- **15 detachments** — `Detachment` sharedSelectionEntryGroup: rule + all stratagems (CP + WHEN/TARGET/
  EFFECT verbatim) + Force Disposition link + Detachment Points (Gladius 3, rest 1).
- **Enhancements — two kinds, both detachment-gated** (each sub-group has a `set hidden=true` modifier that
  fires when its detachment's `selections` at roster scope `< 1`, matching the mature file):
  - **Normal enhancements** → shared max-1 `Enhancements` group (12 detachment sub-groups), entryLinked onto
    every **non-Epic-Hero CHARACTER** (22 units): one per character, from a detachment that's in the list.
  - **`[UPGRADE]` enhancements** (8 of them — Ironclad's Artificer Sarcophagus & Venerable Champion, Gravis
    Siege's Immovable Conquerors, Ironstorm's & Stormlance's pairs, Assault Brethren's Furious Assault) →
    shared `Detachment Upgrades` group (5 sub-groups), entryLinked onto **all 67 units**. Each carries
    `max 1 per unit (parent)` + `max 3 per army (roster)`, so e.g. a Dreadnought can take Ironclad's upgrades
    but no character enhancement. QA: each upgrade's own target restriction (Dreadnought / Gravis / Vehicle /
    Mounted) lives in its ability text — enforce it if you want the picker to hide it on ineligible units.
  Enhancement **points** are the real MFM-accurate values from the TTB detachment-focus articles (codex +
  chapter) — only **Stormlance Task Force's 2** are still unknown (TTB printed them as "Xpts"); the
  `Enhancements` slot cost is 1 each.
- **Weapon-keyword RULE infoLinks** — every shared weapon links to the game system's keyword rules
  (Pistol / Rapid Fire / Assault / Devastating Wounds / Lethal Hits / Sustained Hits / Torrent / Blast /
  Twin-linked / Heavy / Precision / Melta / Hazardous / Lance / Ignores Cover / Extra Attacks), 170 links
  in the main file, all resolving.
- **Wargear swaps** — unambiguous "This model's X can be replaced with 1 Y" bullets are built as proper
  min1/max1 groups with a `defaultSelectionEntryId` (base weapon) + the alternative entryLinked, exactly
  like the sibling file's `Weapon 1`/`Weapon 2` groups (the base weapon is moved out of the fixed
  entryLinks into the group). Multi-target / non-weapon bullets stay as a `Wargear Options` group.
- **Squad point-scaling** (17 multi-size squads) — restructured into a `Unit` group with Sergeant + trooper
  model sub-entries carrying the statline + only the "Every model is equipped with" default weapons; squad
  size constraints (e.g. Intercessors 5–10, Bladeguard 3–6); base cost = min-size points, with a cost
  modifier setting it to the max-size points at full size (e.g. 85→170). Special/sergeant weapons sit in a
  `Weapon Options` group. Caveats for QA: only the two MFM brackets are modelled (intermediate sizes take
  the base cost), the option per-model limits are generous (set to the trooper max, tighten to the real
  "1 in 5" limits), and the cost-modifier condition is worth a load-test in BattleScribe.
- **Leader + Support attach-lists (derived from the 11e release data)** — the pre-release codex datasheet
  pages don't print the "can be attached to" box, so attach lists were derived from the **current BSData 11e
  release catalogues** (`wh40k-11e`: main SM + chapter files) and filtered to units that exist in this codex:
  - **Leaders** — 25 of 27 carry a `Leader` ability ("This model can be attached to the following units: …").
    Not matched (set from the codex/app): **Captain on Bike**, **Marneus Calgar** (Legends/special in the data).
  - **Support** units — 9 carry a `Support` ability from the `Supporting` association (Ancient/Ancient in
    Term, Apothecary, Apothecary Biologis, Bladeguard Ancient, Judiciar, Wardens of Ultramar, and Lieutenant
    /Lt-in-Phobos which attach as *both* Leader and Support). Not matched: Victrix Honour Guard, Company
    Heroes, Kaius Konorius.
  **Verify against the final codex** — attachments can shift with a new book. Optional: convert the `Leader`/
  `Support` ability text into the functional `Leading`/`Supporting` associations the release data uses.
- **Chapter-specific detachments — one per codex-compliant chapter, in its chapter file.** The 15 generic
  codex detachments sit in the main file's `Detachment` group; each compliant chapter's own detachment sits
  in its chapter file's `<Chapter> Detachment` group (all data from the TTB detachment-focus articles, MFM-
  accurate):
  - Ultramarines — **Blade of Ultramar** (3 DP; Priority Assets / Take and Hold) · Iron Hands — **Medusa's
    Wrath** (2 DP; Purge the Foe) · Imperial Fists — **Ceramite Sentinels** (2 DP; Take and Hold) ·
    Salamanders — **Forgefathers' Seekers** (2 DP; Priority Assets) · Raven Guard — **Shadowmark Talon**
    (2 DP; Disruption) · White Scars — **Spearpoint Task Force** (2 DP; Reconnaissance).
  - Each has its rule + stratagems + enhancements with **real points**. The **non-codex-compliant** chapters
    (Blood Angels, Dark Angels, Space Wolves, Deathwatch, Black Templars) are deliberately excluded — they
    have separate rules / their own catalogues.
  - **Enhancement hosting (the fix):** a chapter detachment's enhancement / upgrade sub-groups are placed in
    main's shared `Enhancements` / `Detachment Upgrades` groups, each `set hidden=true` unless that chapter
    detachment is selected in the roster (the same gating pattern the release data uses — e.g. Black Templars).
    So any eligible character/unit (incl. the generic ones in the main file) can take a chapter enhancement,
    but only when that chapter detachment is taken. White Scars' **Chogorian Huntmaster** is an `[UPGRADE]`
    → it sits in `Detachment Upgrades` (unit, not character).
- **Named characters split** into per-chapter files that import the main codex (above).
- **Validation run:** all 7 files JSON-valid; every categoryLink / weapon entryLink / profile typeId /
  characteristic typeId / cost typeId resolves (0 unresolved; 371 weapon links in the main file all
  resolve). **NOT** yet loaded in BattleScribe/NewRecruit — that's QA step 1.

## What QA still needs to finish
1. **Stormlance Task Force — 2 enhancement points** (Supercharged Engines, Auspex Triangulation Shrines):
   TTB printed them as "Xpts", so they're 0 pending the official MFM. Every other enhancement (codex +
   chapter) carries its real MFM-accurate value.
2. **Leader attach-lists** — populated by the 11e-release-data derivation (see above), but **verify for 11e** and fill the 2
   unmatched leaders (Captain on Bike, Marneus Calgar). Optional: convert the `Leader` ability text into the
   functional `Leading` association (per-target conditions) the mature file uses.
3. **BattleScribe load-test** — I can't run BattleScribe/NewRecruit here; load each file and sanity-check
   (esp. the squad cost-modifier condition and the enhancement `hidden` gating firing correctly).

## Data gaps / flags
- **No points entry:** `Aethon Shaan`, `Kaius Konorius` (points chart had two garbled names "Caven"/"Kais"
  — almost certainly these two, not assumed). Fill on MFM.
- **Verify these transcriptions** (full list in each datasheet JSON's `uncertain` field):
  Chief Librarian Tigurius Storm of the Emperor's Wrath A `D3+6`? · Roboute Guilliman keyword `Mobile`? ·
  Intercessor Grenade Launcher Krak S `10`/`9`? & Frag `[BLAST 1]`? · Librarian in Terminator Armour
  keyword line omits `CHARACTER` · Chaplain (foot) omits `CHAPTER`/`Chaplain` · Kayvaan Shrike keyword
  line · Infernus Pyreblaster `[BLAST 1, TORRENT]` · Suboden Khan spelling. Damaged-bracket shorthand
  (vehicles show only "Damaged N") is expected 11e, not an error.

## Reproduce / rebuild
Extracted per-datasheet + per-detachment JSON: `data/codex-11e/space-marines/{datasheets,detachments}/`.
Builder: `tools/build_sm11e_cat.py` (regenerates every catalogue from that JSON).
