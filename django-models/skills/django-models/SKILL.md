---
name: django-models
description: Navigate and understand Django models in any codebase. Use when users ask about database schema, model relationships, ORM queries, or need to understand data structures. Triggers on questions like "What models relate to X?", "Show me the Y model", "How do I query Z?", "What are the relationships between...", or any Django ORM/model exploration task.
---

# Django Models

Navigate Django models efficiently using native tools (Grep, Glob, Read).

## Model Discovery

Find models with these patterns:

```bash
# Find all model definitions
Grep: "class \w+\(.*Model.*\):" --type py

# Find specific model
Grep: "class Entry\(" --type py

# Find models in an app
Glob: "**/blog/**/models.py" then Read
```

### Common Base Classes

Search for classes inheriting from:

- `models.Model` - Standard Django model
- `AbstractUser`, `AbstractBaseUser` - Custom user models
- `TimeStampedModel`, `UUIDModel` - Common third-party mixins (django-extensions, etc.)

### Find Django Apps

1. Look in settings for `INSTALLED_APPS`
2. Each app has models in `models.py` or `models/` package

### Standard Locations

```
app_name/models.py           # Single file (most common)
app_name/models/__init__.py  # Package with imports
app_name/models/entry.py     # Split by entity
```

## Reference Models

Standard Django documentation examples:

```python
class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

class Author(models.Model):
    name = models.CharField(max_length=200)
    email = models.EmailField()

class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateField()
    authors = models.ManyToManyField(Author)
```

## Field Types Quick Reference

| Field           | Purpose            | Key Args                       |
| --------------- | ------------------ | ------------------------------ |
| `CharField`     | Short text         | `max_length` (required)        |
| `TextField`     | Long text          | -                              |
| `IntegerField`  | Whole numbers      | -                              |
| `DecimalField`  | Exact decimals     | `max_digits`, `decimal_places` |
| `BooleanField`  | True/False         | `default`                      |
| `DateField`     | Date only          | `auto_now`, `auto_now_add`     |
| `DateTimeField` | Date and time      | `auto_now`, `auto_now_add`     |
| `EmailField`    | Email validation   | `max_length=254`               |
| `JSONField`     | Structured data    | `default=dict`                 |
| `UUIDField`     | Unique identifiers | `default=uuid.uuid4`           |

### Common Field Options

```python
field = models.CharField(
    max_length=100,
    null=True,          # Database allows NULL
    blank=True,         # Forms allow empty
    default='value',    # Default value
    choices=CHOICES,    # Limit to predefined options
    unique=True,        # Enforce uniqueness
    db_index=True,      # Create database index
)
```

## Relationship Fields

| Field             | Cardinality  | Example                                  |
| ----------------- | ------------ | ---------------------------------------- |
| `ForeignKey`      | Many-to-one  | Entry belongs to one Blog                |
| `OneToOneField`   | One-to-one   | Profile extends User                     |
| `ManyToManyField` | Many-to-many | Entry has multiple Authors               |

### ForeignKey Options

```python
blog = models.ForeignKey(
    Blog,
    on_delete=models.CASCADE,      # Delete entries when blog deleted
    related_name='entries',        # blog.entries.all()
    related_query_name='entry',    # Blog.objects.filter(entry__headline=...)
)
```

### on_delete Options

- `CASCADE` - Delete related objects
- `PROTECT` - Prevent deletion
- `SET_NULL` - Set to NULL (requires `null=True`)
- `SET_DEFAULT` - Set to default value
- `DO_NOTHING` - Take no action (use carefully)

### Reverse Relations

```python
# ForeignKey creates automatic reverse accessor
# Entry has: blog = ForeignKey(Blog, related_name='entries')

blog.entries.all()        # All entries for this blog
blog.entries.count()      # Count of entries
blog.entries.filter(...)  # Filtered entries
```

Default reverse name: `<model_name>_set` (e.g., `entry_set`)

## Navigation Strategies

### Trace Relationships

1. Read the model file
2. Find ForeignKey/M2M fields
3. Note `related_name` for reverse access
4. Follow chain: `Blog -> Entry -> Author`

### Map an App's Models

```bash
# Find all models in an app
Glob: "**/blog/**/models.py"
# Then grep for relationships
Grep: "ForeignKey|ManyToMany" in those files
```

### Find Model Usage

```bash
# Where is this model imported?
Grep: "from.*models import.*Entry"

# Where is it queried?
Grep: "Entry\.objects\."
```

## Common Query Patterns

```python
# Basic retrieval
Entry.objects.all()
Entry.objects.get(pk=1)
Entry.objects.first()

# Filtering
Entry.objects.filter(pub_date__year=2024)
Entry.objects.filter(headline__contains='Django')
Entry.objects.exclude(pub_date__gte=datetime.date.today())

# Chaining
Entry.objects.filter(blog__name='Tech').order_by('-pub_date')[:5]

# Spanning relationships
Entry.objects.filter(blog__name='Tech Blog')
Blog.objects.filter(entry__headline__contains='Django')

# Select related (reduces queries for ForeignKey)
Entry.objects.select_related('blog')

# Prefetch related (reduces queries for M2M/reverse FK)
Blog.objects.prefetch_related('entries')
Entry.objects.prefetch_related('authors')

# Aggregation
from django.db.models import Count, Avg, Sum
Blog.objects.annotate(num_entries=Count('entries'))
Entry.objects.aggregate(Avg('rating'))

# Q objects for complex queries
from django.db.models import Q
Entry.objects.filter(Q(headline__startswith='What') | Q(headline__startswith='Why'))

# F expressions for field comparisons
from django.db.models import F
Entry.objects.filter(num_comments__gt=F('num_pingbacks'))

# Update multiple objects
Entry.objects.filter(blog=blog).update(headline='Updated')
```

## Meta Options

```python
class Entry(models.Model):
    # ... fields ...

    class Meta:
        ordering = ['-pub_date']           # Default ordering
        verbose_name_plural = 'entries'    # Admin display name
        db_table = 'blog_entries'          # Custom table name
        unique_together = [['blog', 'slug']]  # Composite unique
        indexes = [
            models.Index(fields=['pub_date']),
        ]
```
