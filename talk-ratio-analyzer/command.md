---
description: >
  Advanced conversation dynamics analyzer that calculates talk-to-listen ratios from Read AI meeting transcripts,
  measures question effectiveness, segments by call type, and correlates patterns with deal outcomes to identify
  coaching opportunities and top performer techniques.
allowed_tools:
  - mcp__google-docs-mcp
  - mcp__zapier
  - mcp__filesystem
---

## Arguments

```
/talk-ratio-analyzer
meeting_identifier=<meeting_name_or_id>
[drive_folder="AI Labs/Meeting Notes (Read AI)"]
[date_range="YYYY-MM-DD to YYYY-MM-DD"]
[rep_names="rep1,rep2,rep3"]
[call_types="discovery,demo,negotiation,closing"]
[analysis_mode="single|comparative|historical"]
[question_analysis="yes|no"]
[outcome_correlation="yes|no"]
[coaching_report="yes|no"]
[output_format="markdown|json|html"]
```

### Examples

```bash
# Analyze single meeting with full insights
/talk-ratio-analyzer meeting_identifier="Q4 Discovery Call - Enterprise Client"

# Comparative analysis across team
/talk-ratio-analyzer date_range="2025-01-01 to 2025-08-31" rep_names="John Smith,Jane Doe,Mike Wilson" analysis_mode=comparative

# Historical pattern analysis with outcome correlation
/talk-ratio-analyzer analysis_mode=historical outcome_correlation=yes coaching_report=yes

# Quick single meeting check
/talk-ratio-analyzer meeting_identifier="Product Demo" question_analysis=no coaching_report=no
```

---

## Context

### 1. Initialize and Parse Arguments
* Extract `meeting_identifier` - specific meeting or batch processing
* Parse `drive_folder` - default to "AI Labs/Meeting Notes (Read AI)"
* Process `date_range` - for multi-meeting analysis
* Parse `rep_names` - comma-separated list for filtering
* Identify `call_types` - discovery, demo, negotiation, closing
* Set `analysis_mode` - single, comparative, or historical
* Configure analysis options (question_analysis, outcome_correlation, coaching_report)
* Set `output_format` - markdown (default), json, or html

### 2. Access Google Drive and Locate Transcripts
```javascript
// Use Zapier MCP to find Read AI transcripts
mcp__zapier__google_drive_find_a_file({
  instructions: `Find Read AI meeting transcripts in folder "${drive_folder}"`,
  folder: drive_folder,
  title: meeting_identifier || "*", 
  file_types: "document"
})

// For date range filtering
mcp__zapier__google_drive_retrieve_files_from_google_drive({
  instructions: `Get all Read AI transcripts from ${date_range}`,
  customQuery: `name contains 'Read AI' and modifiedTime >= '${start_date}'`,
  pageSize: 50
})
```

### 3. Extract and Parse Transcript Content
```javascript
// Read transcript content
mcp__google-docs-mcp__readGoogleDoc({
  documentId: transcript_id,
  format: "text",
  maxLength: 100000
})

// Parse speaker segments
const parseTranscript = (content) => {
  const segments = [];
  const speakerPattern = /^([A-Za-z\s]+):\s*(.+)$/gm;
  
  let match;
  while ((match = speakerPattern.exec(content)) !== null) {
    segments.push({
      speaker: match[1].trim(),
      text: match[2].trim(),
      wordCount: match[2].split(/\s+/).length,
      timestamp: extractTimestamp(match[2])
    });
  }
  
  return segments;
};

// Identify speaker roles (sales rep vs customer)
const identifySpeakerRoles = (segments) => {
  const knownReps = rep_names.split(',').map(n => n.trim());
  
  return segments.map(seg => ({
    ...seg,
    role: knownReps.some(rep => seg.speaker.includes(rep)) ? 'rep' : 'customer'
  }));
};
```

### 4. Calculate Talk-to-Listen Ratios
```javascript
const calculateRatios = (segments) => {
  const stats = {
    rep: { words: 0, time: 0, segments: 0 },
    customer: { words: 0, time: 0, segments: 0 }
  };
  
  segments.forEach(seg => {
    stats[seg.role].words += seg.wordCount;
    stats[seg.role].time += estimateSpeakingTime(seg.wordCount);
    stats[seg.role].segments++;
  });
  
  const totalWords = stats.rep.words + stats.customer.words;
  const totalTime = stats.rep.time + stats.customer.time;
  
  return {
    repTalkPercentage: (stats.rep.words / totalWords) * 100,
    customerTalkPercentage: (stats.customer.words / totalWords) * 100,
    ratio: `${Math.round(stats.rep.words / totalWords * 100)}:${Math.round(stats.customer.words / totalWords * 100)}`,
    repAvgSegmentLength: stats.rep.words / stats.rep.segments,
    customerAvgSegmentLength: stats.customer.words / stats.customer.segments,
    totalDuration: totalTime,
    segmentStats: stats
  };
};

// Estimate speaking time (150 words per minute average)
const estimateSpeakingTime = (wordCount) => wordCount / 150;
```

### 5. Analyze Question Effectiveness
```javascript
const analyzeQuestions = (segments) => {
  const questions = {
    openEnded: [],
    closed: [],
    followUp: [],
    probing: []
  };
  
  // Open-ended question patterns
  const openPatterns = [
    /^(what|how|why|tell me|describe|explain|share)/i,
    /could you (tell|explain|describe|share)/i,
    /help me understand/i,
    /walk me through/i
  ];
  
  // Closed question patterns
  const closedPatterns = [
    /^(is|are|do|does|did|can|could|will|would|have|has)/i,
    /\?$/
  ];
  
  segments.forEach((seg, idx) => {
    if (seg.role === 'rep' && seg.text.includes('?')) {
      const question = {
        text: seg.text,
        type: 'unknown',
        responseLength: 0
      };
      
      // Classify question type
      if (openPatterns.some(p => p.test(seg.text))) {
        question.type = 'open-ended';
        questions.openEnded.push(question);
      } else if (closedPatterns.some(p => p.test(seg.text))) {
        question.type = 'closed';
        questions.closed.push(question);
      }
      
      // Measure response length
      if (idx < segments.length - 1 && segments[idx + 1].role === 'customer') {
        question.responseLength = segments[idx + 1].wordCount;
      }
    }
  });
  
  return {
    totalQuestions: questions.openEnded.length + questions.closed.length,
    openEndedCount: questions.openEnded.length,
    closedCount: questions.closed.length,
    openEndedRatio: questions.openEnded.length / (questions.openEnded.length + questions.closed.length),
    avgResponseLength: {
      openEnded: average(questions.openEnded.map(q => q.responseLength)),
      closed: average(questions.closed.map(q => q.responseLength))
    },
    effectiveness: calculateQuestionEffectiveness(questions)
  };
};

const calculateQuestionEffectiveness = (questions) => {
  // Score based on open-ended ratio and response length
  const openRatioScore = questions.openEnded.length / (questions.openEnded.length + questions.closed.length) * 50;
  const responseLengthScore = Math.min(50, (average(questions.openEnded.map(q => q.responseLength)) / 50) * 50);
  
  return {
    score: openRatioScore + responseLengthScore,
    grade: getGrade(openRatioScore + responseLengthScore),
    insights: generateQuestionInsights(questions)
  };
};
```

### 6. Segment by Call Type and Apply Optimal Ratios
```javascript
const callTypeOptimalRatios = {
  discovery: { rep: 30, customer: 70, description: "Customer should do most talking" },
  demo: { rep: 50, customer: 50, description: "Balanced conversation with engagement" },
  negotiation: { rep: 40, customer: 60, description: "Customer concerns drive discussion" },
  closing: { rep: 35, customer: 65, description: "Customer questions and commitment" }
};

const analyzeCallTypeAlignment = (ratios, callType) => {
  const optimal = callTypeOptimalRatios[callType];
  const deviation = Math.abs(ratios.repTalkPercentage - optimal.rep);
  
  return {
    callType,
    optimal,
    actual: {
      rep: ratios.repTalkPercentage,
      customer: ratios.customerTalkPercentage
    },
    deviation,
    alignment: deviation <= 10 ? 'excellent' : deviation <= 20 ? 'good' : 'needs improvement',
    recommendations: generateCallTypeRecommendations(ratios, optimal, callType)
  };
};

const generateCallTypeRecommendations = (actual, optimal, callType) => {
  const recommendations = [];
  
  if (actual.repTalkPercentage > optimal.rep + 10) {
    recommendations.push({
      issue: "Talking too much",
      suggestion: `For ${callType} calls, aim to speak only ${optimal.rep}% of the time`,
      techniques: [
        "Ask more open-ended questions",
        "Practice active listening",
        "Pause after speaking to invite response",
        "Use the 3-second rule before jumping in"
      ]
    });
  }
  
  if (actual.repTalkPercentage < optimal.rep - 10) {
    recommendations.push({
      issue: "Not guiding conversation enough",
      suggestion: `For ${callType} calls, you should speak about ${optimal.rep}% of the time`,
      techniques: [
        "Prepare key talking points",
        "Share relevant insights and value",
        "Bridge between customer comments",
        "Summarize and confirm understanding"
      ]
    });
  }
  
  return recommendations;
};
```

### 7. Correlate with Deal Outcomes
```javascript
const correlateWithOutcomes = async (meetings) => {
  // Extract deal outcomes from transcript metadata or CRM integration
  const outcomeData = meetings.map(meeting => {
    const outcome = extractDealOutcome(meeting.content);
    const ratios = calculateRatios(meeting.segments);
    const questions = analyzeQuestions(meeting.segments);
    
    return {
      meetingId: meeting.id,
      repName: meeting.repName,
      callType: meeting.callType,
      talkRatio: ratios.repTalkPercentage,
      questionEffectiveness: questions.effectiveness.score,
      outcome: outcome.status, // won, lost, pending
      dealSize: outcome.dealSize,
      cycleLength: outcome.cycleLength
    };
  });
  
  // Statistical analysis
  const correlations = {
    talkRatioToWinRate: calculateCorrelation(
      outcomeData.map(d => d.talkRatio),
      outcomeData.map(d => d.outcome === 'won' ? 1 : 0)
    ),
    questionEffectivenessToWinRate: calculateCorrelation(
      outcomeData.map(d => d.questionEffectiveness),
      outcomeData.map(d => d.outcome === 'won' ? 1 : 0)
    ),
    optimalRanges: findOptimalRanges(outcomeData)
  };
  
  return correlations;
};

const findOptimalRanges = (data) => {
  const wonDeals = data.filter(d => d.outcome === 'won');
  
  return {
    talkRatio: {
      min: Math.min(...wonDeals.map(d => d.talkRatio)),
      max: Math.max(...wonDeals.map(d => d.talkRatio)),
      optimal: average(wonDeals.map(d => d.talkRatio))
    },
    questionScore: {
      min: Math.min(...wonDeals.map(d => d.questionEffectiveness)),
      max: Math.max(...wonDeals.map(d => d.questionEffectiveness)),
      optimal: average(wonDeals.map(d => d.questionEffectiveness))
    }
  };
};
```

### 8. Identify Coaching Opportunities
```javascript
const identifyCoachingOpportunities = (repAnalysis) => {
  const coaching = {
    priority: [],
    strengths: [],
    improvements: []
  };
  
  // Analyze each rep's performance
  repAnalysis.forEach(rep => {
    const score = calculatePerformanceScore(rep);
    
    if (score < 60) {
      coaching.priority.push({
        repName: rep.name,
        score,
        issues: identifyMainIssues(rep),
        actionPlan: generateActionPlan(rep)
      });
    } else if (score > 80) {
      coaching.strengths.push({
        repName: rep.name,
        score,
        techniques: extractTopTechniques(rep)
      });
    } else {
      coaching.improvements.push({
        repName: rep.name,
        score,
        opportunities: identifyImprovementAreas(rep)
      });
    }
  });
  
  return coaching;
};

const generateActionPlan = (rep) => {
  const plan = {
    immediate: [],
    shortTerm: [],
    longTerm: []
  };
  
  // Talk ratio issues
  if (rep.avgTalkRatio > 60) {
    plan.immediate.push({
      action: "Practice the 30-second rule",
      description: "Limit responses to 30 seconds before asking a question",
      metric: "Reduce talk time by 20% in next 5 calls"
    });
  }
  
  // Question effectiveness issues
  if (rep.questionEffectiveness < 50) {
    plan.shortTerm.push({
      action: "Open-ended question training",
      description: "Complete workshop on consultative questioning",
      metric: "Achieve 60% open-ended question ratio"
    });
  }
  
  return plan;
};
```

### 9. Extract Top Performer Techniques
```javascript
const extractTopPerformerTechniques = (topPerformers) => {
  const techniques = {
    conversationFlow: [],
    questioningPatterns: [],
    listeningTechniques: [],
    engagementStrategies: []
  };
  
  topPerformers.forEach(performer => {
    // Analyze conversation patterns
    const patterns = analyzeConversationPatterns(performer.transcripts);
    
    techniques.conversationFlow.push(...patterns.flow);
    techniques.questioningPatterns.push(...patterns.questions);
    techniques.listeningTechniques.push(...patterns.listening);
    techniques.engagementStrategies.push(...patterns.engagement);
  });
  
  // Rank and deduplicate techniques
  return {
    top5ConversationTechniques: rankTechniques(techniques.conversationFlow).slice(0, 5),
    top5QuestionTypes: rankTechniques(techniques.questioningPatterns).slice(0, 5),
    top5ListeningBehaviors: rankTechniques(techniques.listeningTechniques).slice(0, 5),
    top5EngagementTactics: rankTechniques(techniques.engagementStrategies).slice(0, 5)
  };
};

const analyzeConversationPatterns = (transcripts) => {
  const patterns = {
    flow: [],
    questions: [],
    listening: [],
    engagement: []
  };
  
  transcripts.forEach(transcript => {
    // Identify successful patterns
    const segments = parseTranscript(transcript);
    
    // Flow patterns
    const transitions = findSmoothTransitions(segments);
    patterns.flow.push(...transitions);
    
    // Question patterns that generate long responses
    const effectiveQuestions = findEffectiveQuestions(segments);
    patterns.questions.push(...effectiveQuestions);
    
    // Active listening indicators
    const listeningCues = findListeningCues(segments);
    patterns.listening.push(...listeningCues);
    
    // Engagement tactics
    const engagementMethods = findEngagementMethods(segments);
    patterns.engagement.push(...engagementMethods);
  });
  
  return patterns;
};
```

### 10. Generate Comprehensive Report
```javascript
const generateMarkdownReport = (analysis) => {
  const report = `# Talk-to-Listen Ratio Analysis Report
Generated: ${new Date().toISOString()}

## Executive Summary
${generateExecutiveSummary(analysis)}

## Overall Metrics
### Talk Ratios by Representative
${generateRepTable(analysis.repMetrics)}

### Question Effectiveness Scores
${generateQuestionTable(analysis.questionAnalysis)}

## Call Type Analysis
${generateCallTypeAnalysis(analysis.callTypes)}

## Correlation with Deal Outcomes
${generateOutcomeCorrelation(analysis.correlations)}

## Coaching Recommendations

### Priority Coaching Needed
${generatePriorityCoaching(analysis.coaching.priority)}

### Top Performer Techniques
${generateTopTechniques(analysis.topPerformers)}

### Team Improvement Opportunities
${generateTeamOpportunities(analysis.teamAnalysis)}

## Individual Rep Reports
${generateIndividualReports(analysis.individualReports)}

## Action Items
${generateActionItems(analysis)}

## Appendix
### Methodology
${generateMethodology()}

### Optimal Ratio Guidelines
${generateGuidelines()}
`;

  return report;
};

// Save report to filesystem
mcp__filesystem__write_file({
  path: `./talk-ratio-analysis-${Date.now()}/report.md`,
  content: report
});

// Optional JSON export
if (output_format === 'json') {
  mcp__filesystem__write_file({
    path: `./talk-ratio-analysis-${Date.now()}/data.json`,
    content: JSON.stringify(analysis, null, 2)
  });
}
```

### 11. Continuous Improvement Loop
```javascript
// Track improvements over time
const trackProgress = (historicalData, currentAnalysis) => {
  const trends = {
    talkRatioTrend: calculateTrend(historicalData.talkRatios),
    questionEffectivenessTrend: calculateTrend(historicalData.questionScores),
    outcomeTrend: calculateTrend(historicalData.winRates)
  };
  
  return {
    improvements: identifyImprovements(trends),
    regressions: identifyRegressions(trends),
    recommendations: generateNextSteps(trends)
  };
};
```

## Success Metrics
- Accurate talk-to-listen ratio calculation
- Meaningful question effectiveness scoring
- Actionable coaching recommendations
- Clear correlation with deal outcomes
- Practical top performer technique extraction