The 1000-Company Proposal: Building an ICP-Qualified List in 30 Days
Submitted by: Tumili M
Date: June 3, 2026
________________________________________
Executive Summary
To build 1000 genuinely ICP-qualified Federer companies in 30 days, I propose a phased funnel approach combining automated data sourcing, AI-assisted qualification, and human-in-the-loop quality control. This plan respects the observed 25–30% final yield from DeepThought's research and ensures every company is properly verified.
The Numbers:
Stage	Companies	Yield
Initial universe (Week 1)	5,000	—
After E1/E2 gates (Week 2)	~3,000	60%
After Federer scoring (Week 3)	~1,200	40% of remaining
Final verified list (Week 4)	1,000	83% of scored
Final yield from start: 20% (5,000 → 1,000), which is realistic given the multi-stage filtering.
________________________________________
Week-by-Week Execution Plan

Week 1 Deliverable:
•	5,000-row raw database with company names, websites, sectors, cities, and source tags
•	Deduplication report showing how many duplicates were removed
________________________________________
WEEK 2: Automated Gate Filtering (E1 + E2)
Goal: Apply eligibility gates automatically, reduce to ~3,000 companies
E1 Gate: Producer Check
What we're checking: Does this company produce something in-house (has plant/facility) vs. being a trader/distributor/service provider?
Automation approach:
python
# Pseudo-code for E1 Producer Check
def check_producer(website_text):
    producer_signals = [
        "plant", "manufacturing facility", "production line", "in-house", 
        "capacity", "tonnes/year", "factory", "production unit", 
        "manufacturing unit", "production capacity", "our plant"
    ]
    trader_signals = [
        "distributor", "trader", "importer", "reseller", "dealer", 
        "wholesaler", "retailer", "supplier", "intermediary"
    ]
    
    producer_score = sum(1 for kw in producer_signals if kw in website_text.lower())
    trader_score = sum(1 for kw in trader_signals if kw in website_text.lower())
    
    if producer_score >= 3 and trader_score == 0:
        return "PASS", f"Found {producer_score} producer signals"
    elif trader_score >= 2:
        return "FAIL", f"Trader signals detected ({trader_score})"
    else:
        return "MANUAL_REVIEW", f"Inconclusive: {producer_score} producer, {trader_score} trader"

AI-assisted enhancement:
•	Use Claude API to analyze website "About Us" and "Facilities" pages
•	Prompt:
text
Analyze this company website text. Is this a producer (owns plant/facility, makes products) 
or a trader/service provider (distributor, CRO, testing lab)?

Output format:
- PASS/FAIL/MANUAL_REVIEW
- 1-line evidence quote from the text
E2 Gate: Accessibility Check
What we're checking: Does this company have operational presence in India (HQ or primary operations)?
Automation approach:
1.	Filter by location data from MCA database (registered office address)
2.	Google Maps API to verify physical facility exists at listed address
3.	Website location check: Look for "Headquartered in [Indian city]" or "Manufacturing facility in [Indian city]"
4.	Discard: Foreign entities with no India operations, purely remote companies
Quality Control for Week 2:
QC Method	Sample Size	Target Error Rate
Manual spot-check of AI decisions	250 companies (5%)	<10% false positives
Cross-validate with secondary source	100 companies	Confirm E1/E2 status
Review all MANUAL_REVIEW cases	~300 companies	Manual decision
Week 2 Deliverable:
•	3,000 companies passing E1 + E2 gates
•	Gate filtering report showing:
o	How many failed E1 (traders/service companies)
o	How many failed E2 (non-India)
o	Evidence tags for each decision
•	Failed companies list with reasons (for DeepThought's learnings)
________________________________________
WEEK 3: Federer Scoring at Scale
Goal: Score remaining 3,000 companies on C3–C8, identify ~1,200 A/B-band companies
The 6 Criteria (C3–C8) Scoring Pipeline:
For each company, run parallel AI analysis across 6 data sources:
Criterion	Weight	Data Source	AI Prompt Strategy
C3: Differentiated	20	Website, patents, regulatory filings	"Does this company have patents, USFDA/EU-GMP, DSIR recognition, specialized equipment, or proprietary products? Score 0 (Weak), 8 (Moderate), or 20 (Strong). Provide evidence quote."
C4: Decision-Maker Quality	15	LinkedIn, About Us, MCA filings	"Find founder/MD background. Is PhD/IIT/NIT/scientist/engineer with technical publications OR operator with ERP/SAP experience? Score 0/7/15 with evidence."
C5: Growing Sector	15	Industry reports, PLI eligibility	"Is this sector PLI-eligible, China+1 beneficiary, Make-in-India priority, or export tailwind? Score 0/7/15 with sector name."
C6: Growth Signals	15	LinkedIn jobs, news, website	"Count growth signals: (1) 5+ open roles on LinkedIn, (2) new plant/expansion announced, (3) new certification in 2 years, (4) website copyright 2024–2025, (5) revenue growth in filings. Score 0/7/15 with count."
C7: Systems Maturity	20	Job posts, LinkedIn, website	"Evidence of SAP/ERP implementation, MIS dashboards, IT head on LinkedIn, structured costing models? Score 0/8/20 with evidence."
C8: Leadership Succession	15	MCA board filings, LinkedIn	"Gen-2 formally on board with operational role? Professional managers hired (COO/CFO/CTO from outside)? Independent directors? Score 0/7/15 with names."
Scoring Workflow:
text
Batch Processing (3 batches of 1,000 companies each):

Batch 1 
  ↓
Run AI scoring on all 6 criteria
  ↓
Manual calibration sample: 50 companies (5%)
  ↓
Compare AI scores with human scores → adjust prompts
  ↓
Batch 2 

  ↓
Run optimized AI prompts on 1,000 companies
  ↓
Manual calibration sample: 50 companies
  ↓
Final prompt tweaks
  ↓
Batch 3

  ↓
Run final prompts on remaining 1,000 companies
Scoring Thresholds & Classification:
Score Range	Band	Action
80–100	A	Auto-include (strong Federer)
60–79	B	Auto-include (probable Federer)
40–59	C	Flag for manual review
<40	D	Auto-exclude, log reason
Manual Review Process:
Company Band	Manual Review Required?	Review Depth
A-band	Random audit only	50 companies (5%)
B-band	Random audit only	50 companies (5%)
C-band	Full manual review	All ~300 companies
D-band	None	Skip (already excluded)
Week 3 Deliverable:
•	~1,200 companies with full Federer scores (C3–C8)
•	Scoring report showing:
o	Distribution of scores (how many A/B/C/D band)
o	Average score per criterion
o	Common failure patterns
•	A/B-band list with evidence tags for each criterion
•	C-band companies with manual review decisions
________________________________________
WEEK 4: Validation, Cleaning & Final List
Goal: Verify 1,000 companies, remove stale/inaccurate data, deliver final verified list
Validation Checklist:
1.	Freshness Check 
o	Visit each company website → confirm it's active (not 404)
o	Check copyright year → should be 2024 or 2025
o	Check "News/Press" section → should have updates in last 12 months
o	Check LinkedIn page → activity in last 6 months
o	Exclude: Stale websites,inactive companies
2.	Revenue Verification
o	Cross-check with Tofler/ZaubaCorp for latest financial filings
o	Confirm revenue band: ₹50Cr–₹500Cr
o	Exclude: Companies >₹500Cr (too large), <₹50Cr (too small, if data available)
3.	Ownership Check 
o	Scan MCA filings for promoter changes
o	Check for: PE/VC majority ownership, acquisition by larger group, subsidiary of Tata/Reliance/etc.
o	Exclude: PE/VC-controlled, acquired companies, group subsidiaries
4.	Duplicate Removal 
o	Final deduplication pass
o	Merge partial records (same company with incomplete data)
5.	Manual Spot-Check 
o	Randomly verify 100 companies (10%) end-to-end
o	Deep-dive verify 20 A-band companies in detail
o	Confirm all evidence is accurate

	
	Week 4 Deliverable:
•	1,000-row verified CSV with all fields filled
•	Validation report showing:
o	How many companies excluded at each step
o	Reasons for exclusion (stale, too large, PE-owned, etc.)
o	Final A/B-band breakdown
•	Top 200 list with rich personalization hooks (for immediate outreach)
•	GitHub repo with all code, prompts, and methodology
•	Execute the phased plan as outlined above
•	Provide weekly progress updates with metrics
•	Deliver final 1,000-company list by Day 30


