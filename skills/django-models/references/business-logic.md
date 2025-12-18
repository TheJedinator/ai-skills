# Financial Domain Business Logic

Domain-specific patterns and rules for financial application models.

## Table of Contents

- [Accounting](#accounting)
- [Payments](#payments)
- [Banking](#banking)
- [Foreign Exchange (FX)](#foreign-exchange-fx)
- [Cards](#cards)
- [Transactions](#transactions)

## Accounting

### Double-Entry Bookkeeping

Every financial movement creates balanced entries:

```
Debit Account A  + $100
Credit Account B - $100
Sum = 0 (always balanced)
```

**Model patterns to expect:**

- `JournalEntry` - Container for a balanced set of line items
- `JournalEntryLine` - Individual debit/credit entry
- `Account` / `Ledger` - Chart of accounts with types (Asset, Liability, Equity, Revenue, Expense)
- `AccountingPeriod` - Fiscal periods with open/close state

**Key constraints:**

- Journal entries must balance (sum of debits = sum of credits)
- Closed periods reject new entries
- Reversals create counter-entries, never delete

### Reconciliation

Matching internal records with external sources:

- `Reconciliation` model tracks matching state
- Status flow: `unmatched` → `matched` → `reconciled`
- Discrepancies tracked with variance amount and notes

## Payments

### State Machine Pattern

Payments follow strict status transitions:

```
pending → processing → completed
                    ↘ failed → retry → processing
                              ↘ cancelled
```

**Model patterns:**

- `Payment` with `status` field (typically CharField with choices)
- `PaymentAttempt` - Each processing attempt (for retry tracking)
- `PaymentMethod` - Card, bank account, etc.

**Key rules:**

- Completed payments are immutable
- Failed payments need failure codes and messages
- Idempotency keys prevent duplicate processing

### Idempotency

Critical for payment operations:

- Store `idempotency_key` (UUID) on payment models
- Check for existing key before processing
- Return cached result for duplicate requests

## Banking

### Account Models

```
BankAccount
├── account_number (encrypted)
├── routing_number
├── balance (computed or cached)
├── status (active, frozen, closed)
└── account_type (checking, savings)
```

**Balance management:**

- `available_balance` vs `pending_balance`
- Holds/authorizations reduce available but not ledger balance
- Settlement moves pending to available

### Transaction Integrity

- Use database transactions for balance updates
- `select_for_update()` to prevent race conditions
- Audit trail on all balance changes

## Foreign Exchange (FX)

### Exchange Rates

```
ExchangeRate
├── source_currency
├── target_currency
├── rate (DecimalField, high precision)
├── effective_at (timestamp)
└── provider (rate source)
```

**Key patterns:**

- Rates are time-series data (never update, insert new)
- Inverse rates: `1 / rate` with precision handling
- Rate margins/spreads for customer-facing rates

**Calculation rules:**

- Never mix currencies in arithmetic
- Convert to common currency for comparison
- Store original + converted amounts for audit
