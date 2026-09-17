# delivery-vs-cash

Reconciling ad spend on two bases at once, and the verification standard that makes either one worth quoting.

## The problem nobody names

Platforms report on delivery. Cards get charged on billing thresholds or invoice cycles. The two never match inside a month, and both are correct.

Most teams pick one basis without noticing they have picked one. Then finance asks why the report says one number and the bank says another, and the answer is a week of spreadsheet archaeology every time it comes up.

Run both bases deliberately, with a written bridge between them.

## The bridge

    delivered spend
      minus unbilled at period end
      plus carry-in from the prior period
      equals cash paid

Delivery basis answers performance questions and budget versus actual. Cash basis answers bank reconciliation. Platform fees and credit-funded spend are tracked separately, outside delivered spend, because they are neither.

Neither basis is wrong. The gap is timing, and timing reverses. **If a gap does not reverse in the next period, it was never timing and something is actually broken.** That single test is most of the value here.

The model earns trust when it predicts. On one program, the computed unbilled tail for a month matched, to the cent, the invoice that arrived on the first of the following month. A bridge that calls the invoice before it lands is a bridge that survives a finance review.

## What receipts are, and are not

Receipts and payment confirmations inside a billing window restate cumulatively. They are snapshots of a running total, not additive line items. Summing them overstates, and the overstatement is not small. On one folder it produced a month roughly 60% above the true figure, and the same folder held eight literal duplicate PDFs filed under different transaction IDs.

Three rules came out of that.

1. **Ads Manager for spend. Receipts for cash timing and fees.** Receipts were demoted from a spend measure to a cash-timing and fee-verification tool after being tested against a known-good source, not assumed to be wrong.
2. **Fingerprint the folder before reading it.** Content hashing exposed the duplicates. File names and transaction IDs did not.
3. **Do not model your way out of it.** Rebuilding daily spend from overlapping receipt windows with non-negative least squares left residuals it could not explain. The model was dropped rather than tuned. A reconciliation that needs a fitted model is not a reconciliation.

## The verification standard, written before you need it

Four checks. Each one has caught something real.

1. **Campaign rows must sum to the export's own account total.** Same file, two levels, no excuse.
2. **Monthly slices must sum to any cumulative report.** Zero variance, not close.
3. **Statement balances must chain.** Opening, plus activity, plus fees, minus payments, equals closing. Every month, to the penny.
4. **Two independent sources covering the same thing must agree to the penny before either is used.** Not the average of them. Agreement, or neither one ships.

Sub-dollar rounding residuals are accepted and disclosed. Anything larger is investigated until explained, and explained means a sentence naming the mechanism, not a plug line.

Two things this caught on one program. A recurring monthly discrepancy that was structurally closed once its mechanism was found rather than absorbed every month. And a gap between a platform's reported spend and its billed activity that turned out to be overdelivery plus credits that were never billed. The second one is worth sitting with. It looked like an error and it was the platform working as designed.

## Explain the gap, never close it by adjusting

When two systems disagree, neither number gets adjusted to match the other. The disagreement is published as a labelled residual with its explanation attached.

One platform's API-reported cost and its billed activity differed across seven months, traced to a single campaign that reported one figure and was billed a lower one. That residual is displayed, not smoothed.

A reader who can see the gap and the reason trusts the rest of the page. A reader who finds a gap you hid does not, and they stop trusting the parts that were fine.

## Attribution, when campaign names are the only key

Ordered pattern-matching rules map campaign names to the thing being funded. Order matters, because names collide.

Plan for the messy cases before they arrive, because they always arrive.

- A platform-side misspelling of your own product name
- Boosted organic posts carrying no identifying token, assigned by content
- An always-on search campaign that genuinely serves everything, held as unallocated until an owner makes the call

**Unmatched spend renders in red and never disappears.** An unallocated bucket you can see gets driven to zero. One that quietly distributes itself across the rows you were watching does not, and you will not find out until someone asks why a release cost more than it did.

Every owner decision gets dated in the rules file, next to the rule it created.

## One data block, every view a function of it

If the same figure appears in eight views, it comes from one place. The failure mode is not arithmetic, it is drift: a detail table and a summary tile disagreeing because they were built off different layers.

When a reconciled layer and a raw layer legitimately differ, emit the difference as an explicit adjustments row, so that no two views can disagree. Then assert, programmatically, that every view equals every other view for every period.

**That assertion is a build step, not a review step.** A consistency rule a human has to remember to run is a consistency rule that holds until the month you are busy.

Monthly close checklist in [CHECKLIST.md](CHECKLIST.md).
