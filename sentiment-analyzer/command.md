# Sentiment Analyzer for Read AI Meeting Reports
---
description: >
  Advanced sentiment analysis tool for Read AI meeting reports in Google Drive. Analyzes 
  customer sentiment, sales representative confidence, and identifies key moments of sentiment 
  shifts during discussions. Generates detailed coaching reports with actionable insights.
allowed_tools:
  - gdrive                 # Access to Google Drive for meeting reports
  - filesystem             # File operations for report generation
  - sequentialthinking     # Advanced analysis and reasoning
---

## Arguments

```
/sentiment-analyzer 
[report_name=<meeting-title|auto-select>]
[drive_folder="AI Labs/Meeting Notes (Read AI)"]
[analysis_depth=<comprehensive|standard|quick>]
[output_format=<markdown|json|both>]
[include_recommendations=<yes|no>]
```
*Defaults → `report_name=auto-select  drive_folder="AI Labs/Meeting Notes (Read AI)"  analysis_depth=comprehensive  output_format=markdown  include_recommendations=yes`*

### Examples

```
/sentiment-analyzer 
report_name="Q4 Sales Call - Acme Corp"
analysis_depth=comprehensive
```

```
/sentiment-analyzer 
drive_folder="AI Labs/Meeting Notes (Read AI)"
output_format=both
include_recommendations=yes
```

---

## Context – what the AI should do

1. **Initialize and Locate Meeting Report**
   * Connect to Google Drive using `gdrive` MCP server
   * Navigate to the specified folder (default: "AI Labs/Meeting Notes (Read AI)")
   * If `report_name` is not provided, list available reports and ask user to select
   * Search for the specific meeting report using `/gdrive__find_a_file`

2. **Extract Meeting Content**
   * Use `/gdrive__retrieve_file_or_folder_by_id` to get the full meeting report
   * Parse the Read AI meeting structure:
     - Meeting transcript
     - Meeting notes and summaries
     - Action items and key moments
     - Participant information
   * Extract temporal markers for conversation flow analysis

3. **Sequential Sentiment Analysis**
   * Use `/sequentialthinking__sequential_thinking` for deep analysis:
     ```
     Analyze the following meeting transcript for sentiment patterns:
     1. Customer Sentiment Evolution:
        - Initial sentiment baseline
        - Sentiment shifts during objections
        - Reactions to pricing discussions
        - Response to product demonstrations
        - Final sentiment state
     
     2. Sales Representative Analysis:
        - Confidence level throughout the call
        - Handling of objections
        - Tone during critical moments
        - Persuasion effectiveness
     
     3. Key Inflection Points:
        - Identify exact moments of sentiment change
        - Catalog triggers for positive/negative shifts
        - Note missed opportunities
     ```

4. **Sentiment Scoring Framework**
   * **Customer Sentiment Score (1-10)**:
     - 1-3: Negative (resistance, skepticism, disengagement)
     - 4-6: Neutral (cautious interest, information gathering)
     - 7-10: Positive (engaged, enthusiastic, ready to proceed)
   
   * **Sales Rep Confidence Score (1-10)**:
     - 1-3: Low confidence (hesitant, uncertain, defensive)
     - 4-6: Moderate confidence (steady, informative)
     - 7-10: High confidence (assertive, compelling, authoritative)

5. **Critical Moment Detection**
   * **Objection Handling Analysis**:
     - Identify all customer objections
     - Measure response time and quality
     - Score effectiveness of objection handling
   
   * **Pricing Discussion Analysis**:
     - Track sentiment before/during/after pricing reveal
     - Identify value justification effectiveness
     - Note any pricing pushback patterns
   
   * **Product Demo Analysis**:
     - Engagement level during demonstrations
     - Questions asked (quantity and quality)
     - Feature interest mapping

6. **Pattern Recognition and Cross-Reference**
   * Identify recurring sentiment patterns across:
     - Similar deal sizes
     - Industry verticals
     - Sales rep performance
   * Cross-reference with historical deal outcomes if available
   * Flag patterns that correlate with successful/failed deals

7. **Generate Comprehensive Report**
   * Create markdown report structure:
     ```markdown
     # Sentiment Analysis Report: [Meeting Title]
     **Date**: [Meeting Date]
     **Participants**: [List]
     **Duration**: [Time]
     
     ## Executive Summary
     - Overall Customer Sentiment: [Score]/10
     - Sales Rep Confidence: [Score]/10
     - Deal Probability: [High/Medium/Low]
     - Key Recommendation: [Primary action item]
     
     ## Sentiment Timeline
     [Visual representation of sentiment changes throughout the meeting]
     
     ## Critical Moments Analysis
     ### Objection Handling
     [Detailed breakdown of each objection and response]
     
     ### Pricing Discussion
     [Analysis of pricing conversation dynamics]
     
     ### Product Demonstration
     [Engagement and interest analysis]
     
     ## Sentiment Shift Analysis
     [Table of major sentiment changes with timestamps and triggers]
     
     ## Coaching Recommendations
     ### Tone Improvement
     - [Specific suggestions for tone adjustment]
     - [Example phrases that could be improved]
     
     ### Product Positioning Insights
     - [Features that resonated most]
     - [Areas needing better explanation]
     
     ### Pricing Strategy Feedback
     - [Effectiveness of value proposition]
     - [Suggestions for pricing presentation]
     
     ## Deal Risk Assessment
     - **Risk Factors**: [List]
     - **Opportunity Factors**: [List]
     - **Recommended Next Steps**: [Actionable items]
     
     ## Historical Pattern Comparison
     [How this meeting compares to similar successful/unsuccessful deals]
     ```

8. **File Operations and Delivery**
   * Use `/filesystem__write_file` to save the report
   * Name the file: `[Meeting_Title]_Sentiment_Analysis.md`
   * Create a companion JSON file if `output_format=both`
   * Store in organized directory structure

9. **Advanced Analytics (if comprehensive mode)**
   * **Linguistic Analysis**:
     - Word choice patterns indicating sentiment
     - Speech tempo and interruption patterns
     - Question-to-statement ratios
   
   * **Engagement Metrics**:
     - Speaking time distribution
     - Topic dwell time analysis
     - Follow-up question quality
   
   * **Predictive Insights**:
     - Deal closure probability based on patterns
     - Recommended intervention strategies
     - Optimal follow-up timing and approach

10. **Quality Assurance and Validation**
    * Validate sentiment scores against transcript evidence
    * Ensure all critical moments are captured
    * Cross-check recommendations for actionability
    * Verify report formatting and completeness

## Error Handling

* If meeting report not found: List available reports for selection
* If Google Drive access fails: Provide troubleshooting steps
* If analysis incomplete: Save partial results with clear indicators

## Success Metrics

* Accurate sentiment scoring with evidence
* Identification of all critical conversation moments
* Actionable coaching recommendations
* Clear correlation between sentiment and deal outcomes
* Time-stamped sentiment trajectory mapping

> This tool transforms meeting recordings into strategic insights, enabling sales teams to improve their approach, understand customer psychology, and increase deal closure rates through data-driven coaching.