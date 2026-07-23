This note here should include all the info I gathered on my targets. All this info was gathered before I started to use any tactics to evade the sources anti bot measures.

My plan is to first of all write down the info that my repo in codex (data-readiness-lab) gathered, then am going to add at the end info that Gemini found for the anti bot measures these web sites use. Then I'm going to also explain in which way each of the sources will be tackled (which architecture I'm going to use).
##### [TheYachtMarket](https://www.theyachtmarket.com/en/?srsltid=AfmBOop7ymL4qMM_qhxbfI-gy5NtYdxLC4Oa1R5n0TO-BwqBneZfLbqe) 

This was actually the best source we found. Codex tried to scrape 2 waves of websites and this was the biggest one that actually worked. 

Info that i got from ==codex== about TheYachtMarket:

- Public target used: `https://www.theyachtmarket.com/en`
- Target confidence: `official_public_url`
- What we tested: homepage, `/boats-for-sale/`, Croatia sale page, raw detail-link discovery, then a tiny parser on confirmed detail pages.
- Access result: homepage `200`, listing page `200`, `robots.txt` `200`, `sitemap.xml` `200`
- Defense/blocking: no major anti-bot defense was hit in our tiny sample.
- Detail discovery result: strongest of the group. We tested 5 candidate links and confirmed 3 probable public detail pages with stable `/id.../` URLs.
- What raw public pages exposed: listing title, price, currency, builder/model, year, location, LOA, engine, fuel, boat type, and image presence.
- Parser result: `parser_prototype_success`
- Weak or missing fields: broker/seller field was weak or missing; structured data was not a strong dependency.
- Current conclusion: this is the strongest current broader marketplace source and the first real adapter candidate.

Example extracted fields from parser sample:

- `Beneteau Oceanis Clipper 373`
- price `Ł64,950 GBP`
- location `Oban, Argyll, United Kingdom`
- LOA `11.25 m`
- engine `Volvo Penta MD2040 marine diesel engine`

Now i also want to add what ==gemini== said about this source:

- **Volume & Scope:** Very High (~50,000+ global listings). Strongest broader marketplace source and the primary anchor candidate for your MVP.
    
- **Defense Profile:** Low / No meaningful defense. Confirmed by Codex with direct `200 OK` status codes across the homepage, sitemap, `robots.txt`, and deep listing pages.
    
- **Extraction Strategy:** The Lightweight Engine (`curl_cffi`).
    
- **Toolkit Requirements:**
    
    - International Rotating Proxies (simply to safely distribute load over thousands of pages, not to bypass firewalls).
        
    - Brotli/Zstd compression forcing (`Accept-Encoding: br, gzip, zstd`).
        
    - `Selectolax` parser configured for standard, raw HTML tags.
        
- **Obsidian Note:** The ideal target for early development. Because it lacks aggressive JavaScript/WAF barriers, you can retrieve highly structured boat schemas (LOA, engine, price, currency, locations) using simple, low-overhead HTTP requests. Keep the request pipeline lightweight to protect your 5GB data cap.
    

Probable Errors & How to Fix Them:

> **Error 1: Weak or Missing Seller/Broker Data**
> 
> - **Symptom**: The parser successfully extracts core vessel specs (like the Beneteau 373 data) but consistently fails or returns empty fields for the listing broker or seller.
>     
> - **Fix**: Do not rely on a single, strict class selector for the broker card. Implement a fallback parser cascade in your `selectolax` parser: first look for the primary broker container, and if that is missing, write a regex rule to search the raw response body for common broker anchor strings (e.g., `text() contains "Presented by"` or `"Contact Seller"`).
>     

> **Error 2: Mixed Unit Formats (LOA / Engines)**
> 
> - **Symptom**: The scraper pulls a length of `"11.25 m"` on one listing and `"36 ft"` on another. Trying to write these directly to a numeric database column will crash the pipeline.
>     
> - **Fix**: Pass all extracted strings through your Pydantic (v2) validation models in `src/models.py`. Write a custom validator that detects the unit suffix, standardizes the metric (e.g., multiplying feet by `0.3048` to convert to meters), and returns a clean float value.
>     

> **Error 3: Silent Schema Shifts on Private/Broker Ads**
> 
> - **Symptom**: Your parser works flawlessly on 90% of pages but breaks with an `AttributeError` on others because private-party listings use a slightly different HTML structure than professional broker ads.
>     
> - **Fix**: Implement the parsing functions inside a safe dictionary extractor (using `.get()` values) rather than direct element chains. Validate your output schema against your Pydantic base model with strict default values (e.g., `location: str = "Unknown"`) to prevent a single missing field from halting the entire Crawlee execution loop.
>
##### [Boat24](https://www.boat24.com/en/)

Boat24 was a fail, we could not scrape anything because of the 403, and we just labelled it as blocked. 

This what ==codex== wrote down about Boat24:

- Public target used: `https://www.boat24.com`
- Target confidence: `official_public_url`
- What we tested: only obvious public entry points plus `robots.txt` and `sitemap.xml`
- Access result: probed public page returned `403`, `robots.txt` `200`, `sitemap.xml` `403`
- Defense/blocking: clear HTTP `403` stop signal on public probing, including sitemap path behavior.
- What we did after that: stopped immediately, per repo rules.
- What we did not do: no field probe, no detail discovery, no parser, no rendering, no bypass attempts.
- Current conclusion: `blocked_403_do_not_bypass`
- Product implication: not needed for MVP if TheYachtMarket remains stable; maybe revisit later only via permission/feed/API or later research.

This is what ==gemini== found out about Boat24:

- **Volume & Scope:** Very High. A major European marine listing marketplace.
    
- **Defense Profile:** High. Protected by Cloudflare Bot Management (TLS fingerprint checks + automatic challenge loops).
    
- **Extraction Strategy:** **Session Sync Bridge (Hybrid `nodriver` + `curl_cffi`)**.
    
- **Toolkit Requirements:**
    
    - `nodriver` on startup to capture session clearance.
        
    - `curl_cffi` (impersonating `chrome120+`) using sticky residential proxies for index and listing extraction.
        
    - Shared SQLite Session Store to hold cookies and headers.
        
- **Obsidian Note:** The `403` encountered by standard crawlers is a default TLS fingerprint block. By spoofing the exact browser JA3/JA4 signature using `curl_cffi` and passing the cookies generated by an active `nodriver` session, we bypass the main firewall edge entirely. This gives us lightning-fast crawling speeds without the high CPU overhead of running a live browser for all requests.
    

Probable Errors & How to Fix Them

> **Error 1: Session Desynchronization (Sudden 403 Mid-Crawl)**
> 
> - **Symptom**: The crawler runs fine for 50 requests, then suddenly throws consistent `403 Forbidden` errors on all subsequent listing pages.
>     
> - **Fix**: Implement an automatic session-refresh trigger. If `curl_cffi` receives a `403` or a redirect to a challenge page, pause the queue, execute the background `nodriver` cookie generator script, update the SQLite Session Store, and resume the queue with the new headers.
>     

> **Error 2: Proxy IP and TLS Mismatch**
> 
> - **Symptom**: Cloudflare flags and challenges the request even though the correct cookies are copied into `curl_cffi`.
>     
> - **Fix**: Cloudflare matches the IP address and the TLS fingerprint of the client that originally solved the challenge. Ensure that when `curl_cffi` takes over, it uses the **exact same sticky proxy IP session port** and the **exact same user-agent string** that `nodriver` used to initialize the bypass.
>

##### [Marine One / YachtBrokerage](https://www.yachtbrokerage.eu/)

Marine One / YachtBrokerage was the second and last source i could scrape using codex. Its main issue was the unreliable were details about the engine and the location of the boats. This is what ==codex== wrote:

- Public target used: `https://www.yachtbrokerage.eu`
- Important correction: we first had a wrong assumed domain, `https://www.marineone.com.hr`, which failed. We then corrected to the active public YachtBrokerage site carrying Marine One branding.
- Target confidence: `official_public_url`
- What we tested: homepage, `/boats-for-sale`, field probe, rendered-text probe, tiny parser, and a regression case.
- Access result: homepage `200`, listing page `200`, `robots.txt` `200`, `sitemap.xml` `200`
- Defense/blocking: no hard block like 403. The main issue is not access denial but incomplete/reliable field exposure in raw HTML.
- What raw listing/detail pages exposed reliably: title, price, currency, builder, model, year, LOA, image presence, structured-data presence, stable listing URL.
- What stayed weak: listing-specific location and engine were not reliably extractable across the raw parser flow.
- Field probe result: `candidate_accessible_fields_visible`
- Parser result: `parser_prototype_partial`
- Current sample size: 4 parsed detail pages
- Why partial instead of success: core commercial fields parse well, but `location_hint` and `engine_hint` remain unreliable in the parser.

Rendered-text / regression findings:

- We added the Astrea 42 regression page:  
    [Marine One Astrea 42](https://www.yachtbrokerage.eu/marine-one-brokerage/fountaine-pajot-astrea-42/code-\(1051\)/2025)
- We specifically checked whether full response text and rendered text contained `croatia` and `docked`.
- Result: both raw full-response text and rendered text contained `croatia` and `docked`, and the phrase `docked in croatia` was found.
- Meaning: listing-level location can exist, but it may appear deeper in response/template flow than the tiny parser’s current fetch cap catches reliably.
- Engine evidence still remained weak.

Current conclusion:

- good early local broker trust-anchor
- not blocked
- parser is useful
- still not fully reliable for location/engine without further hardening
- classification remains `parser_prototype_partial`

This is what ==gemini== had to say about Marine One / YachtBrokerage:

- **Volume & Scope:** Low to Moderate (~150+ listings). A highly authoritative local broker in Croatia and Europe, serving as a critical localized trust-anchor for your MVP.
    
- **Defense Profile:** Low / Template Friction. No active IP blocks, but highly unstable, un-classed HTML structures that shift between listings.
    
- **Extraction Strategy:** **Lightweight Engine (`curl_cffi`) with Deep Text-Matching Fallbacks**.
    
- **Toolkit Requirements:**
    
    - `Selectolax` parser with fuzzy keyword fallbacks.
        
    - Body-wide regex scanners to parse raw full-response text.
        
- **Obsidian Note:** Highly accessible. Our goal here is structural stability. Since listing templates differ wildly based on who uploaded the broker listing, we ignore strict DOM paths. Instead, we scan the text elements directly to pull critical specs like engines, location, and length.
    

Probable Errors & How to Fix Them

> **Error 1: Missing Listing-Level Location Fields (`location_hint` Parser Failure)**
> 
> - **Symptom**: The parser runs successfully but returns `None` or an empty string for the location, even though "Croatia" or specific marina names are visible on the rendered page.
>     
> - **Fix**: Implement a body-wide regex fallback scanner. If your primary CSS selector for location yields nothing, search the entire raw HTML response text using a pattern like:
>     
> 
> Python
> 
> ```
> re.compile(r"(?i)\bdocked\s+in\s+([A-Za-z\s]+)")
> ```
> 
> This allows you to extract critical location hints directly out of unstructured paragraphs.

> **Error 2: Unstable Engine Specification Layout**
> 
> - **Symptom**: Engine parameters (like HP, model, and fuel type) are buried inside unformatted text blocks or arbitrary table cells, leading to partial parsing failures.
>     
> - **Fix**: Write an unstructured text-block parser that captures the main description element, tokenizes the text, and checks it against a targeted list of keywords (e.g., `["volvo", "yanmar", "diesel", "hp", "inboard"]`). Pass the matched segments into your Pydantic validation models to cleanly rebuild the schema.
>

##### [Croatian Yachting](https://www.croatia-yachting.hr/en)

This is what info ==codex== found: 

- Public target used: `https://www.croatia-yachting.hr/en`
- Corrected public pages:
    - `https://www.croatia-yachting.hr/en/yachts-for-sale`
    - `https://www.croatia-yachting.hr/en/yachts-for-sale/used-boats`
- Target confidence: `official_public_url`
- What we tested: homepage, yachts-for-sale page, used-boats page, field probe, one rendered-text comparison.
- Access result: homepage `200`, listing page `200`, `robots.txt` `200`, `sitemap.xml` `404`
- Defense/blocking: not a 403-blocked source. The problem is that raw HTML does not expose enough safely usable listing detail structure.
- What raw HTML exposed: title-like signals, year-like values, location-like text, some structured-data markers.
- What raw HTML did not safely expose: a clear small set of detail-page URLs, clear price/currency, clear LOA, clear image-card evidence.
- Rendered-text check: the used-boats page mentioned Croatia, but we did not confirm a listing-level phrase like `docked in Croatia`.
- Parser status: no parser built.
- Current conclusion:
    - publicly reachable
    - not blocked by hard defense
    - but raw HTML discovery is incomplete
    - likely needs browser-rendered discovery to be useful
- Current classifications in the repo:
    - `candidate_accessible_needs_browser_rendering`
    - later finalized as `rendered_adapter_candidate`

And this is what info ==gemini ==found: 

- **Volume & Scope:** Low to Moderate. Highly prominent Croatian regional charter and used boat dealership.
    
- **Defense Profile:** Medium / Client-Side Rendering (CSR). No active blockwalls, but standard text crawlers will retrieve an empty body shell because listing grids and prices require JavaScript to render.
    
- **Extraction Strategy:** **Client-Side Rendering Parser (`nodriver` with dynamic hydration delays)**.
    
- **Toolkit Requirements:**
    
    - `nodriver` configured with explicit element visibility waits.
        
    - DataImpulse rotating proxy to mimic native regional Croatian visitors.
        
- **Obsidian Note:** Classified as a client-side rendered target. Since there is no heavy WAF protecting the site, we can use `nodriver` safely without complex cookie handoffs. The key is allowing the target page's Javascript framework (like React or Vue) to finish initializing before trying to read the DOM.
    

Probable Errors & How to Fix Them

> **Error 1: Stalled Discovery Loop Due to Sitemap 404**
> 
> - **Symptom**: Your crawler relies on parsing the site's XML sitemap to discover new used boat URLs, but the request returns a 404 Not Found, breaking the discovery step.
>     
> - **Fix**: Bypass sitemap discovery entirely by hardcoding the starting index pages directly in your crawl manager configuration:
>     
> 
> Python
> 
> ```
> start_urls = [
>     "https://www.croatia-yachting.hr/en/yachts-for-sale/used-boats"
> ]
> ```
> 
> Configure the headless browser to load these pages and dynamically click pagination elements or scroll to discover listing detail links.

> **Error 2: Empty Element Scraping (Dynamic DOM Hydration)**
> 
> - **Symptom**: The parser attempts to scrape price or LOA but extracts empty strings because the JS script that populates those elements hasn't finished executing when the page loads.
>     
> - **Fix**: In your Playwright / Crawlee script, force the crawler to wait until a specific listing container (e.g., `.yacht-card-details` or similar) is fully visible and populated in the DOM before passing the document to selectolax for extraction.
>
#### [iNautia](https://www.inautia.com/)

- Public target used: `https://www.inautia.com`
- Target confidence: `official_public_url`
- What we tested: only a minimal public access check plus `robots.txt` and `sitemap.xml`
- Access result: public page `403`, `robots.txt` `403`, `sitemap.xml` `403`
- Defense/blocking: strongest stop-signal pattern of the set; even the standard discovery endpoints returned `403`.
- What we did after that: stopped immediately.
- What we did not do: no field probe, no detail discovery, no parser, no rendering, no bypass attempts.
- Current conclusion: `blocked_403_do_not_bypass`

**Short takeaways**

- Best usable source so far: `TheYachtMarket`
- Best local broker source so far: `Marine One / YachtBrokerage`
- Reachable but not ready without rendering: `Croatian Yachting`
- Hard blocked, stop immediately: `Boat24` and `iNautia`

This is the info ==gemini ==provided: 

- **Volume & Scope:** Very High. A massive, globally dominant boat listing directory.
    
- **Defense Profile:** Extreme. Protected by Cloudflare Bot Management with aggressive TLS fingerprints, behavior matching, and IP threat-score rules.
    
- **Extraction Strategy:** **Stealth Browser Engine (`nodriver`) with Dual-Node Architecture**.
    
- **Toolkit Requirements:**
    
    - `nodriver` running on the high-performance Home PC (Worker Node) to preserve ThinkPad performance.
        
    - Sticky Residential Proxies (DataImpulse) bound to European locations.
        
    - Network Asset Blocker (CDP Interception) to prevent loading media assets, keeping data consumption low.
        
- **Obsidian Note:** Re-activated for production. Codex got blocked because it didn't use proxies or custom TLS profiles. We will conquer this by utilizing our dual-node architecture: offload the heavy browser tasks to the Home PC (Worker Node) to preserve ThinkPad performance, and utilize sticky residential IPs to maintain persistent session states during the pagination loops.
    

Probable Errors & How to Fix Them

> **Error 1: Turnstile Challenge Interception**
> 
> - **Symptom**: The browser navigates to the listing page but gets stuck in an infinite Cloudflare redirect loop or triggers an interactive Turnstile captcha.
>     
> - **Fix**: Ensure `nodriver` is running with human-like behavioral emulation. Inject tiny, random mouse movements and smooth scroll offsets onto the page surface before clicking any listing items. If the proxy IP gets flagged, automatically cycle to a new residential IP in the same geographic sub-net to clear the threat score.
>     

> **Error 2: Bandwidth Exhaustion (Data Cap Drainage)**
> 
> - **Symptom**: Scraping high-resolution listing pages quickly drains your 5GB DataImpulse plan within hours because of heavy images, media files, and interactive scripts.
>     
> - **Fix**: Implement our **CDP Network Asset Blocker**. Force Chrome to intercept requests at the browser engine level and immediately drop any requests matching image formats, video streams, or non-critical font stylesheets. We only want the raw, un-styled HTML markup.
>
#### [Njuskalo Motorni Brodovi](https://www.njuskalo.hr/motorni-brodovi)

Here i only have the geminis info, codex didn't try to scrape njuskalo. Another important thing this evaluation of njuskalo was for njuskalo nautika not njuskalo motorni brodovi so maybe there are some differences between this evaluation and what i actually need but i think its not that important.

- **Volume & Scope**: Very High (~5,000+ active localized boats). This is Croatia's largest domestic classifieds platform.
    
- **Defense Profile**: Extreme. Heavy Cloudflare setup, aggressive local IP-based geo-blocking (non-Croatian traffic gets challenged instantly), and complex mobile-first API security.
    
- **Extraction Strategy**: Mobile App API Emulation or Playwright with Croatian Residential Proxies.
    
- **Toolkit Requirements**:
    
    - High-quality **Croatian (HR) Residential Proxies** (Absolute Requirement).
        
    - API Authorization header extractor (Bearer token scraper).
        
    - SQLite or PostgreSQL database to track incremental changes.
        
- **Obsidian Note**: The ultimate treasure trove of Croatian local listings, but incredibly hard to scrape raw. The best way to extract this is to emulate their native mobile app endpoints, which bypasses the heavy web obfuscation. You'll need a script to periodically capture and refresh the API authorization tokens.
    

Probable Errors & How to Fix Them

> **Error 1: Severe Geo-Blocking (Persistent 403s on Non-HR IPs)**
> 
> - **Symptom**: Even high-quality global proxies fail if they don't route through Croatia.
>     
> - **Fix**: Pin your proxy provider to strict `HR` geolocation. Njuškalo treats foreign traffic with extreme suspicion and will throw Cloudflare challenges constantly to non-local IPs.
>     

> **Error 2: Expired Bearer Tokens on Mobile API**
> 
> - **Symptom**: The mobile API requests will suddenly return `401 Unauthorized` or `403` once the session token expires.
>     
> - **Fix**: Build a helper script (`bearer_token_finder.py`) that boots up a headless browser, logs into a dummy Njuškalo account (or browses anonymously), intercepts the outgoing request headers to grab the fresh `Authorization: Bearer <token>`, and saves it to a local config file for your main scraper to use.
>     

> **Error 3: Hidden Phone Numbers**
> 
> - **Symptom**: Phone numbers are dynamic assets and are not present in the initial HTML structure.
>     
> - **Fix**: Query the dynamic phone API endpoint (`/api/v1/ads/{ad_id}/phone`) using the extracted listing ID, ensuring you pass the active Bearer token.
>