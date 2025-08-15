# Talk-to-Listen Ratio Analyzer

Advanced conversation dynamics analyzer for Read AI meeting transcripts that measures sales effectiveness through talk-to-listen ratios, question quality, and outcome correlation.

## Overview

The Talk-to-Listen Ratio Analyzer transforms Read AI meeting transcripts into actionable insights about conversation dynamics. It calculates precise talk-to-listen ratios, evaluates question effectiveness, segments analysis by call type, and correlates patterns with deal outcomes to identify coaching opportunities and extract top performer techniques.

## Key Features

### Talk-to-Listen Ratio Analysis

- **Precise Calculation**: Measures exact percentage of sales rep vs customer speaking time
- **Word Count Analysis**: Tracks total words spoken by each party
- **Segment Analysis**: Analyzes average segment length and conversation flow
- **Time Estimation**: Calculates estimated speaking duration based on word count

### Question Effectiveness Assessment

- **Question Classification**: Identifies open-ended vs closed questions
- **Response Measurement**: Tracks customer response length for each question type
- **Effectiveness Scoring**: Calculates question quality score (0-100)
- **Pattern Recognition**: Identifies successful questioning techniques

### Call Type Segmentation

- **Optimal Ratios by Type**:
- Discovery: 30/70 (rep/customer)
- Demo: 50/50
- Negotiation: 40/60
- Closing: 35/65
- **Deviation Analysis**: Measures alignment with optimal ratios
- **Contextual Recommendations**: Provides call-type specific improvement suggestions

### Deal Outcome Correlation

- **Win Rate Analysis**: Correlates talk ratios with successful deals
- **Statistical Insights**: Identifies optimal ranges for winning deals
- **Pattern Discovery**: Finds conversation patterns that predict success
- **Predictive Metrics**: Develops benchmarks for future calls

### Coaching Intelligence

- **Priority Identification**: Flags reps needing immediate coaching
- **Strength Recognition**: Highlights top performers and their techniques
- **Personalized Plans**: Generates individual improvement action plans
- **Team Analytics**: Provides team-wide performance insights

## Installation

### Prerequisites

1. Claude Code CLI with MCP support
1. Required MCP servers:
- `google-docs-mcp` - For accessing Google Drive documents
- `zapier` - For Google Drive integration
- `filesystem` - For local report generation

### Setup

```bash
# Clone the command repository

git clone https://github.com/yourusername/commands.git

cd commands

# Ensure MCP servers are configured

claude-code mcp install google-docs-mcp

claude-code mcp install zapier

```

## Usage

### Basic Analysis

```bash
# Analyze a single meeting

/talk-ratio-analyzer meeting_identifier="Q4 Discovery Call - Enterprise"

# Quick analysis without coaching report

/talk-ratio-analyzer meeting_identifier="Product Demo" coaching_report=no

```

### Comparative Analysis

```bash
# Compare multiple reps over time

/talk-ratio-analyzer \

date_range="2025-01-01 to 2025-08-31" \

rep_names="John Smith,Jane Doe,Mike Wilson" \

analysis_mode=comparative

# Focus on specific call types

/talk-ratio-analyzer \

call_types="discovery,demo" \

rep_names="Sarah Johnson" \

date_range="2025-07-01 to 2025-07-31"

```

### Historical Analysis

```bash
# Full historical analysis with correlations

/talk-ratio-analyzer \

analysis_mode=historical \

outcome_correlation=yes \

coaching_report=yes \

output_format=markdown

```

### Advanced Options

```bash
# JSON export for integration

/talk-ratio-analyzer \

meeting_identifier="Strategic Account Review" \

question_analysis=yes \

output_format=json

# Batch processing with custom folder

/talk-ratio-analyzer \

drive_folder="Sales/Transcripts/Q4" \

date_range="2025-10-01 to 2025-12-31" \

coaching_report=yes

```

## Output Structure

### Markdown Report

```
talk-ratio-analysis-[timestamp]/

├── report.md                 # Comprehensive analysis report
├── coaching-recommendations.md # Detailed coaching plans
├── top-performer-playbook.md  # Best practices extraction
└── data.json                  # Optional raw data export

```

### Report Sections

1. **Executive Summary**: Key findings and metrics
1. **Talk Ratio Analysis**: Detailed breakdown by rep and meeting
1. **Question Effectiveness**: Analysis of questioning techniques
1. **Call Type Alignment**: Performance against optimal ratios
1. **Deal Correlation**: Statistical analysis of outcomes
1. **Coaching Priorities**: Ranked list of improvement areas
1. **Top Techniques**: Extracted best practices from top performers
1. **Individual Reports**: Personalized analysis for each rep
1. **Action Items**: Specific next steps and recommendations

## Metrics Explained

### Talk-to-Listen Ratio

- **Calculation**: (Rep Words / Total Words) × 100
- **Optimal Ranges**: Varies by call type (see segmentation above)
- **Red Flags**: >70% rep talking in discovery, <30% in demos

### Question Effectiveness Score

- **Components**:
- Open-ended question ratio (50% weight)
- Average response length (50% weight)
- **Scoring**:
- 80-100: Excellent consultative approach
- 60-79: Good questioning skills
- 40-59: Needs improvement
- <40: Requires immediate coaching

### Alignment Score

- **Calculation**: 100 - |Actual Ratio - Optimal Ratio|
- **Interpretation**:
- 90-100: Excellent alignment
- 75-89: Good alignment
- 60-74: Moderate alignment
- <60: Poor alignment

## Best Practices

### For Discovery Calls

- Aim for 30% talk time
- Use 80% open-ended questions
- Average 3+ follow-up questions per topic
- Listen for 45+ seconds before responding

### For Demo Calls

- Maintain 50/50 balance
- Check for understanding every 2-3 minutes
- Ask engagement questions throughout
- Pause for questions after each feature

### For Negotiations

- Target 40% talk time
- Focus on clarifying questions
- Summarize agreements frequently
- Let customer express concerns fully

## Troubleshooting

### Common Issues

**Issue**: Cannot access Google Drive files

- **Solution**: Ensure Zapier MCP is properly authenticated with Google Drive permissions

**Issue**: Rep names not recognized

- **Solution**: Provide exact names as they appear in transcripts, comma-separated

**Issue**: Call type not detected

- **Solution**: Manually specify call_types parameter or ensure transcript titles include type

**Issue**: Slow processing for large date ranges

- **Solution**: Process in monthly batches or increase timeout settings

## Integration

### CRM Integration

Export JSON data for integration with:

- Salesforce
- HubSpot
- Pipedrive
- Custom CRM systems

### Coaching Platforms

Compatible with:

- Gong.io
- Chorus.ai
- ExecVision
- Custom LMS systems

### Reporting Tools

- Tableau
- Power BI
- Google Data Studio
- Custom dashboards

## Performance Benchmarks

- **Single Meeting**: 30-60 seconds
- **10 Meetings**: 2-3 minutes
- **50 Meetings**: 5-8 minutes
- **100+ Meetings**: 10-15 minutes

## Changelog

### Version 1.0.0 (2025-08-13)

- Initial release
- Full talk-to-listen ratio analysis
- Question effectiveness scoring
- Call type segmentation
- Deal outcome correlation
- Coaching recommendations
- Top performer technique extraction

## License

MIT License. All rights reserved.

---

*Copyright © 2025 AlphaPoint.com*
