# Competitive Intelligence Analyzer

## Overview

The Competitive Intelligence Analyzer is an advanced AI-powered tool that extracts and analyzes competitor mentions from Read AI meeting transcripts stored in Google Drive. It provides comprehensive insights into competitive positioning, sales response effectiveness, and strategic recommendations for sales enablement and product development.

## Key Features

### Competitor Detection & Tracking

- **Automatic Detection**: Smart pattern recognition identifies competitor mentions even without explicit names
- **Context Extraction**: Captures full context around each mention for accurate analysis
- **Multi-Competitor Support**: Track multiple competitors simultaneously
- **Trend Analysis**: Identify patterns over time and across different meeting types

### Sales Response Analysis

- **Response Categorization**: Classifies how sales reps handle competitor mentions
- **Effectiveness Scoring**: Rates responses based on customer reactions and outcomes
- **Best Practice Identification**: Extracts successful counter-positioning strategies
- **Consistency Tracking**: Measures response consistency across the sales team

### Win/Loss Correlation

- **Outcome Analysis**: Correlates competitor mentions with deal outcomes
- **Statistical Insights**: Provides win rate analysis by competitor presence
- **Critical Factor Identification**: Highlights competitive factors affecting deals
- **Predictive Indicators**: Identifies early warning signs in competitive situations

### Battle Card Generation

- **Dynamic Updates**: Generates battle card updates based on real customer feedback
- **Competitor Profiles**: Comprehensive profiles with strengths, weaknesses, and counters
- **Pricing Intelligence**: Captures pricing comparisons and justifications
- **Positioning Recommendations**: Suggests positioning updates based on market feedback

### Training & Enablement

- **Individual Coaching Plans**: Personalized recommendations for each sales rep
- **Team Training Priorities**: Identifies common areas for improvement
- **Real-World Examples**: Uses actual meeting excerpts for training materials
- **Role-Play Scenarios**: Creates practice scenarios based on real situations

### Product Development Insights

- **Feature Gap Analysis**: Identifies product features where competitors have advantages
- **Customer-Validated Insights**: Based on actual customer feedback, not assumptions
- **Roadmap Recommendations**: Prioritizes features by competitive impact
- **Market Positioning**: Suggests positioning opportunities based on competitive landscape

## Installation

### Prerequisites

1. **Claude Code with MCP Support**
- Ensure you have Claude Code installed with MCP capabilities enabled

2. **Required MCP Servers**

- `google-docs-mcp`: For accessing and reading Google Docs
- `zapier`: For Google Drive integration
- `filesystem`: For local file operations

3. **Google Drive Access**

- Access to the folder containing Read AI meeting transcripts
- Proper authentication configured through Zapier MCP

### Setup Steps

1. **Clone or Download**

```bash
# If adding to existing Commands.com repository

cd your-commands-repo

cp -r competitive-intelligence ./

```

2. **Configure MCP Servers**

Ensure the following MCP servers are installed and configured:

- Google Docs MCP for document access
- Zapier MCP with Google Drive connection
- Filesystem MCP (usually pre-installed)

3. **Update commands.yaml**

The command entry will be automatically added to your commands.yaml file

4. **Verify Access**

Test access to your Read AI transcripts folder in Google Drive

## Usage

### Basic Usage

```bash
# Analyze a single meeting

/competitive-intelligence meeting_identifier="Q4 Sales Call - Enterprise"

# Analyze meetings over a time period

/competitive-intelligence date_range="2025-01-01 to 2025-08-31"

# Focus on specific competitors

/competitive-intelligence meeting_identifier="Product Demo" competitors="CompetitorA,CompetitorB"

```

### Advanced Usage

```bash
# Comprehensive analysis with all features

/competitive-intelligence \

date_range="2025-07-01 to 2025-08-31" \

competitors="Acme Corp,TechCo,StartupX" \

analysis_depth=comprehensive \

focus_areas=all \

win_loss_correlation=yes \

generate_battlecard=yes \

output_format=all

# Quick pricing-focused analysis

/competitive-intelligence \

meeting_identifier="Pricing Discussion" \

focus_areas=pricing \

analysis_depth=quick \

output_format=markdown

# Team performance comparison

/competitive-intelligence \

date_range="2025-01-01 to 2025-08-31" \

focus_areas=positioning \

win_loss_correlation=yes \

output_format=json

```

## Parameters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `meeting_identifier` | string | required* | Name or ID of specific meeting to analyze |
| `drive_folder` | string | "AI Labs/Meeting Notes (Read AI)" | Google Drive folder path |
| `date_range` | string | - | Date range for batch analysis (YYYY-MM-DD to YYYY-MM-DD) |
| `competitors` | string | auto-detect | Comma-separated list of competitors to track |
| `analysis_depth` | enum | comprehensive | Level of analysis: comprehensive, standard, quick |
| `focus_areas` | string | all | Analysis focus: mentions, features, pricing, positioning, all |
| `win_loss_correlation` | boolean | yes | Correlate with deal outcomes |
| `generate_battlecard` | boolean | yes | Generate battle card updates |
| `output_format` | enum | markdown | Output format: markdown, json, html, all |

* Either `meeting_identifier` or `date_range` must be provided

## Output

The command generates a comprehensive analysis package:

### Output Structure

```
./competitive-intelligence-[timestamp]/

├── competitive-intelligence-report.md    # Main analysis report
├── battle-cards-update.md               # Battle card recommendations
├── training-recommendations.md          # Sales training guide
├── product-insights.md                  # Product development insights
└── data.json                            # Raw data (if requested)

```

### Report Sections

1. **Executive Summary**
- Key findings and metrics
- Competitive threat assessment
- Action priorities

2. **Competitor Analysis**

- Mention frequency and context
- Trend analysis
- Competitive positioning

3. **Sales Effectiveness**

- Response scoring
- Best practices
- Improvement areas

4. **Win/Loss Insights**

- Statistical correlation
- Critical success factors
- Risk indicators

5. **Actionable Recommendations**

- Battle card updates
- Training priorities
- Product roadmap input

## Use Cases

### Sales Enablement

- Update battle cards with real customer feedback
- Identify and replicate successful counter-positioning strategies
- Create training materials from actual competitive situations

### Sales Management

- Track team performance in competitive situations
- Identify coaching opportunities for individual reps
- Measure improvement over time

### Competitive Intelligence

- Monitor competitor positioning and messaging
- Track feature and pricing changes
- Identify emerging competitive threats

### Product Management

- Validate feature priorities with customer feedback
- Identify competitive gaps affecting win rates
- Guide roadmap decisions with market intelligence

### Revenue Operations

- Correlate competitive factors with deal outcomes
- Identify predictive indicators for deal risk
- Optimize sales process for competitive situations

## Best Practices

### For Optimal Results

1. **Regular Analysis**
- Run weekly or monthly to track trends
- Analyze immediately after important competitive deals

2. **Comprehensive Coverage**

- Include all customer-facing meetings
- Don't limit to just "competitive" calls

3. **Action Orientation**

- Share battle card updates with entire team
- Implement training recommendations promptly
- Feed insights to product team regularly

4. **Data Quality**

- Ensure Read AI transcripts are complete
- Verify meeting outcomes are documented
- Maintain consistent competitor naming

### Important Considerations

- **Privacy**: Ensure compliance with data handling policies
- **Accuracy**: Verify critical insights before major decisions
- **Context**: Always consider full context of competitor mentions
- **Objectivity**: Balance competitive intelligence with your unique value

## Troubleshooting

### Common Issues

**Issue**: Cannot access Google Drive folder

- **Solution**: Verify Zapier MCP connection and permissions

**Issue**: No competitor mentions found

- **Solution**: Check transcript quality and adjust detection patterns

**Issue**: Missing meeting outcomes

- **Solution**: Ensure CRM integration or manual outcome tracking

**Issue**: Slow performance on large batches

- **Solution**: Process in smaller date ranges or use quick analysis mode

## Version History

- **v1.0.0** (2025-08-13): Initial release with full competitive intelligence capabilities

## License

MIT License. All rights reserved.

---

*Copyright © 2025 AlphaPoint Corp.*
