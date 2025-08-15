# Competitive Intelligence Analyzer
---
description: >
  Advanced competitive intelligence tool that analyzes Read AI meeting transcripts from Google Drive
  to track competitor mentions, assess differentiation strategies, correlate with win/loss outcomes,
  and generate actionable insights for sales enablement and product positioning.
allowed_tools:
  - mcp__google-docs-mcp     # Google Docs access and manipulation
  - mcp__zapier              # Google Drive integration and automation
  - filesystem               # Local file operations and report generation
---

## Arguments

```
/competitive-intelligence
meeting_identifier="<meeting name or ID from Read AI>"
[drive_folder="AI Labs/Meeting Notes (Read AI)"]
[date_range="YYYY-MM-DD to YYYY-MM-DD"]
[competitors="<comma-separated list>"]
[analysis_depth=<comprehensive|standard|quick>]
[focus_areas="<mentions,features,pricing,positioning,all>"]
[win_loss_correlation=<yes|no>]
[generate_battlecard=<yes|no>]
[output_format=<markdown|json|html|all>]
```

### Default Values
- `drive_folder`: "AI Labs/Meeting Notes (Read AI)"
- `analysis_depth`: comprehensive
- `focus_areas`: all
- `win_loss_correlation`: yes
- `generate_battlecard`: yes
- `output_format`: markdown

### Examples

```bash
# Analyze single meeting for all competitor mentions
/competitive-intelligence meeting_identifier="Q4 Enterprise Sales Call"

# Deep analysis with specific competitors over time period
/competitive-intelligence date_range="2025-01-01 to 2025-08-31" competitors="Competitor A,Competitor B" analysis_depth=comprehensive

# Quick analysis focused on pricing comparisons
/competitive-intelligence meeting_identifier="Product Demo" focus_areas=pricing analysis_depth=quick

# Generate battle card updates from recent meetings
/competitive-intelligence date_range="2025-07-01 to 2025-08-31" generate_battlecard=yes output_format=all
```

---

## Context

### 1. Initialize and Validate Access

```yaml
- Verify Google Drive access via Zapier MCP
- Confirm Read AI folder exists and contains transcripts
- Parse input parameters and set defaults
- Initialize tracking structures for competitor data
```

### 2. Transcript Discovery and Loading

**Single Meeting Mode:**
```yaml
- Use mcp__zapier__google_docs_find_a_document with meeting_identifier
- Load full transcript content via mcp__google-docs-mcp__readGoogleDoc
- Extract metadata (date, participants, outcome if available)
```

**Batch Analysis Mode:**
```yaml
- Use mcp__zapier__google_drive_find_a_file for date range
- Filter meetings within specified timeframe
- Load each transcript iteratively
- Track meeting metadata for correlation analysis
```

### 3. Competitor Detection and Extraction

**Competitor Identification:**
```yaml
- If competitors list provided: Use exact matching
- If not provided: Use smart detection patterns:
  - Common competitor names and variations
  - Competitive language patterns ("they have", "unlike them", "compared to")
  - Feature comparison indicators
  - Pricing comparison signals
```

**Context Extraction:**
```yaml
For each competitor mention:
- Extract surrounding context (3 sentences before/after)
- Identify mention type:
  - Direct comparison
  - Feature discussion
  - Pricing comparison
  - Win/loss reason
  - General reference
- Capture sales rep response
- Note customer sentiment/reaction
```

### 4. Differentiation Response Analysis

**Response Categorization:**
```yaml
For each competitor mention response:
- Classify response strategy:
  - Feature differentiation
  - Value proposition
  - Price justification
  - Partnership benefits
  - Technical superiority
  - Service/support advantages
  - Deflection/dismissal
  - Acknowledgment without counter
```

**Effectiveness Scoring:**
```yaml
Score responses (1-10) based on:
- Completeness of answer
- Confidence indicators
- Customer reaction/follow-up
- Topic closure success
- Conversion to positive discussion
```

### 5. Pattern Recognition and Analysis

**Competitor Mention Patterns:**
```yaml
- Frequency by competitor
- Context clustering (features, pricing, support, etc.)
- Meeting stage when mentioned (early, mid, late)
- Initiative source (customer vs sales rep)
```

**Sales Response Patterns:**
```yaml
- Most effective response strategies by competitor
- Response consistency across team
- Improvement areas identified
- Best practice examples extracted
```

### 6. Win/Loss Correlation

**Outcome Analysis:**
```yaml
If win_loss_correlation enabled:
- Extract deal outcome from transcript or metadata
- Correlate competitor mention patterns with outcomes:
  - Number of competitor mentions vs win rate
  - Specific competitors vs win rate
  - Response effectiveness vs outcome
  - Unaddressed concerns vs losses
```

**Statistical Insights:**
```yaml
- Win rate when competitors mentioned vs not mentioned
- Most challenging competitors by win rate
- Most effective counter-strategies by outcome
- Critical objections leading to losses
```

### 7. Counter-Positioning Strategy Identification

**Successful Strategies:**
```yaml
Extract and rank strategies by effectiveness:
- Feature superiority demonstrations
- ROI/value calculations
- Customer success stories
- Partnership advantages
- Integration capabilities
- Support differentiation
- Innovation roadmap discussions
```

**Strategy Templates:**
```yaml
For each successful strategy:
- Create reusable response template
- Include context triggers
- Provide supporting evidence points
- Note required follow-up actions
```

### 8. Battle Card Generation

**Competitor Profiles:**
```yaml
For each competitor:
- Strengths mentioned by customers
- Weaknesses/gaps identified
- Common objections
- Effective counter-points
- Win/loss statistics
- Pricing intelligence
```

**Recommended Updates:**
```yaml
- New competitor features discovered
- Pricing changes detected
- Positioning shifts observed
- Customer perception changes
- Market narrative updates
```

### 9. Training and Enablement Recommendations

**Individual Rep Coaching:**
```yaml
For each sales rep:
- Response effectiveness score
- Areas of strength
- Improvement opportunities
- Specific examples for coaching
- Recommended practice scenarios
```

**Team Training Priorities:**
```yaml
- Common weak areas across team
- New competitor threats requiring training
- Successful tactics to replicate
- Role-play scenarios based on real situations
```

### 10. Product Development Insights

**Feature Gap Analysis:**
```yaml
- Features frequently mentioned as competitor advantages
- Customer-validated competitive strengths
- Recurring product limitations
- Integration requirements
- Performance comparisons
```

**Roadmap Recommendations:**
```yaml
Priority recommendations based on:
- Frequency of mention
- Impact on win/loss
- Customer importance signals
- Competitive differentiation potential
```

### 11. Report Generation

**Markdown Report Structure:**
```markdown
# Competitive Intelligence Analysis Report
## Executive Summary
- Key findings and metrics
- Win/loss correlation insights
- Top competitive threats

## Competitor Mention Analysis
- Frequency and context breakdown
- Trend analysis over time
- Meeting stage patterns

## Sales Response Effectiveness
- Response categorization and scoring
- Top performer strategies
- Improvement opportunities

## Win/Loss Correlation
- Statistical analysis
- Critical factors identified
- Predictive indicators

## Battle Card Updates
- Competitor profiles
- New intelligence gathered
- Recommended positioning changes

## Training Recommendations
- Individual coaching plans
- Team training priorities
- Best practice examples

## Product Insights
- Feature gap analysis
- Roadmap recommendations
- Competitive positioning opportunities

## Action Items
- Immediate updates needed
- Training schedule
- Follow-up analysis recommendations
```

**JSON Export:**
```yaml
If output_format includes JSON:
- Structure data for CRM integration
- Include all metrics and scores
- Provide timestamp and metadata
- Enable programmatic analysis
```

### 12. File Operations and Delivery

```yaml
- Create output directory: ./competitive-intelligence-[date]/
- Write main report: competitive-intelligence-report.md
- Generate battle cards: battle-cards-update.md
- Create training guide: training-recommendations.md
- Export JSON data if requested
- Generate summary for immediate action
```

### Error Handling

```yaml
- Gracefully handle missing transcripts
- Skip corrupted or inaccessible files
- Provide partial results if batch processing fails
- Log all errors with context
- Suggest remediation steps
```

### Performance Optimization

```yaml
- Process transcripts in parallel when possible
- Cache competitor patterns for reuse
- Optimize regex patterns for speed
- Implement progress indicators for large batches
- Memory-efficient processing for long transcripts
```

---

## Success Metrics

The command should deliver:
1. **Comprehensive competitor mention tracking** with full context
2. **Quantified response effectiveness** with improvement recommendations
3. **Statistical win/loss correlation** with competitive factors
4. **Actionable battle card updates** based on real customer feedback
5. **Personalized training recommendations** with real examples
6. **Product development insights** validated by customer conversations
7. **Exportable data** for integration with other tools

## Best Practices

- Always maintain context around competitor mentions
- Score objectively based on customer reactions
- Extract verbatim quotes for training materials
- Identify patterns across multiple meetings
- Prioritize insights by business impact
- Generate actionable recommendations, not just analysis