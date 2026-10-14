
**Don't:**
- List activities without outcomes ("Built feature X")
- Use vague language without metrics ("improved efficiency", "enhanced quality")
- Claim full credit for team achievements without clarifying your specific role
- Focus on effort/hours worked rather than impact delivered
- Include work that had no measurable outcome
- Ignore the business impact area connections
- Forget to quantify where data is available
- Over-focus on technical details without business context

**Do:**
- Start with the outcome/result
- Use specific numbers and metrics
- Clarify your unique contribution
- Connect technical work to business impact
- Show the breadth and depth of impact
- Be honest about collaborative efforts
- Provide context for significance

---

## Common Mistakes

| Mistake | Why It's a Problem | How to Fix |
|---------|-------------------|------------|
| **Listing activities without outcomes** | "Built feature X" doesn't show impact or value delivered | Ask "What changed because of this work?" Add metrics showing the result |
| **Using vague metrics** | "Improved efficiency" is unmeasurable and unclear | Specify exact numbers: "Reduced API latency by 75% (800ms → 200ms)" |
| **Claiming team credit as personal** | Overclaiming damages credibility and collaboration perception | Clarify: "I led X, which enabled the team to achieve Y" |
| **Forgetting business context** | Technical achievements without business tie-in lack strategic value | Connect technical work to customer/revenue/operational outcomes |
| **Skipping quantification** | Impact without numbers is hard to calibrate | Add metrics: percentages, time saved, customers affected, $ value |
| **Mixing activity and outcome** | "Improved API and it performed better" conflates what you did with what happened | Separate: Activity = "Optimized 12 queries", Outcome = "Reduced latency 75%" |
| **Not mapping to impact areas** | Missing connection to Float's business priorities | Explicitly tag each outcome with 1-2 of Float's 5 impact areas |
| **Stopping at Phase 2** | Activities + outcomes alone don't show strategic alignment | Complete all 5 phases: brainstorm → outcomes → mapping → quantify → scope |

---

## Success Criteria

You're done with the WHAT when you have:

- ✓ Comprehensive list of work from the review period
- ✓ Each item has a clear, measurable outcome (not just activity)
- ✓ Outcomes are mapped to Float's 5 business impact areas
- ✓ Your specific contribution is clear for each item
- ✓ Metrics and quantification wherever possible
- ✓ Scope and significance assessed for each outcome
- ✓ A structured summary ready to share in your review

---

## Next Step

Once you've completed documenting the WHAT, you're ready to move to **float-performance-how** which will help you:
- Identify the key strengths that enabled your impact
- Connect your work to Float's values
- Calibrate against your level expectations
- Build compelling narrative examples

---

## Example: Good vs. Great Documentation

### ❌ Weak Example
**Impact Area:** Operational Excellence

**Outcome:** Improved API performance

**Your Contribution:** Optimized database queries and added caching

### ✓ Better Example
**Impact Area:** Operational Excellence

**Outcome:** Reduced API p95 latency from 800ms to 200ms (75% improvement)

**Context:** Our core transaction API was experiencing slowdowns during peak hours, causing customer dashboard timeouts and increasing support load.

**Your Contribution:** Analyzed query patterns, optimized 12 high-frequency database queries, implemented Redis caching for frequently accessed data, and added performance monitoring to prevent regression.

**Impact:** 
- Reduced API latency by 75% (800ms → 200ms p95)
- Eliminated dashboard timeouts for 95% of customers
- Reduced "slow dashboard" support tickets by 60% (30/week → 12/week)
- Improved customer NPS scores for dashboard experience from 7.2 to 8.5

**Scope:** Cross-functional (affected Engineering, CS, and 10K+ customers)

### ✨ Great Example (Shows Multiple Impact Areas)
**Impact Areas:** Customer Satisfaction + Operational Excellence

**Outcome:** Reduced API p95 latency by 75% and customer dashboard timeouts by 95%, directly improving customer experience and operational efficiency

**Context:** Our core transaction API was the #1 source of customer complaints (30 tickets/week) and was blocking several roadmap features due to unreliable performance during peak hours (11am-2pm daily).

**Your Contribution:** 
- Led performance investigation identifying 3 root causes (N+1 queries, missing indexes, lack of caching)
- Redesigned query patterns for 12 high-frequency endpoints
- Implemented tiered Redis caching strategy reducing database load by 40%
- Built performance monitoring dashboard with automated alerting to prevent regression
- Created runbook for on-call team to handle performance incidents

**Impact:**
- **Customer Satisfaction:**
  - Reduced "slow dashboard" support tickets by 60% (30/week → 12/week)
  - Improved customer NPS for dashboard experience from 7.2 to 8.5 (+18%)
  - Eliminated timeout errors for 95% of customers during peak hours
  - Unblocked 3 roadmap features that required reliable API performance
- **Operational Excellence:**
  - Reduced API p95 latency by 75% (800ms → 200ms)
