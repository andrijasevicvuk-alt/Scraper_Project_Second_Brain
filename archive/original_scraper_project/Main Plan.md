Scraper_project is a side project I'm doing for another project named YPI (a boat valuation tool). What i want to make here is a scraper that works and successfully can bypass my sources defensive measures against automated scraping. Chat GPT Plus and codex don't want to help me with the ==evasion of CAPTCHA, anti bot measures, 403 evasion== and because of that i need to put more effort into this scraper. Gemini on the other hand will help me with the coding  for defence bypass tactics so this folder was created because i wanted to explain to myself what I'm doing. 

I'm working in a repo on my laptop at the moment named "scaper_project" and the plan is to successfully scrape my main targets (sources) by creating a ==Strict Separation of Concerns (SoC)== architecture, meaning we are going to divide the project into 3 parts and define which ai can work on which part. We are using this architecture so that codex or chat GPT don't delete and replace my defence evasion code that i will write with gemini.

- ==Gemini== is going to help me create the defence zone, it will write the core engine of the scraper
- ==Chat GPT== will design the data structure, it will write rules that make messy strings into clean integers
- ==Codex== will will write raw, pure HTML parsing functions that pull out data like price and title (using BS4 or Selectolax)

The end goal of this project is to make a ==data readiness lab== out of it using codex and Chat GPT when we confirm that the architecture that i will make with Gemini can extract the data i need from my targes. I want to score the data i scrape and make a data set that is at a high enough level for my YPI project. 

I want to use a ==vertical spike approach== with this project, meaning i want to uncover the "Defence Ceiling" by using a one source first approach. I will be testing out one source after another. There will be ==one unified core engine== and then for each source i will have lightweight ==Parser scripts== that will contain the text extraction rules. If one of my sources defence is too strong then my core engine does not have to change but i will only have to swap out one specific parser file.

Another key information about this project is that it is dividend into 2 scraping phases. The phase 1 "Genesis Scrape" and the phase 2 "Routine Scrape". The genesis scrape is the first scrape that i will do, and it has to be complete. With it we need to make a fundational dataset that supports all the future routine scrape. With this division i want to also add that my goal is to create one data set after another. For example the first source i want to scrape is boat 24, when im sure i made a complete scrape of all the data i need i will score it and connect it with the YPI project. When one source is confirmed to produce a valuable data set i will continue to the next one. 
#### [[Dual-Node Infrastructure]]
I'm going to make a dual-node infrastructure so that i can continue to work from my summer job on this project. At the moment am using my ==ThinkPad L15 Gen 2== laptop and its running on my phones hotspot, so i cant really do any heavy lifting with it. I want to use my ==home pc== as a worker node and my think pad at the beach like my control node. GitHub and Remote Access will act as my bridge between the worker node and the control node.
##### PC specs

| Component        | Specification                               |
| ---------------- | ------------------------------------------- |
| **GPU**          | Zotac Gaming GeForce GTX 1660 (likely 6 GB) |
| **CPU**          | AMD Ryzen 5 1600 (6 cores / 12 threads)     |
| **Motherboard**  | ASUS Prime B450-Plus                        |
| **RAM**          | 16 GB Corsair DDR4 (exact speed unknown)    |
| **Power Supply** | Corsair CV650 (650 W, 80+ Bronze)           |
| **SSD**          | 1 TB SSD                                    |
| **Hard Drive**   | 2 TB HDD                                    |
| **Build Year**   | 2021                                        |

#### [[The sources]] (targets)
The sources that i want to scrape are:
- The Yacht Market 
- Boat24
- MarineOne
- Croatian Yachting 
- iNautia (alternatives: Band of Boats, Yacht Focus)
- Njuškalo Nautika (alternatives: Burza Nautike, Index Oglasi/Nautika, Mornar.net)

The note i created for the sources has the description of every sources defence, volume, geography and info codex found for me based on its attempts to scrape.

#### The master inventory

One thing i want to say, before i made this inventory just in case this agentic scraper named Scrapling doesn't work. I still have tested out its performance and if it works, my plan was to run it in a isolated container and make a test run on Boat24 on my pc. If Scrapling doesn't work my plan was to make the scraping engine myself and not use ai.  

##### Infrastructure and Network Layer:
- ==The Control Node== (Thinkpad): my laptop from which i work on the project 
- ==The Worker Node== (Home PC): home pc that does all the heavy lifting
- ==Private GitHub Repository==: the bridge between the two nodes
- ==DataImpulse Proxies Split-Pool==: the international rotating pool and the croatian sticky residential pool
- ==Dual-Pool DNS Isolation==: network logic configuration that ensure DNS queries are resolved remotely

##### Core engine and orchestration:
- ==Crawlee-python== (GitHub): he master orchestrator. Handles your request queues, concurrency scaling, automatic retry logic, and proxy rotation.
- ==Curl_cffi ==(GitHub): Your primary lightweight engine. Acts like standard Python requests but spoofs desktop browser TLS/JA4 signatures and HTTP/2 settings. Ideal for high-speed scraping on Tier 2/3 targets (Boat24, Band of Boats) without browser overhead.
- ==Nodriver== (GitHub): My heavy stealth browser. It connects directly over the Chrome DevTools Protocol (CDP) without exposing WebDriver flags, making it virtually invisible to Radware and Cloudflare Enterprise (Njuškalo, iNautia).
- ==Brotli and Zstd Compression forcing==: custom headers that force servers to heavily compress HTML text payloads
- ==Route Asset Interception==: code filters that make sure i only spend data on raw HTML text

##### Biometric Evasion Layer:
- ==OxyMouse== or ==HumanMoveMouse ==(GitHub): pre-built frameworks used to generate realistic mouse movements
- ==Jittery Scrolling Routines==: Custom python loops that replace constant,  linear webpage scrolling with humanized scrolling routines.
- ==Input.dispatchKeyEvents== (Native CDP): low-level browser commands that bypass basic JavaScript text injections, It mimics a real persons typing
- ==Header-Locality Synchronization==: manual layout rules that injects native localization string values whenever the engine utilizes a Croatian residential proxy node. 

##### Data Parsing and Storage Layer
- ==pydantic== (v2): Data gatekeeper. Validates types and sanitizes messy raw data into strict, structured models
- ==selectolax==: a fast HTML parsing library, used to shred text out of HTML layout trees faster than BeautifulSoup.
- ==SQLite3 Engine== (cache.db): SQL database file that stores my scraper listings locally on my Home PC
- ==Delta Cache Architecture==: My primary proxy-saving mechanism. The scraper extracts the unique ID and price from the main search pages first, checking them against cache.db. If the listing exists and the price hasn't changed, it skips the heavy listing page download and just updates a timestamp.
- ==Isolated Parser Factory== (src/parsers/): a strict organized directory of pure python scripts containing nothing but offline extraction logic-keeping your parsers completely separate from proxies or network code.

##### Security and configuration files
- ==.env file====: a local, hidden file containing my private DataImpulse proxy credentials and system paths. This file stays only on my physical machines 
- ==.gitingnore file==: instructs Git to block my venv, env, cache databases, and system logs from ever leaking onto public GitHub repositories.
- ==loguru== telemetry: advanced local logging that captures un-parsable HTML anomalies and dumps them into offline text dumps for easy debugging later on my laptop 

##### Repository map

C:\Users\HT-ICT\Desktop\scraper_project\
│
├── .gitignore                  <-- Blocks venv/, .env, cache.db from leaking to GitHub
├── .env                        <-- Stores local DATAIMPULSE_PROXY credentials securely
├── cache.db                    <-- The local SQLite database file (The Delta Ledger)
├── requirements.txt            <-- Lists pip packages: crawlee, curl_cffi, nodriver, etc.
│
└── src/
    ├── __init__.py             
    ├── engine.py               <-- Light/Heavy network routing (curl_cffi & nodriver live here)
    ├── database.py             <-- Delta Cache Architecture logic (checks IDs & prices)
    ├── models.py               <-- Pydantic (v2) data validation models & schemas
    ├── pipeline.py             <-- Apify Crawlee orchestrator (manages loops & retry queues)
    │
    ├── stealth/                <-- THE HUMAN ILLUSION ZONE
    │   ├── __init__.py
    │   ├── biometrics.py       <-- OxyMouse Bézier path calculations & jitter scroll loops
    │   └── headers.py          <-- Header-Locality configs (Croatian translation rules)
    │
    └── parsers/                <-- THE PLUG-INS ZONE (Pure Selectolax offline extraction)
        ├── __init__.py
        ├── njuskalo.py         <-- Njuškalo HTML text extraction
        ├── theyachtmarket.py   <-- TheYachtMarket HTML text extraction
        ├── boat24.py           <-- Boat24 HTML text extraction
        └── marineone.py        <-- MarineOne HTML text extraction

#### Hybrid Delta + Priority rules + stale refresh + quality scoring architecture (this architecture is more orientated for phase 2 )

##### What i want to use:
Source registry
   ↓
List-page discovery crawl
   ↓
Extract listing key + URL + price + title + visible specs
   ↓
Compare against local DB
   ↓
Fetch detail page only if needed
   ↓
Save raw snapshot
   ↓
Pipeline: extract → normalize → validate → dedupe → quality score
   ↓
Publish only eligible records to valuation-ready layer

##### The important decision rule should be:
Fetch detail page if:
1. listing is new
2. detail snapshot does not exist yet
3. price changed and source does not expose enough list-level detail
4. title/spec fingerprint changed
5. listing is high-priority for target models
6. stored record has missing critical fields
7. last detail refresh is older than your refresh window
8. parser version changed and you need a new sample/fixture

##### Otherwise, only update:
last_seen_at
visible price
visible title/specs
listing_status
price_history if price changed

That fits my existing model, because the normalized listing layer already expects `source_listing_key`, `first_seen_at`, `last_seen_at`, `listing_status`, `price_eur`, `parse_confidence`, `data_quality_score`, and duplicate tracking.

#### The scraping schedule 

This part i still haven't really worked out. My main plan was to send the obsidian folder to chat gpt 5.6 and see what would be the most efficient way to maintain my foundational dataset created with the genesis scrape with routine scrapes. Another important information that I'm not sure if i wrote down is that I'm using residential proxies provided my DataImpulse for my scrapes, my plan was to spend around 30 gb of proxies for the genesis scrape, but for the routine scrapes i wanted to have a goal of 5 gb of proxies a month. 