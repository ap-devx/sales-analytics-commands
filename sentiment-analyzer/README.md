# Read AI Meeting Sentiment Analyzer

An advanced AI-powered tool for analyzing sentiment patterns in Read AI meeting reports stored in Google Drive. This command provides comprehensive insights into customer sentiment, sales representative performance, and critical conversation dynamics.

## Purpose

Transform your Read AI meeting recordings into actionable business intelligence by:

- Scoring customer sentiment and sales rep confidence (1-10 scale)
- Identifying key moments where sentiment shifts occur
- Analyzing effectiveness during objections, pricing discussions, and demos
- Generating coaching recommendations for improved outcomes
- Cross-referencing patterns with historical deal success rates

## Installation

```bash

claude mcp add --transport http sentiment-analyzer https://api.commands.com/mcp/[your-username]/sentiment-analyzer

```

## Prerequisites

1. **Google Drive Access**: Ensure you have access to the Google Drive containing Read AI meeting reports
1. **MCP Servers Required**:
- `gdrive` - For accessing Google Drive files
- `filesystem` - For saving analysis reports
- `sequentialthinking` - For advanced sentiment analysis

## Usage

### Basic Usage

```bash
/sentiment-analyzer

```

This will prompt you to select a meeting report from your default folder.

### Specify a Meeting Report

```bash
/sentiment-analyzer report_name="Q4 Sales Call - Acme Corp"

```

### Custom Folder Location

```bash
/sentiment-analyzer drive_folder="Sales Team/Meeting Reports" report_name="Product Demo - TechCorp"

```

### Quick Analysis Mode

```bash
/sentiment-analyzer report_name="Weekly Standup" analysis_depth=quick

```

### Export as JSON

```bash
/sentiment-analyzer report_name="Client Review" output_format=json

```

## What You'll Get

### Sentiment Scores

- **Customer Sentiment (1-10)**: Measures engagement and receptiveness
- **Sales Rep Confidence (1-10)**: Evaluates presenter effectiveness
- **Deal Probability**: High/Medium/Low assessment based on patterns

### Critical Moments Analysis

- **Objection Handling**: How well concerns were addressed
- **Pricing Discussions**: Reaction to cost and value proposition
- **Product Demonstrations**: Engagement level and interest indicators

### Coaching Recommendations

- **Tone Improvements**: Specific suggestions for better communication
- **Product Positioning**: Insights on features that resonate
- **Pricing Strategy**: Feedback on value presentation

### Timeline Visualization

Visual representation of sentiment changes throughout the meeting with:

- Timestamp markers for key moments
- Trigger identification for sentiment shifts
- Missed opportunity flags

## Output Formats

### Markdown Report (Default)

A comprehensive, formatted report with sections for:

- Executive Summary
- Sentiment Timeline
- Critical Moments Analysis
- Coaching Recommendations
- Deal Risk Assessment
- Historical Pattern Comparison

### JSON Export

Structured data output for:

- Integration with CRM systems
- Custom dashboard creation
- Programmatic analysis
- Data warehousing

## Configuration Options

| Parameter | Options | Default | Description |
| --- | --- | --- | --- |
| `report_name` | Meeting title or "auto-select" | auto-select | Specific report to analyze |
| `drive_folder` | Folder path | AI Labs/Meeting Notes (Read AI) | Google Drive folder location |
| `analysis_depth` | comprehensive, standard, quick | comprehensive | Level of analysis detail |
| `output_format` | markdown, json, both | markdown | Report format |
| `include_recommendations` | yes, no | yes | Include coaching suggestions |

## Analysis Depth Levels

### Comprehensive (Default)

- Full transcript analysis
- Linguistic pattern detection
- Historical comparison
- Predictive insights
- Detailed coaching recommendations

### Standard

- Core sentiment scoring
- Key moment identification
- Basic recommendations
- Summary insights

### Quick

- Rapid sentiment overview
- High-level scores
- Top 3 recommendations
- Executive summary only

## Key Features

### Sentiment Evolution Tracking

- Maps emotional journey throughout the conversation
- Identifies inflection points and their causes
- Correlates sentiment with specific topics

### Pattern Recognition

- Compares against successful deal patterns
- Identifies recurring objection themes
- Highlights effective sales techniques

### Actionable Insights

- Specific phrases to improve or avoid
- Optimal follow-up timing recommendations
- Risk mitigation strategies

## Example Output

# Sentiment Analysis Report: Q4 Sales Call - Acme Corp

**Date**: 2024-12-15

**Duration**: 45 minutes

## Executive Summary

- Overall Customer Sentiment: 7.5/10 ↑
- Sales Rep Confidence: 8/10
- Deal Probability: High
- Key Recommendation: Schedule technical deep-dive within 48 hours

## Critical Moments

### Pricing Discussion (15:30)

- Sentiment dropped from 8 to 6
- Recovery through value demonstration
- Recommendation: Lead with ROI metrics earlier

[... detailed analysis continues ...]

## Troubleshooting

### Common Issues

**Report Not Found**

- Verify the exact meeting title
- Check folder permissions in Google Drive
- Use auto-select mode to see available reports

**Access Denied**

- Ensure Google Drive MCP server is properly configured
- Verify you have read access to the folder

**Incomplete Analysis**

- Check if the meeting report contains transcript data
- Ensure sufficient content for analysis (minimum 10 minutes)

## Privacy & Security

- All analysis is performed locally through MCP servers
- No data is sent to external services
- Reports are saved to your local filesystem
- Google Drive access is read-only

## Best Practices

1. **Regular Analysis**: Analyze key meetings within 24 hours for timely coaching
1. **Pattern Building**: Analyze multiple meetings to identify trends
1. **Team Sharing**: Share insights with sales teams for collective improvement
1. **Follow-up Actions**: Use recommendations to guide next interactions

## License

MIT License. All rights reserved.

---

*Copyright © 2025 AlphaPoint.com*
