# Financial Billing and Data Integrity

## Reported finance entities

| Entity | Responsibility in the billing model |
| --- | --- |
| `FeeStructure` | Defines a collection of configured fees |
| `FeeStructureItem` | Represents an item within a structure |
| `BillingRun` | Records a billing operation |
| `StudentBill` | Represents a bill for a student |
| `StudentBillLine` | Represents the billed line items |
| `FeeDiscountExemption` | Models a discount or exemption associated with a fee |

## The duplicate-billing problem

Billing can be retried after an interruption, operator mistake or ambiguous application response. A robust workflow should avoid charging the same billable source twice merely because the operation is repeated.

The project uses the reported concept of a `BillingSourceKey` to identify billing provenance and support **idempotency**: repeating a logically identical billing request should not create an unintended second charge.

### Illustrative process (not application source code)

```text
Receive billing request
  -> determine a stable identity for its billing source
  -> check whether that source was billed previously
  -> if already billed: return or reuse the existing outcome
  -> otherwise: create bill and its line items
  -> save consistently and record the run
```

This is a **conceptual flow**, not an exact excerpt from implemented code. In a production design, concurrency and database-level uniqueness require particular care: checking for an existing bill before insertion is not sufficient by itself to prevent races.

## Testing questions

- Does repeating the same billing request produce the same logical outcome?
- What happens after a partial failure?
- Can two requests for the same source run concurrently?
- Are discounts and exemptions applied consistently?
- Does authorization constrain who can issue a billing run?

These are review/test objectives; their presence here **does not assert that every scenario has already been covered by tests**.
