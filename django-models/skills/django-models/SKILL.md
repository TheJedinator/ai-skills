---
name: django-model-context
description: Navigate and understand Django models in any codebase. Use when users ask about database schema, model relationships, ORM queries, or need to understand data structures. Triggers on questions like "What models relate to X?", "Show me the Y model", "How do I query Z?", "What are the relationships between...", or any Django ORM/model exploration task.
---

# Django Model Context

Navigate Django models efficiently using native tools (Grep, Glob, Read).

## Model Discovery

Find models with these patterns:

```bash
# Find all model definitions
Grep: "class \w+\(.*Model.*\):" --type py

# Find specific model
Grep: "class Transaction\(" --type py

# Find models in a domain
Glob: "**/payments/**/models.py" then Read
```

### Common Base Classes

Search for classes inheriting from:

- `models.Model`, `Model`
- `AbstractBaseModel`, `BaseModel`
- `TimeStampedModel`, `UUIDModel`

### Standard Locations

```
app_name/models.py           # Single file
app_name/models/__init__.py  # Package with imports
app_name/models/user.py      # Split by entity
*/apis/*/models.py           # API-organized projects
```

## Understanding Models

### Field Types Quick Reference

| Field | Purpose | Key Args |
|-------|---------|----------|
| `CharField` | Short text | `max_length` |
| `TextField` | Long text | - |
| `IntegerField` | Whole numbers | - |
| `DecimalField` | Exact decimals | `max_digits`, `decimal_places` |
| `BooleanField` | True/False | `default` |
| `DateTimeField` | Timestamps | `auto_now`, `auto_now_add` |
| `JSONField` | Structured data | - |
| `UUIDField` | Unique identifiers | `default=uuid.uuid4` |

### Relationship Fields

| Field | Cardinality | Navigation |
|-------|-------------|------------|
| `ForeignKey` | Many-to-one | `instance.field`, `RelatedModel.objects.filter(field=instance)` |
| `OneToOneField` | One-to-one | `instance.field`, `instance.reverse_name` |
| `ManyToManyField` | Many-to-many | `instance.field.all()`, `instance.field.add()` |

### Reverse Relations

ForeignKey creates automatic reverse accessor:

```python
# If Transaction has: vendor = ForeignKey(Vendor, related_name='transactions')
vendor.transactions.all()  # All transactions for this vendor
```

Default reverse name: `<model_name>_set` (e.g., `transaction_set`)

## Navigation Strategies

### Trace Relationships

1. Read the model file
2. Find ForeignKey/M2M fields
3. Note `related_name` for reverse access
4. Follow chain: `A -> B -> C`

### Map a Domain

```bash
# Find all models in a domain
Glob: "**/accounting/**/models.py"
# Then grep for relationships
Grep: "ForeignKey|ManyToMany" in those files
```

### Find Usage

```bash
# Where is this model imported?
Grep: "from.*models import.*Transaction"

# Where is it queried?
Grep: "Transaction\.objects\."
```

## Common Query Patterns

```python
# Filter with relationships
Transaction.objects.filter(vendor__name="Acme")

# Prefetch to avoid N+1
Transaction.objects.prefetch_related('line_items')

# Select related for FK
Transaction.objects.select_related('vendor', 'created_by')

# Aggregate
Transaction.objects.filter(status='completed').aggregate(Sum('amount'))

# Annotate
Vendor.objects.annotate(total=Sum('transactions__amount'))
```

## Domain-Specific Patterns

For business logic patterns in financial domains (accounting, payments, banking, FX), see [references/business-logic.md](references/business-logic.md).
