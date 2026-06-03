# Analyst Notes

## Investigative Approach Evolution

### Initial Mistakes

**Protocol Assumption Errors:**
- Initially searched: `tcp.port==3942`
- Result: No matches returned
- Root cause: Assumed SSDP used TCP without verifying protocol
- Correction: Pivoted to `udp.port==3942` and identified SSDP traffic

**Question-Driven vs. Investigation-Driven:**
- Started by answering exercise questions sequentially
- Result: Fragmented analysis without coherent narrative
- Correction: Built attack timeline before answering specific questions

**Screenshot Overload:**
- Captured screenshots of every Wireshark interaction
- Result: Evidence folder cluttered with tool demonstrations, not attack proof
- Correction: Limited screenshots to one per attack stage, focused on provable attacker actions

---

## Course Corrections

### Timeline-First Approach

**What Changed:**
- Built temporal sequence of events before detailed protocol analysis
- Identified attacker vs. victim actions for each event
- Used timeline as investigative framework for evidence correlation

**Impact:**
- Clearer attack narrative
- Better understanding of attack flow
- Reduced investigative dead ends

### Evidence Quality Focus

**What Changed:**
- Shifted from "show tool usage" to "prove attacker behavior"
- Eliminated screenshots that demonstrated Wireshark features rather than attack evidence
- Added explicit "Assessment" sections to each evidence subsection

**Impact:**
- Report reads as forensic investigation, not tool tutorial
- Evidence directly supports investigative conclusions
- Clearer differentiation between observation and analysis

---

## What Worked

### DNS Filter Refinement

**Filter Used:** `dns.flags.response==1`

**Reasoning:**
- Generic `dns` filter returned both requests and responses
- Responses contain resolution data needed for investigation
- Eliminated noise from redundant request traffic

**Result:**
- Focused analysis on successful DNS resolutions
- Identified victim hostname (IEWIN7)
- Confirmed network communication patterns

### Protocol Hierarchy Analysis

**Approach:**
- Used Wireshark's protocol hierarchy statistics before applying filters
- Identified HTTP and FTP as primary investigation targets
- Noted absence of encrypted protocols (HTTPS, SFTP)

**Result:**
- Efficient investigation focus
- Early identification of security weaknesses
- Avoided time spent on irrelevant protocols

### Evidence Labeling Discipline

**System Implemented:**
- Screenshot naming: `##_descriptive_name.png`
- Markdown references: `![Descriptive Title](screenshots/##_descriptive_name.png)`
- Evidence-to-finding mapping in report structure

**Result:**
- Easy evidence cross-referencing
- Clear visual-to-text correlation
- Professional report presentation

---

## Key Lessons for Next Investigation

### 1. Timeline First, Details Second

**Principle:**
Build attack timeline before diving into protocol-specific analysis.

**Application:**
- Extract timestamps from initial packet overview
- Identify key events (authentication, file access, data exfiltration)
- Use timeline to guide detailed investigation

**Why It Matters:**
Timeline provides investigative structure and prevents getting lost in protocol details.

---

### 2. Verify Protocol Assumptions Early

**Principle:**
Don't assume protocol characteristics—verify in packet capture.

**Application:**
- Check protocol hierarchy statistics
- Confirm transport layer (TCP/UDP) before applying filters
- Review protocol documentation if unfamiliar

**Why It Matters:**
Wrong protocol assumptions waste time and may miss critical evidence.

---

### 3. Attribution Clarity in Every Finding

**Principle:**
Every piece of evidence should identify who did what.

**Application:**
- Label timeline events with ATTACKER/VICTIM
- Use active voice in evidence descriptions
- Clearly state agent of action (e.g., "Attacker captured credentials" not "Credentials were exposed")

**Why It Matters:**
Attribution turns packet analysis into attack reconstruction.

---

### 4. Evidence Minimalism

**Principle:**
One screenshot per finding, not per tool interaction.

**Application:**
- Capture evidence that proves attacker action
- Eliminate screenshots showing Wireshark UI without attack content
- Combine related evidence in single screenshot when possible

**Why It Matters:**
Reduces cognitive load for report readers, maintains professional presentation.

---

### 5. Narrative Over Q&A

**Principle:**
Investigation tells a story, not answers a quiz.

**Application:**
- Frame report as attack reconstruction
- Use exercise questions as validation checkpoints, not report structure
- Lead with "what happened," support with evidence, conclude with impact

**Why It Matters:**
Hiring managers assess investigative thinking, not ability to answer specific questions.

---

## Tools & Techniques Refined

### Wireshark Display Filters

**Effective Filters Discovered:**
```

dns.flags.response==1 # DNS responses only 
tcp.stream eq X # Follow specific TCP stream 
ftp.request.command # FTP commands issued 
http.request.method # HTTP methods (GET, POST, etc.) 
ip.addr==X && ip.addr==Y # Traffic between two specific hosts


```
**Filter Strategy:**
- Start broad (protocol name)
- Refine based on noise level
- Use protocol-specific fields for precision

### Stream Following

**When to Use:**
- HTTP: Credential exposure, form data, session tokens
- FTP: File transfer commands, authentication
- TCP: Raw data inspection when protocol-specific dissectors fail

**Best Practice:**
- Follow streams after identifying relevant packets with filters
- Don't follow every stream—use filters to target specific evidence

---

## Reflection on Exercise Design

### What the Exercise Taught

**Technical Skills:**
- Protocol analysis (HTTP, FTP, DNS, SSDP)
- Traffic pattern recognition
- Credential exposure identification
- Attack timeline reconstruction

**Investigative Skills:**
- Question formulation based on evidence
- Evidence correlation across multiple protocols
- Distinguishing attacker from victim actions
- Impact assessment from technical findings

### What the Exercise Didn't Cover

**Gaps for Future Learning:**
- Encrypted traffic analysis (TLS/SSL)
- Malware network behavior patterns
- Large-scale PCAP analysis (this was ~2MB)
- Intrusion detection system (IDS) integration

**Implications:**
Next investigations should target:
- CyberDefenders challenges with encrypted traffic
- Malware traffic analysis (malware-traffic-analysis.net)
- Larger packet captures requiring automated filtering

---

## Next Investigation Preparation

### Platform: CyberDefenders

**Target Challenge:**
Beginner network forensics challenge (TBD based on platform availability)

**Preparation Needed:**
- Review encrypted traffic analysis techniques
- Practice with larger PCAP files
- Explore automated analysis tools (tshark, NetworkMiner)

### Skill Development Goals

**Technical:**
- TLS/SSL decryption techniques
- Malware C2 traffic patterns
- Network baseline establishment

**Investigative:**
- Hypothesis-driven investigation
- Evidence prioritization under time pressure
- Multi-source evidence correlation

---

## Personal Notes

### What I Learned About My Learning Process

**Observation:**
I tend to start tasks without finishing them when I encounter the first challenge.

**Example in This Investigation:**
- Hit dead end with `tcp.port==3942`
- Immediate impulse: try different approach without understanding why first failed
- Correction: Stopped, verified protocol assumption, then proceeded

**Pattern Recognition:**
This same pattern appeared in:
- Course accumulation instead of finishing one
- Starting new tools before mastering current ones
- Exercise question-hopping instead of completing full investigation

**Mitigation Strategy:**
- Build timeline/plan before starting analysis
- Commit to completing one investigation before starting next
- Document blockers instead of immediately pivoting

### Confidence vs. Competence

**Before This Investigation:**
- Confidence: Low (unsure if I could complete a "real" investigation)
- Competence: Moderate (had tool knowledge, lacked investigative structure)

**After This Investigation:**
- Confidence: Moderate (proven I can reconstruct attack from packets)
- Competence: Moderate (still refining evidence selection, report writing)

**Gap to Close:**
The gap between confidence and competence narrows through output, not more courses.

---

## Investigation Metrics

**Time Spent:**
- Initial analysis: ~3 hours
- Report drafting: ~2 hours
- Revision after feedback: ~1.5 hours
- Total: ~6.5 hours

**Evidence Collected:**
- Screenshots: 6 (reduced from initial 12)
- Wireshark filters used: 8
- Protocols analyzed: 5 (HTTP, FTP, DNS, SSDP, ICMP)

**Report Iterations:**
- Version 1: Question-driven, 15+ screenshots
- Version 2: Timeline-driven, 8 screenshots
- Version 3 (final): Attack narrative, 6 screenshots

**Key Insight:**
More iterations = better output. First draft is never publish-ready.

---

## Acknowledgments

**Resources Used:**
- Security Blue Team Network Analysis course
- Wireshark documentation
- RFC 959 (FTP specification)
- Personal debugging and trial-and-error

**Feedback Incorporated:**
- Claude AI: Report structure, evidence presentation, investigative methodology
- Self-review: Timeline clarity, screenshot reduction, attribution precision

---

## Archive Note

This investigation represents a learning milestone: **first completed DFIR portfolio piece**.

Not the best investigation I'll ever do, but the first one I finished and published.

**Date Completed:** May 10, 2026  
**Status:** Published (GitHub, LinkedIn, Medium)  
**Next Investigation:** CyberDefenders network forensics challenge
