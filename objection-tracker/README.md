\# Objection Tracker for Read AI Sales Meetings

\## Overview

The \*\*Objection Tracker\*\* is an advanced AI-powered command that analyzes sales meeting transcripts from Read AI to extract, categorize, and provide actionable insights on customer objections. This tool helps sales teams understand objection patterns, improve handling strategies, and develop targeted training programs.

\## Key Features

\### 🎯 Objection Detection

- Automatically identifies customer objections using advanced keyword and pattern recognition
- Captures context around each objection for deeper understanding
- Detects subtle concerns and hesitation markers

\### 📊 Intelligent Categorization

- \*\*Budget/Pricing\*\*: Cost concerns, ROI questions, budget constraints
- \*\*Trust/Credibility\*\*: Reliability concerns, product maturity questions
- \*\*Functionality\*\*: Feature gaps, technical limitations
- \*\*Competition\*\*: Competitor comparisons, alternative solutions
- \*\*Timeline\*\*: Timing issues, implementation concerns
- \*\*Authority\*\*: Decision-making process, approval requirements

\### 📈 Performance Analytics

- Resolution rate calculation by objection type and sales rep
- Comparative analysis across team members
- Response effectiveness scoring
- Deal outcome correlation

\### 📚 Playbook Generation

- Automated creation of objection-handling playbooks
- Best practice extraction from top performers
- Response templates proven to work
- Common mistakes to avoid

\### 🎓 Training Recommendations

- Individual coaching plans for each sales rep
- Team-wide training priorities
- Role-play scenarios and practice exercises
- Customized improvement strategies

\## Installation

\### Prerequisites

1. \*\*Claude Code\*\* with MCP support enabled
1. \*\*Google Drive Access\*\* with Read AI meeting transcripts
1. \*\*Required MCP Servers\*\*:
- Google Docs MCP (`google-docs-mcp`)
- Zapier MCP (`zapier`)
- Filesystem MCP (`filesystem`)

\### Setup Steps

1. \*\*Clone the Repository\*\*

\```bash

git clone https://github.com/yourusername/objection-tracker.git

cd objection-tracker

\```

1. \*\*Configure MCP Servers\*\*

Ensure the following MCP servers are installed and configured:

- Google Docs MCP for document access
- Zapier MCP for Google Drive integration
- Filesystem MCP for local file operations

3\. \*\*Set Google Drive Permissions\*\*

- Grant access to folders containing Read AI transcripts
- Default folder: "AI Labs/Meeting Notes (Read AI)"

\## Usage

\### Basic Command

\```bash

/objection-tracker meeting\_identifier="Q4 Sales Call - Acme Corp"

\```

\### Advanced Options

\#### Analyze Multiple Meetings

\```bash

/objection-tracker

date\_range="2025-01-01 to 2025-08-31"

analysis\_mode=historical

generate\_playbook=yes

\```

\#### Compare Sales Rep Performance

\```bash

/objection-tracker

rep\_names="John Smith,Jane Doe,Mike Johnson"

analysis\_mode=comparative

output\_format=markdown

\```

\#### Focus on Specific Objections

\```bash

/objection-tracker

meeting\_identifier="Product Demo - TechCorp"

objection\_focus="pricing,competition"

resolution\_threshold=80

\```

\## Parameters

| Parameter | Type | Default | Description |

\|-----------|------|---------|-------------|

| `meeting\_identifier` | string | required | Meeting name or ID to analyze |

| `drive\_folder` | string | "AI Labs/Meeting Notes (Read AI)" | Google Drive folder path |

| `date\_range` | string | - | Date range for multiple meetings (YYYY-MM-DD to YYYY-MM-DD) |

| `rep\_names` | string | - | Comma-separated list of sales reps |

| `analysis\_mode` | enum | single | Analysis type: single, comparative, historical |

| `objection\_focus` | string | - | Specific objection types to focus on |

| `output\_format` | enum | markdown | Output format: markdown, json, html |

| `generate\_playbook` | boolean | yes | Generate objection-handling playbook |

| `resolution\_threshold` | number | 70 | Minimum acceptable resolution rate (%) |

\## Output Structure

\### Generated Files

\```

objection-analysis-[date]/

├── objection-analysis-report.md     # Main analysis report

├── objection-handling-playbook.md   # Actionable playbook

├── training-recommendations.md      # Training plans

└── objection-data.json             # Raw data (optional)

\```

\### Report Sections

1. \*\*Executive Summary\*\*: Key findings and critical metrics
1. \*\*Objection Analysis\*\*: Detailed breakdown by category
1. \*\*Rep Performance\*\*: Individual and comparative metrics
1. \*\*Response Effectiveness\*\*: What works and what doesn't
1. \*\*Playbook\*\*: Ready-to-use response templates
1. \*\*Training Plan\*\*: Specific recommendations for improvement

\## Example Output

\### Objection Analysis Sample

\```markdown

\## Budget/Pricing Objections

- \*\*Frequency\*\*: 23 occurrences (38% of total)
- \*\*Resolution Rate\*\*: 65%
- \*\*Average Severity\*\*: 3.2/5

\### Top Objection: "Too expensive compared to current solution"

\*\*Best Response Template\*\*:

"I understand cost is important. Let me show you the ROI calculation

based on similar clients who saw 40% efficiency gains..."

\*\*Success Rate\*\*: 78% when using ROI data

\```

\### Rep Performance Matrix

\```markdown

| Sales Rep | Resolution Rate | Strength | Development Area |

\|-----------|----------------|----------|------------------|

| John S.   | 82%           | Pricing  | Competition      |

| Jane D.   | 75%           | Trust    | Timeline         |

| Mike J.   | 68%           | Features | Pricing          |

\```

\## Best Practices

\### 🎯 For Best Results

1. Ensure meeting transcripts include clear speaker identification
1. Use consistent naming conventions for meetings
1. Include meeting outcomes in transcript metadata
1. Run analysis regularly to track improvement

\### 📊 Interpreting Results

- \*\*Resolution Rate > 70%\*\*: Good objection handling
- \*\*Resolution Rate 50-70%\*\*: Needs improvement
- \*\*Resolution Rate < 50%\*\*: Critical training needed

\### 🔄 Continuous Improvement

1. Review playbook monthly and update based on new patterns
1. Track resolution rate improvements after training
1. Share successful response templates across team
1. Create objection-specific battle cards

\## Troubleshooting

\### Common Issues

\*\*Issue\*\*: Cannot find meeting transcripts

- \*\*Solution\*\*: Verify Google Drive folder path and permissions

\*\*Issue\*\*: Objections not detected accurately

- \*\*Solution\*\*: Check transcript format and speaker identification

\*\*Issue\*\*: Low resolution rates across team

- \*\*Solution\*\*: Focus on training for top 3 objection categories

\## Support

For issues or feature requests, please contact:

- Email: support@your-company.com
- Documentation: [https://docs.your-company.com/objection-tracker](https://docs.your-company.com/objection-tracker)

\## License

This command is proprietary software. All rights reserved.

\---

* Built with Claude Code and MCP integration for advanced sales intelligence\*
