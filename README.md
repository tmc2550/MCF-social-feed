<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MCF Grantee Social Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --ocean: #0d4f6e;
  --ocean-mid: #1a7a9a;
  --ocean-light: #e8f4f8;
  --sand: #f5f0e8;
  --sky: #a8d4e0;
  --ink: #1a1a2e;
  --muted: #6b7a8d;
  --border: #dce4ea;
  --white: #ffffff;
  --card-shadow: 0 2px 12px rgba(13,79,110,0.08);
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: 'DM Sans', sans-serif; background: var(--sand); color: var(--ink); min-height: 100vh; }

.header {
  background: var(--ocean);
  padding: 28px 40px 24px;
  display: flex; align-items: flex-end; justify-content: space-between; gap: 20px; flex-wrap: wrap;
}
.header-left h1 { font-family: 'DM Serif Display', serif; font-size: 1.9rem; color: var(--white); letter-spacing: -0.01em; line-height: 1.1; }
.header-left .subtitle { font-size: 0.82rem; color: var(--sky); margin-top: 5px; font-weight: 300; letter-spacing: 0.04em; text-transform: uppercase; }
.header-meta { display: flex; align-items: center; gap: 20px; flex-wrap: wrap; }
.stat-pill { background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2); border-radius: 20px; padding: 6px 14px; font-size: 0.78rem; color: var(--sky); font-weight: 500; }
.stat-pill strong { color: var(--white); font-size: 1rem; display: block; line-height: 1; }
#last-updated { font-size: 0.75rem; color: rgba(168,212,224,0.7); font-weight: 300; }

.toolbar {
  background: var(--white);
  border-bottom: 1px solid var(--border);
  padding: 10px 40px;
  display: flex; align-items: center; gap: 12px; flex-wrap: wrap;
  position: sticky; top: 0; z-index: 100;
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
}
.search-wrap { position: relative; flex: 1; min-width: 160px; max-width: 260px; }
.search-wrap svg { position: absolute; left: 10px; top: 50%; transform: translateY(-50%); color: var(--muted); pointer-events: none; }
#search { width: 100%; padding: 7px 12px 7px 32px; border: 1.5px solid var(--border); border-radius: 7px; font-family: 'DM Sans', sans-serif; font-size: 0.83rem; color: var(--ink); background: var(--sand); outline: none; transition: border-color 0.2s; }
#search:focus { border-color: var(--ocean-mid); background: var(--white); }
#search::placeholder { color: var(--muted); }

.toolbar-divider { width: 1px; height: 22px; background: var(--border); flex-shrink: 0; }
.filter-label { font-size: 0.7rem; color: var(--muted); font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; white-space: nowrap; flex-shrink: 0; }
.filter-group { display: flex; gap: 4px; align-items: center; flex-wrap: wrap; }
.filter-btn { padding: 5px 10px; border-radius: 5px; border: 1.5px solid var(--border); background: var(--white); font-family: 'DM Sans', sans-serif; font-size: 0.74rem; font-weight: 500; color: var(--muted); cursor: pointer; transition: all 0.15s; white-space: nowrap; }
.filter-btn:hover { border-color: var(--ocean-mid); color: var(--ocean); }
.filter-btn.active { background: var(--ocean); border-color: var(--ocean); color: var(--white); }

.filter-btn.loc-alaska.active    { background: #4a6fa5; border-color: #4a6fa5; }
.filter-btn.loc-colorado.active  { background: #2e7d32; border-color: #2e7d32; }
.filter-btn.loc-newmexico.active { background: #b71c1c; border-color: #b71c1c; }
.filter-btn.loc-newyork.active   { background: #6a1b9a; border-color: #6a1b9a; }
.filter-btn.loc-nc.active        { background: #00695c; border-color: #00695c; }
.filter-btn.loc-bahamas.active   { background: #0277bd; border-color: #0277bd; }
.filter-btn.loc-panama.active    { background: #e65100; border-color: #e65100; }
.filter-btn.issue-land.active    { background: #558b2f; border-color: #558b2f; }
.filter-btn.issue-water.active   { background: #0288d1; border-color: #0288d1; }
.filter-btn.issue-ocean.active   { background: #01579b; border-color: #01579b; }
.filter-btn.issue-future.active  { background: #f57f17; border-color: #f57f17; }

.feed-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 0; min-height: calc(100vh - 200px); }
.feed-column { border-right: 1px solid var(--border); display: flex; flex-direction: column; }
.feed-column:last-child { border-right: none; }
.col-header { padding: 13px 18px 11px; border-bottom: 2px solid var(--border); display: flex; align-items: center; gap: 10px; position: sticky; top: 69px; background: var(--white); z-index: 50; }
.col-header.instagram { border-bottom-color: #c13584; }
.col-header.twitter   { border-bottom-color: #000; }
.col-header.facebook  { border-bottom-color: #1877f2; }
.platform-icon { width: 29px; height: 29px; border-radius: 7px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.platform-icon.instagram { background: linear-gradient(135deg,#f09433,#e6683c,#dc2743,#cc2366,#bc1888); }
.platform-icon.twitter   { background: #000; }
.platform-icon.facebook  { background: #1877f2; }
.platform-icon svg { color: white; }
.col-title { font-family: 'DM Serif Display', serif; font-size: 1rem; color: var(--ink); }
.col-count { margin-left: auto; font-size: 0.72rem; color: var(--muted); background: var(--sand); border-radius: 12px; padding: 2px 9px; font-weight: 500; }
.col-cards { padding: 9px; display: flex; flex-direction: column; gap: 6px; flex: 1; }

.org-card { background: var(--white); border: 1px solid var(--border); border-radius: 9px; padding: 10px 12px; transition: box-shadow 0.15s, transform 0.15s; }
.org-card:hover { box-shadow: var(--card-shadow); transform: translateY(-1px); }
.org-card.no-handle { opacity: 0.4; background: var(--sand); }
.card-top { display: flex; align-items: flex-start; justify-content: space-between; gap: 8px; margin-bottom: 5px; }
.org-name { font-size: 0.79rem; font-weight: 600; color: var(--ink); line-height: 1.3; flex: 1; }
.handle-link { font-size: 0.71rem; text-decoration: none; font-weight: 500; white-space: nowrap; padding: 2px 7px; border-radius: 4px; transition: background 0.15s; flex-shrink: 0; }
.handle-link.instagram { background: #fce4f3; color: #c13584; }
.handle-link.instagram:hover { background: #f5c6e8; }
.handle-link.twitter { background: #f0f0f0; color: #333; }
.handle-link.twitter:hover { background: #e0e0e0; }
.handle-link.facebook { background: #e7f0fd; color: #1877f2; }
.handle-link.facebook:hover { background: #cfe0fb; }

.card-tags { display: flex; flex-wrap: wrap; gap: 3px; margin-bottom: 5px; }
.tag { font-size: 0.62rem; font-weight: 700; padding: 1px 6px; border-radius: 3px; letter-spacing: 0.02em; text-transform: uppercase; }
.tag-alaska    { background: #dce8f5; color: #4a6fa5; }
.tag-colorado  { background: #dcedc8; color: #2e7d32; }
.tag-newmexico { background: #ffcdd2; color: #b71c1c; }
.tag-newyork   { background: #e8d5f5; color: #6a1b9a; }
.tag-nc        { background: #d0f0eb; color: #00695c; }
.tag-bahamas   { background: #d1ecfd; color: #0277bd; }
.tag-panama    { background: #ffe0cc; color: #e65100; }
.tag-land      { background: #f1f8e9; color: #558b2f; }
.tag-water     { background: #e1f5fe; color: #0288d1; }
.tag-ocean     { background: #e3f2fd; color: #01579b; }
.tag-future    { background: #fff8e1; color: #f57f17; }

.post-preview { font-size: 0.75rem; color: var(--muted); line-height: 1.45; font-style: italic; padding: 6px 8px; background: var(--sand); border-radius: 5px; border-left: 2px solid var(--border); margin-top: 5px; }
.post-date { font-size: 0.67rem; color: var(--muted); margin-top: 4px; text-align: right; font-style: normal; }
.no-handle-label { font-size: 0.7rem; color: var(--muted); font-style: italic; }
.empty-col { padding: 40px 20px; text-align: center; color: var(--muted); font-size: 0.85rem; font-style: italic; }
.dash-footer { background: var(--ocean); color: var(--sky); text-align: center; font-size: 0.75rem; padding: 14px; font-weight: 300; }

@media (max-width: 900px) {
  .feed-grid { grid-template-columns: 1fr; }
  .feed-column { border-right: none; border-bottom: 1px solid var(--border); }
  .header { padding: 20px; }
  .toolbar { padding: 10px 16px; }
  .col-header { top: 85px; }
}
</style>
</head>
<body>

<div class="header">
  <div class="header-left">
    <h1>MCF Grantee Social Dashboard</h1>
    <div class="subtitle">Social media monitoring · Instagram · X · Facebook</div>
  </div>
  <div class="header-meta">
    <div class="stat-pill"><strong id="stat-total">—</strong>Grantees</div>
    <div class="stat-pill"><strong id="stat-ig">—</strong>Instagram</div>
    <div class="stat-pill"><strong id="stat-tw">—</strong>X</div>
    <div class="stat-pill"><strong id="stat-fb">—</strong>Facebook</div>
    <div id="last-updated">Awaiting first sweep</div>
  </div>
</div>

<div class="toolbar">
  <div class="search-wrap">
    <svg width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/></svg>
    <input id="search" type="text" placeholder="Search grantees…" oninput="render()">
  </div>

  <div class="toolbar-divider"></div>
  <span class="filter-label">Location</span>
  <div class="filter-group">
    <button class="filter-btn active" data-loc="all" onclick="setLoc('all',this)">All</button>
    <button class="filter-btn loc-alaska"    data-loc="alaska"    onclick="setLoc('alaska',this)">Alaska</button>
    <button class="filter-btn loc-colorado"  data-loc="colorado"  onclick="setLoc('colorado',this)">Colorado</button>
    <button class="filter-btn loc-newmexico" data-loc="newmexico" onclick="setLoc('newmexico',this)">New Mexico</button>
    <button class="filter-btn loc-newyork"   data-loc="newyork"   onclick="setLoc('newyork',this)">New York</button>
    <button class="filter-btn loc-nc"        data-loc="nc"        onclick="setLoc('nc',this)">North Carolina</button>
    <button class="filter-btn loc-bahamas"   data-loc="bahamas"   onclick="setLoc('bahamas',this)">Bahamas</button>
    <button class="filter-btn loc-panama"    data-loc="panama"    onclick="setLoc('panama',this)">Panama</button>
  </div>

  <div class="toolbar-divider"></div>
  <span class="filter-label">Issue</span>
  <div class="filter-group">
    <button class="filter-btn active" data-issue="all" onclick="setIssue('all',this)">All</button>
    <button class="filter-btn issue-land"   data-issue="land"   onclick="setIssue('land',this)">Land &amp; Forest</button>
    <button class="filter-btn issue-water"  data-issue="water"  onclick="setIssue('water',this)">Clean Water</button>
    <button class="filter-btn issue-ocean"  data-issue="ocean"  onclick="setIssue('ocean',this)">Healthy Oceans</button>
    <button class="filter-btn issue-future" data-issue="future" onclick="setIssue('future',this)">Future Generations</button>
  </div>

  <div class="toolbar-divider"></div>
  <div class="filter-group">
    <button class="filter-btn active" data-show="all" onclick="setShow('all',this)">All grantees</button>
    <button class="filter-btn" data-show="handled" onclick="setShow('handled',this)">Has handle</button>
  </div>
</div>

<div class="feed-grid">
  <div class="feed-column">
    <div class="col-header instagram">
      <div class="platform-icon instagram">
        <svg width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="5"/><circle cx="17.5" cy="6.5" r="1" fill="currentColor" stroke="none"/></svg>
      </div>
      <span class="col-title">Instagram</span>
      <span class="col-count" id="count-ig">—</span>
    </div>
    <div class="col-cards" id="col-ig"></div>
  </div>
  <div class="feed-column">
    <div class="col-header twitter">
      <div class="platform-icon twitter">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="white"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-4.714-6.231-5.401 6.231H2.748l7.73-8.835L1.254 2.25H8.08l4.261 5.636 5.903-5.636Zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
      </div>
      <span class="col-title">X</span>
      <span class="col-count" id="count-tw">—</span>
    </div>
    <div class="col-cards" id="col-tw"></div>
  </div>
  <div class="feed-column">
    <div class="col-header facebook">
      <div class="platform-icon facebook">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="white"><path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/></svg>
      </div>
      <span class="col-title">Facebook</span>
      <span class="col-count" id="count-fb">—</span>
    </div>
    <div class="col-cards" id="col-fb"></div>
  </div>
</div>

<div class="dash-footer">
  MCF Grantee Social Dashboard · Run a sweep in Claude to populate post previews
</div>

<script>
const GRANTEES = [
  // ALASKA
  { name: "Mother Kuskokwim Tribal Coalition", ig: "motherkuskokwim", tw: "NoDonlinGold", fb_url: "https://www.facebook.com/MotherKuskokwim", fb_handle: "MotherKuskokwim", loc: ["alaska"], issues: ["water","land"] },
  { name: "SalmonState", ig: "salmonstateak", tw: "salmonstateak", fb_url: "https://www.facebook.com/SalmonStateAK", fb_handle: "SalmonStateAK", loc: ["alaska"], issues: ["water","ocean"] },
  { name: "Susitna River Coalition", ig: "sustinarivercoalition", tw: "savethesusitna", fb_url: "https://www.facebook.com/susitnarivercoalition", fb_handle: "susitnarivercoalition", loc: ["alaska"], issues: ["water","land"] },
  // BAHAMAS
  { name: "Bahamas Mangrove Alliance", ig: "bahamasmangrovealliance", tw: "", fb_url: "", fb_handle: "", loc: ["bahamas"], issues: ["ocean","water"] },
  { name: "Bahamas National Trust", ig: "bahamasnationaltrust", tw: "bntbahamas", fb_url: "https://www.facebook.com/profile.php?id=100064815787963", fb_handle: "BahamasNationalTrust", loc: ["bahamas"], issues: ["ocean","land"] },
  { name: "Bahamas Reef Environment Educational Foundation (BREEF)", ig: "ceibahamas", tw: "BREEF242", fb_url: "https://www.facebook.com/breef242", fb_handle: "breef242", loc: ["bahamas"], issues: ["ocean","future"] },
  { name: "Bonefish & Tarpon Trust", ig: "bonefishtarpontrust", tw: "bonetarpontrust", fb_url: "https://www.facebook.com/bonefishtarpontrust", fb_handle: "bonefishtarpontrust", loc: ["bahamas"], issues: ["ocean","water"] },
  { name: "Friends of the Environment", ig: "friendsoftheenvironment", tw: "", fb_url: "https://www.facebook.com/friendsoftheenvironment", fb_handle: "friendsoftheenvironment", loc: ["bahamas"], issues: ["ocean","water"] },
  { name: "The Island School (Cape Eleuthera Foundation)", ig: "ceibahamas", tw: "CEIbahamas", fb_url: "https://www.facebook.com/capeeleutherainstitute", fb_handle: "capeeleutherainstitute", loc: ["bahamas"], issues: ["ocean","future"] },
  { name: "Waterkeepers Bahamas", ig: "242waterkeepers", tw: "242Waterkeepers", fb_url: "https://www.facebook.com/242Waterkeepers", fb_handle: "242Waterkeepers", loc: ["bahamas"], issues: ["water","ocean"] },
  // COLORADO
  { name: "Bird Conservancy of the Rockies", ig: "birdconservancyrockies", tw: "BirdConservancy", fb_url: "https://www.facebook.com/birdconservancy", fb_handle: "birdconservancy", loc: ["colorado"], issues: ["land"] },
  { name: "Colorado Cattlemen's Agricultural Land Trust", ig: "ccalt_org", tw: "", fb_url: "https://www.facebook.com/CCALT", fb_handle: "CCALT", loc: ["colorado"], issues: ["land"] },
  { name: "Colorado Open Lands", ig: "coloradoopenlands", tw: "", fb_url: "https://www.facebook.com/ColoradoOpenLands", fb_handle: "ColoradoOpenLands", loc: ["colorado"], issues: ["land"] },
  { name: "Colorado Parks & Wildlife", ig: "coparkswildlife", tw: "COParksWildlife", fb_url: "https://www.facebook.com/CoParksWildlife", fb_handle: "CoParksWildlife", loc: ["colorado"], issues: ["land","water"] },
  { name: "CSU Natural Resources Ecology Laboratory", ig: "nrelecopress", tw: "NREL_EcoPress", fb_url: "https://www.facebook.com/nrelscience/", fb_handle: "nrelscience", loc: ["colorado"], issues: ["land"] },
  { name: "Defend the West Su", ig: "defendthewestsu", tw: "", fb_url: "https://www.facebook.com/defendthewestsu", fb_handle: "defendthewestsu", loc: ["colorado"], issues: ["water","land"] },
  { name: "Rio Grande Headwaters Land Trust", ig: "riograndeheadwaterslandtrust", tw: "COheadwatersLT", fb_url: "https://www.facebook.com/headwaterslandtrust", fb_handle: "headwaterslandtrust", loc: ["colorado"], issues: ["water","land"] },
  { name: "Rio Grande Headwaters Restoration Project", ig: "riogranderestoration", tw: "", fb_url: "https://www.facebook.com/riograndeheadwaters", fb_handle: "riograndeheadwaters", loc: ["colorado"], issues: ["water","land"] },
  { name: "Rio Grande Watershed Conservation & Education Initiative", ig: "", tw: "", fb_url: "https://www.facebook.com/rgwcei", fb_handle: "rgwcei", loc: ["colorado"], issues: ["water","land"] },
  { name: "Rocky Mountain Elk Foundation", ig: "rmef_official", tw: "RMEF", fb_url: "https://www.facebook.com/RMEF1", fb_handle: "RMEF1", loc: ["colorado"], issues: ["land"] },
  { name: "Rocky Mountain Youth Corps", ig: "rockymountainyouthcorps", tw: "RMYC_NM", fb_url: "https://www.facebook.com/rockymountainyouthcorps", fb_handle: "rockymountainyouthcorps", loc: ["colorado"], issues: ["land","future"] },
  { name: "Running Rivers (Flyathlon)", ig: "flyathon", tw: "", fb_url: "https://www.facebook.com/Flyathlon/", fb_handle: "Flyathlon", loc: ["colorado"], issues: ["water"] },
  { name: "Rye Resurgence Restoration Project", ig: "ryeresurgence", tw: "", fb_url: "", fb_handle: "", loc: ["colorado"], issues: ["water","land"] },
  { name: "Western Landowners Alliance", ig: "westernlandowners", tw: "VoiceofWLA", fb_url: "https://www.facebook.com/westernlandownersalliance", fb_handle: "westernlandownersalliance", loc: ["colorado"], issues: ["land"] },
  // NEW MEXICO
  { name: "Friends of the Rio Grande del Norte National Monument", ig: "riogrande_friends", tw: "", fb_url: "https://www.facebook.com/FriendsofRioGrandedelNorte", fb_handle: "FriendsofRioGrandedelNorte", loc: ["newmexico"], issues: ["land","water"] },
  { name: "The Nature Conservancy New Mexico", ig: "nature_newmexico", tw: "", fb_url: "https://www.facebook.com/New.Mexico.Nature.Conservancy", fb_handle: "New.Mexico.Nature.Conservancy", loc: ["newmexico"], issues: ["land","water"] },
  { name: "Trout Unlimited", ig: "troutunlimited", tw: "TroutUnlimited", fb_url: "https://www.facebook.com/TroutUnlimited", fb_handle: "TroutUnlimited", loc: ["newmexico","colorado"], issues: ["water","land"] },
  // NEW YORK
  { name: "Cornell Cooperative Extension of Suffolk County", ig: "cce_slc", tw: "CCESuffolkFHW", fb_url: "https://www.facebook.com/SuffolkCCE", fb_handle: "SuffolkCCE", loc: ["newyork"], issues: ["water","land"] },
  { name: "East End Seaport Museum & Marine Foundation", ig: "eastendseaportmuseum", tw: "", fb_url: "https://www.facebook.com/EastEndSeaportMuseum", fb_handle: "EastEndSeaportMuseum", loc: ["newyork"], issues: ["ocean"] },
  { name: "Group for the East End", ig: "groupfortheeastend", tw: "g4ee", fb_url: "https://www.facebook.com/GroupEastEnd", fb_handle: "GroupEastEnd", loc: ["newyork"], issues: ["land","water"] },
  { name: "GrowNYC", ig: "grownyc", tw: "GrowNYC", fb_url: "https://www.facebook.com/GrowNYC", fb_handle: "GrowNYC", loc: ["newyork"], issues: ["land","future"] },
  { name: "New York City Bird Alliance", ig: "nycbirdalliance", tw: "nycbirdalliance", fb_url: "https://www.facebook.com/nycbirdalliance", fb_handle: "nycbirdalliance", loc: ["newyork"], issues: ["land"] },
  { name: "Peconic Land Trust", ig: "peconiclandtrust", tw: "", fb_url: "https://www.facebook.com/PeconicLandTrust", fb_handle: "PeconicLandTrust", loc: ["newyork"], issues: ["land","water"] },
  { name: "Seatuck Environmental Association", ig: "seatuckenvironmental", tw: "SeatuckEnvAssoc", fb_url: "https://www.facebook.com/seatuck", fb_handle: "seatuck", loc: ["newyork"], issues: ["land","ocean"] },
  { name: "SUNY ESF College Foundation", ig: "suny.esf", tw: "sunyesf", fb_url: "https://www.facebook.com/sunyesf", fb_handle: "sunyesf", loc: ["newyork"], issues: ["land","future"] },
  // NORTH CAROLINA
  { name: "American Rivers", ig: "americanrivers", tw: "americanrivers", fb_url: "https://www.facebook.com/AmericanRivers", fb_handle: "AmericanRivers", loc: ["nc"], issues: ["water"] },
  { name: "Audubon North Carolina", ig: "audubon_nc", tw: "AudubonNC", fb_url: "https://www.facebook.com/audubonnc", fb_handle: "audubonnc", loc: ["nc"], issues: ["land"] },
  { name: "Cape Fear River Watch", ig: "capefearriverwatch", tw: "", fb_url: "https://www.facebook.com/CapeFearRiver", fb_handle: "CapeFearRiver", loc: ["nc"], issues: ["water"] },
  { name: "Dogwood Alliance", ig: "dogwoodalliance", tw: "dogwoodalliance", fb_url: "https://www.facebook.com/DogwoodAlliance", fb_handle: "DogwoodAlliance", loc: ["nc"], issues: ["land"] },
  { name: "National Forest Foundation", ig: "nationalforests", tw: "NationalForests", fb_url: "https://www.facebook.com/NationalForestFoundation", fb_handle: "NationalForestFoundation", loc: ["nc"], issues: ["land"] },
  { name: "North Carolina Coastal Land Trust", ig: "coastallandtrust", tw: "NCCoastalLT", fb_url: "https://www.facebook.com/NCCoastalLandTrust", fb_handle: "NCCoastalLandTrust", loc: ["nc"], issues: ["land","ocean"] },
  { name: "North Carolina Conservation Network", ig: "ncconservationnetwork", tw: "NCConservation", fb_url: "https://www.facebook.com/ncconservationnetwork", fb_handle: "ncconservationnetwork", loc: ["nc"], issues: ["land","water"] },
  { name: "North Carolina Wildlife Federation", ig: "ncwildlifefed", tw: "", fb_url: "https://www.facebook.com/NCWildlifeFederation", fb_handle: "NCWildlifeFederation", loc: ["nc"], issues: ["land","water"] },
  { name: "Rachel Carson Council", ig: "rachelcarsondc", tw: "rachelcarsondc", fb_url: "https://www.facebook.com/RachelCarsonCouncil", fb_handle: "RachelCarsonCouncil", loc: ["nc"], issues: ["water","ocean"] },
  { name: "Southern Environmental Law Center", ig: "southernenvironment", tw: "selc_org", fb_url: "https://www.facebook.com/southernenvironment", fb_handle: "southernenvironment", loc: ["nc"], issues: ["land","water"] },
  { name: "The Nature Conservancy North Carolina", ig: "tnc_nc", tw: "nc_tnc", fb_url: "https://www.facebook.com/TNCNC", fb_handle: "TNCNC", loc: ["nc"], issues: ["land","water"] },
  // PANAMA
  { name: "Environmental Advocacy Center / CIAM (Geoversity)", ig: "ciampanama", tw: "ciampanama", fb_url: "https://www.facebook.com/ciampanama", fb_handle: "ciampanama", loc: ["panama"], issues: ["land"] },
  { name: "Geoversity Foundation (Fundacion Amador)", ig: "geoversity", tw: "geoversity", fb_url: "https://www.facebook.com/Geoversity", fb_handle: "Geoversity", loc: ["panama"], issues: ["land","future"] },
  { name: "Panama Audubon Society", ig: "audubonpanama", tw: "audubonpanama", fb_url: "https://www.facebook.com/audubonpanama", fb_handle: "audubonpanama", loc: ["panama"], issues: ["land","ocean"] },
  { name: "Rainforest Foundation U.S.", ig: "rainforestus", tw: "rainforestus", fb_url: "https://www.facebook.com/RainforestUS", fb_handle: "RainforestUS", loc: ["panama"], issues: ["land"] },
  { name: "Smithsonian Tropical Research Institute", ig: "smithsonianpanama", tw: "stri_panama", fb_url: "https://www.facebook.com/SmithsonianPanama", fb_handle: "SmithsonianPanama", loc: ["panama"], issues: ["ocean","land"] },
  { name: "Wetlands International", ig: "wetlands_international", tw: "WetlandsInt", fb_url: "https://www.facebook.com/wetlandsint", fb_handle: "wetlandsint", loc: ["panama"], issues: ["water","land"] },
  // NATIONAL / INTERNATIONAL (no location tag)
  { name: "Biofuelwatch", ig: "biofuelwatch", tw: "", fb_url: "https://www.facebook.com/Biofuelwatch", fb_handle: "Biofuelwatch", loc: [], issues: ["land"] },
  { name: "Coral Vita", ig: "coralvitareefs", tw: "CoralVitaReefs", fb_url: "https://www.facebook.com/CoralVitaReefs", fb_handle: "CoralVitaReefs", loc: [], issues: ["ocean"] },
  { name: "Everglades Foundation", ig: "evergladesfoundation", tw: "evergfoundation", fb_url: "https://www.facebook.com/evergladesfoundation", fb_handle: "evergladesfoundation", loc: [], issues: ["ocean","water"] },
  { name: "Global Fishing Watch", ig: "globalfishingwatch", tw: "GlobalFishingWatch", fb_url: "https://www.facebook.com/GlobalFishingWatch", fb_handle: "GlobalFishingWatch", loc: [], issues: ["ocean"] },
  { name: "Land Trust Alliance", ig: "ltalliance", tw: "LTAlliance", fb_url: "https://www.facebook.com/landtrustalliance", fb_handle: "landtrustalliance", loc: [], issues: ["land"] },
  { name: "MarViva", ig: "fundacionmarviva", tw: "MarVivaBallenas", fb_url: "https://www.facebook.com/FundacionMarViva", fb_handle: "FundacionMarViva", loc: [], issues: ["ocean"] },
  { name: "Mote Marine Laboratory", ig: "motemarinelab", tw: "motemarinelab", fb_url: "https://www.facebook.com/MoteMarineLab", fb_handle: "MoteMarineLab", loc: [], issues: ["ocean"] },
  { name: "National Audubon Society", ig: "audubonsociety", tw: "audubonsociety", fb_url: "https://www.facebook.com/NationalAudubonSociety", fb_handle: "NationalAudubonSociety", loc: [], issues: ["land"] },
  { name: "Natural Resources Defense Council", ig: "nrdc_org", tw: "NRDC", fb_url: "https://www.facebook.com/nrdc.org", fb_handle: "nrdc.org", loc: [], issues: ["water","land","ocean"] },
  { name: "Only One", ig: "onlyone", tw: "onlyone", fb_url: "https://www.facebook.com/OnlyOneOceanPlatform", fb_handle: "OnlyOneOceanPlatform", loc: [], issues: ["ocean"] },
  { name: "Panacetacea", ig: "panacetacea", tw: "panacetacea", fb_url: "https://www.facebook.com/Panacetacea", fb_handle: "Panacetacea", loc: [], issues: ["ocean"] },
  { name: "Perry Institute for Marine Science", ig: "perryinstituteformarinescience", tw: "perryinstitute", fb_url: "https://www.facebook.com/perryinstituteformarinescience", fb_handle: "perryinstituteformarinescience", loc: [], issues: ["ocean"] },
  { name: "Plastic Pollution Coalition", ig: "plasticpollutes", tw: "PlasticPollutes", fb_url: "https://www.facebook.com/PlasticPollution", fb_handle: "PlasticPollution", loc: [], issues: ["ocean","water"] },
  { name: "SeaLegacy", ig: "sealegacy", tw: "sea_legacy", fb_url: "https://www.facebook.com/sealegacy", fb_handle: "sealegacy", loc: [], issues: ["ocean"] },
  { name: "Shark Conservation Fund", ig: "sharkconservationfund", tw: "SharkRayFund", fb_url: "https://www.facebook.com/sharkconservationfund", fb_handle: "sharkconservationfund", loc: [], issues: ["ocean"] },
  { name: "The Leatherback Project", ig: "leatherbackproject", tw: "LeatherbackProj", fb_url: "https://www.facebook.com/leatherbackproject", fb_handle: "leatherbackproject", loc: [], issues: ["ocean"] },
  { name: "The Nature Conservancy", ig: "nature_org", tw: "nature_org", fb_url: "https://www.facebook.com/thenatureconservancy", fb_handle: "thenatureconservancy", loc: [], issues: ["land","water","ocean"] },
  { name: "The Oxygen Project", ig: "theoxygenproj", tw: "theoxygen_proj", fb_url: "https://www.facebook.com/theoxygenproj", fb_handle: "theoxygenproj", loc: [], issues: ["land"] },
  { name: "Theodore Roosevelt Conservation Partnership", ig: "thetrcp", tw: "thetrcp", fb_url: "https://www.facebook.com/TheTRCP", fb_handle: "TheTRCP", loc: [], issues: ["land","water"] },
  { name: "Wild Salmon Center", ig: "wildsalmoncenter", tw: "wildsalmoncntr", fb_url: "https://www.facebook.com/thewildsalmoncenter", fb_handle: "thewildsalmoncenter", loc: [], issues: ["water","ocean"] },
];

const POST_DATA = /*POST_DATA*/{};

const LOC_LABELS   = { alaska:"Alaska", colorado:"Colorado", newmexico:"New Mexico", newyork:"New York", nc:"North Carolina", bahamas:"Bahamas", panama:"Panama" };
const ISSUE_LABELS = { land:"Land & Forest", water:"Clean Water", ocean:"Healthy Oceans", future:"Future Generations" };

let showMode='all', activeLoc='all', activeIssue='all';

function setShow(v,el)  { showMode=v;    document.querySelectorAll('[data-show]').forEach(b=>b.classList.remove('active')); el.classList.add('active'); render(); }
function setLoc(v,el)   { activeLoc=v;   document.querySelectorAll('[data-loc]').forEach(b=>b.classList.remove('active'));  el.classList.add('active'); render(); }
function setIssue(v,el) { activeIssue=v; document.querySelectorAll('[data-issue]').forEach(b=>b.classList.remove('active')); el.classList.add('active'); render(); }

function igUrl(h){return`https://www.instagram.com/${h}/`;}
function twUrl(h){return`https://x.com/${h}`;}

function tagsHtml(g){
  let h='';
  g.loc.forEach(l=>{h+=`<span class="tag tag-${l}">${LOC_LABELS[l]}</span>`;});
  g.issues.forEach(i=>{h+=`<span class="tag tag-${i}">${ISSUE_LABELS[i]}</span>`;});
  return h?`<div class="card-tags">${h}</div>`:'';
}

function cardHtml(g,platform){
  let handle,url,cls,postKey;
  if(platform==='ig'){handle=g.ig;url=handle?igUrl(handle):'';cls='instagram';postKey=`ig_${handle}`;}
  else if(platform==='tw'){handle=g.tw;url=handle?twUrl(handle):'';cls='twitter';postKey=`tw_${handle}`;}
  else{handle=g.fb_handle;url=g.fb_url;cls='facebook';postKey=`fb_${handle}`;}

  if(!handle&&!url){
    if(showMode==='handled')return'';
    return`<div class="org-card no-handle"><div class="card-top"><span class="org-name">${g.name}</span><span class="no-handle-label">No handle</span></div>${tagsHtml(g)}</div>`;
  }
  const post=POST_DATA[postKey];
  const disp=platform==='fb'?handle:`@${handle}`;
  let ps=`<div class="post-preview">No post data yet — run a sweep to populate.</div>`;
  if(post&&post.text){ps=`<div class="post-preview">${post.text}</div><div class="post-date">${post.date||''}</div>`;}
  return`<div class="org-card"><div class="card-top"><span class="org-name">${g.name}</span><a class="handle-link ${cls}" href="${url}" target="_blank" rel="noopener">${disp}</a></div>${tagsHtml(g)}${ps}</div>`;
}

function render(){
  const q=document.getElementById('search').value.toLowerCase().trim();
  const filtered=GRANTEES.filter(g=>{
    if(q&&!g.name.toLowerCase().includes(q))return false;
    if(activeLoc!=='all'&&!g.loc.includes(activeLoc))return false;
    if(activeIssue!=='all'&&!g.issues.includes(activeIssue))return false;
    return true;
  });
  let igHtml='',twHtml='',fbHtml='',igCount=0,twCount=0,fbCount=0;
  filtered.forEach(g=>{
    if(showMode==='handled'){
      if(g.ig){igHtml+=cardHtml(g,'ig');igCount++;}
      if(g.tw){twHtml+=cardHtml(g,'tw');twCount++;}
      if(g.fb_handle||g.fb_url){fbHtml+=cardHtml(g,'fb');fbCount++;}
    }else{
      igHtml+=cardHtml(g,'ig');
      if(g.tw){twHtml+=cardHtml(g,'tw');twCount++;}
      fbHtml+=cardHtml(g,'fb');
      if(g.ig)igCount++;
      if(g.fb_handle||g.fb_url)fbCount++;
    }
  });
  document.getElementById('col-ig').innerHTML=igHtml||'<div class="empty-col">No results</div>';
  document.getElementById('col-tw').innerHTML=twHtml||'<div class="empty-col">No results</div>';
  document.getElementById('col-fb').innerHTML=fbHtml||'<div class="empty-col">No results</div>';
  document.getElementById('count-ig').textContent=`${igCount} accounts`;
  document.getElementById('count-tw').textContent=`${twCount} accounts`;
  document.getElementById('count-fb').textContent=`${fbCount} accounts`;
  document.getElementById('stat-ig').textContent=igCount;
  document.getElementById('stat-tw').textContent=twCount;
  document.getElementById('stat-fb').textContent=fbCount;
  document.getElementById('stat-total').textContent=filtered.length;
}

const now=new Date().toLocaleString('en-US',{month:'short',day:'numeric',hour:'numeric',minute:'2-digit'});
document.getElementById('last-updated').textContent=`Updated ${now}`;
render();
</script>
</body>
</html>
