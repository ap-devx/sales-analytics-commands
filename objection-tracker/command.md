# Objection Tracker for Read AI Sales Meetings
---
description: >
  Advanced objection analysis tool that extracts, categorizes, and analyzes customer objections 
  from Read AI sales meeting transcripts in Google Drive. Identifies patterns, measures resolution 
  rates, compares rep handling strategies, and generates comprehensive playbooks with training 
  recommendations for improving objection handling across sales teams.
allowed_tools:
  - mcp__zapier__google_drive_find_a_file
  - mcp__zapier__google_docs_find_a_document
  - mcp__google-docs-mcp__readGoogleDoc
  - mcp__google-docs-mcp__listGoogleDocs
  - mcp__google-docs-mcp__searchGoogleDocs
  - mcp__filesystem__write_file
  - mcp__filesystem__create_directory
  - mcp__filesystem__read_file
---

## Arguments

```
/objection-tracker 
meeting_identifier="<meeting name or ID>"
[drive_folder="<Google Drive folder path>"]
[date_range="<YYYY-MM-DD to YYYY-MM-DD>"]
[rep_names="<comma-separated list of sales reps>"]
[analysis_mode=<single|comparative|historical>]
[objection_focus="<specific objection types to focus on>"]
[output_format=<markdown|json|html>]
[generate_playbook=<yes|no>]
[resolution_threshold=<percentage>]
```
*Defaults → `drive_folder="AI Labs/Meeting Notes (Read AI)"  analysis_mode=single  output_format=markdown  generate_playbook=yes  resolution_threshold=70`*

### Examples

#### Single Meeting Analysis
```
/objection-tracker 
meeting_identifier="Q4 Sales Call - Enterprise Client"
generate_playbook=yes
```

#### Comparative Rep Analysis
```
/objection-tracker 
date_range="2025-01-01 to 2025-08-31"
rep_names="John Smith,Jane Doe,Mike Johnson"
analysis_mode=comparative
```

#### Historical Pattern Analysis
```
/objection-tracker 
drive_folder="Sales/Meeting Transcripts"
analysis_mode=historical
objection_focus="pricing,competition"
```

---

## Context – What the AI Should Do

### 1. **Meeting Discovery and Access**
   * Use `mcp__zapier__google_drive_find_a_file` to locate Read AI meeting files
   * Search in specified folder or default "AI Labs/Meeting Notes (Read AI)"
   * Handle multiple file formats: Google Docs, PDFs, text files
   * If date range specified, filter meetings within that period
   * For rep-specific analysis, identify meetings by rep names in titles/content

### 2. **Content Extraction and Preprocessing**
   * Use `mcp__google-docs-mcp__readGoogleDoc` to extract meeting transcripts
   * Parse meeting metadata: date, participants, duration, outcome
   * Identify speaker segments (customer vs sales rep)
   * Extract context around objections (preceding and following statements)
   * Handle various Read AI transcript formats and structures

### 3. **Objection Detection and Extraction**
   * **Keywords and Phrases Detection**:
     - Pricing: "too expensive", "budget", "cost", "price point", "ROI concerns"
     - Competition: "competitor X", "alternative solution", "already using", "switching costs"
     - Trust: "not sure", "concerns about", "worried", "risky", "unproven"
     - Functionality: "doesn't do", "missing feature", "need capability", "requirement"
     - Timeline: "not ready", "bad timing", "next quarter", "postpone"
     - Authority: "need approval", "decision maker", "committee review"
   * **Pattern Recognition**:
     - Question patterns indicating objections
     - Hesitation markers and concern expressions
     - Comparative statements mentioning alternatives
   * **Context Analysis**:
     - Capture 2-3 sentences before and after objection
     - Note emotional tone and intensity
     - Identify if objection was raised multiple times

### 4. **Objection Categorization**
   * **Primary Categories**:
     - **Budget/Pricing**: Cost concerns, ROI questions, budget constraints
     - **Trust/Credibility**: Company reliability, product maturity, references
     - **Functionality/Features**: Missing capabilities, technical limitations
     - **Competition**: Competitor comparisons, existing solutions
     - **Timeline/Urgency**: Timing issues, implementation concerns
     - **Authority/Process**: Decision-making process, approvals needed
   * **Severity Scoring** (1-5):
     - 1: Minor concern, easily addressed
     - 2: Moderate concern, requires explanation
     - 3: Significant objection, needs detailed response
     - 4: Major blocker, requires strategy change
     - 5: Deal-breaker, critical resolution needed
   * **Resolution Status**:
     - Fully Resolved: Clear acceptance after response
     - Partially Resolved: Some concerns remain
     - Unresolved: Objection persists
     - Escalated: Referred to management/specialist

### 5. **Response Analysis**
   * **Rep Response Extraction**:
     - Capture complete response to each objection
     - Note response strategy (acknowledge, reframe, proof, etc.)
     - Measure response time and length
   * **Response Effectiveness Metrics**:
     - Customer sentiment shift after response
     - Follow-up questions or acceptance signals
     - Time to next objection or positive signal
   * **Response Patterns**:
     - Successful techniques by objection type
     - Common mistakes or missed opportunities
     - Best practices from top performers

### 6. **Resolution Rate Calculation**
   * **By Objection Type**:
     - Calculate % resolved for each category
     - Compare against threshold (default 70%)
     - Identify problematic objection types
   * **By Sales Rep**:
     - Individual resolution rates
     - Strengths and weaknesses per rep
     - Comparative performance metrics
   * **By Response Strategy**:
     - Which approaches work best
     - Context-dependent effectiveness
     - Optimal response sequences

### 7. **Comparative Analysis** (when multiple reps/meetings)
   * **Rep Performance Comparison**:
     - Resolution rates by rep and objection type
     - Response time and quality metrics
     - Unique strategies and approaches
   * **Pattern Identification**:
     - Common objections across meetings
     - Seasonal or cyclical patterns
     - Product/service specific concerns
   * **Best Practice Extraction**:
     - Top performer techniques
     - Successful response templates
     - Winning conversation flows

### 8. **Follow-up Analysis**
   * **Post-Objection Tracking**:
     - Customer engagement after objection handling
     - Meeting outcome correlation with resolution
     - Deal progression indicators
   * **Multi-Touch Analysis**:
     - Objections recurring in follow-up meetings
     - Evolution of concerns over time
     - Long-term resolution tracking

### 9. **Playbook Generation**
   * **Structure**:
     ```markdown
     # Objection Handling Playbook
     
     ## Executive Summary
     - Key findings and metrics
     - Critical objections requiring attention
     - Top recommendations
     
     ## Objection Catalog
     ### [Category Name]
     #### Objection: [Specific objection]
     - **Frequency**: X occurrences
     - **Resolution Rate**: X%
     - **Severity**: [1-5]
     
     ##### Best Response Template
     [Proven response that works]
     
     ##### Supporting Evidence
     - Case studies
     - ROI data
     - Customer testimonials
     
     ##### Avoid These Mistakes
     - Common pitfalls
     - Ineffective responses
     
     ## Rep-Specific Recommendations
     ### [Rep Name]
     - Strengths: [What they handle well]
     - Development Areas: [Where to improve]
     - Specific Training Needs: [Targeted coaching]
     
     ## Training Modules
     1. [Module Name]: Addressing [Objection Type]
        - Learning objectives
        - Role-play scenarios
        - Success metrics
     ```

### 10. **Training Recommendations**
   * **Individual Coaching Plans**:
     - Rep-specific improvement areas
     - Customized practice scenarios
     - Mentoring assignments
   * **Team Training Priorities**:
     - Common weaknesses across team
     - New objection handling strategies
     - Competitive positioning updates
   * **Content Development**:
     - Battle cards for common objections
     - Response scripts and templates
     - Case study library
   * **Practice Scenarios**:
     - Role-play exercises
     - Objection handling drills
     - Recorded response examples

### 11. **Report Generation**
   * **Markdown Report Structure**:
     - Executive summary with key metrics
     - Detailed objection analysis
     - Rep performance comparison
     - Playbook and recommendations
     - Appendices with raw data
   * **Visualizations** (as markdown tables/charts):
     - Objection frequency distribution
     - Resolution rate trends
     - Rep performance matrix
     - Response effectiveness heatmap
   * **Export Options**:
     - Save to local filesystem
     - Create shareable Google Doc
     - Generate JSON for integration

### 12. **Quality Assurance**
   * Validate objection detection accuracy
   * Cross-reference with meeting outcomes
   * Ensure statistical significance for patterns
   * Flag anomalies or outliers for review

### 13. **Output Files**
   * Create directory: `objection-analysis-[date]`
   * Main report: `objection-analysis-report.md`
   * Playbook: `objection-handling-playbook.md`
   * Training plan: `training-recommendations.md`
   * Raw data: `objection-data.json` (if requested)

> The command should provide actionable insights that immediately improve sales team objection handling, with specific templates and strategies proven to work in actual customer conversations.