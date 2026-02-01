---
name: build_ebay_pro_debut_urls
description: Generate eBay search URLs for baseball card collectors searching for Topps Pro Debut cards. Creates multiple search variants (regular, premium with refractors/parallels, lot bundles, MILB) with optional seller filtering. Outputs both markdown links and raw JSON. Use when users need eBay URLs for player card searches.
---

# eBay Pro Debut URL Builder

Generate optimized eBay search URLs for Topps Pro Debut baseball cards with multiple search variants.

## When to Use This Skill

Trigger this skill when users request:
- eBay search links for baseball players
- Topps Pro Debut card searches
- Multiple search variants (regular, premium, lots)
- Seller-specific card searches
- Batch URL generation for player lists

## Input Schema

```javascript
{
  players: string[],              // Required: one or more player names
  year: number,                   // Required: e.g. 2025
  set_name: string,               // Required: e.g. "Topps Pro Debut"
  include_bin: boolean,           // Optional: default true (Buy It Now only)
  sort_newest: boolean,           // Optional: default true (newest first)
  seller_id: string | null,       // Optional: filter by seller username
  premium_keywords: string,       // Optional: default "(refractor,gold,black,orange,red,purple,green,SP,SSP,)"
  exclude_terms: string[],        // Optional: default ["digital", "mini"]
  variants: string[]              // Optional: default ["regular", "premium", "lot"]
                                  // Allowed: "regular" | "premium" | "lot" | "milb"
}
```

## URL Construction Rules

### Base URL Template
```
https://www.ebay.com/sch/i.html?_from=R40&_nkw={QUERY}&LH_BIN=1&_sop=15
```

### Query Parameters
- `_from=R40` - Standard eBay search
- `_nkw={QUERY}` - Search keywords (URL encoded)
- `LH_BIN=1` - Buy It Now only (if include_bin=true)
- `_sop=15` - Sort by newest (if sort_newest=true)
- `_ssn={SELLER_ID}` - Filter by seller (if seller_id provided)

### Search Variants

**Regular Query:**
```
"{Player} {Year} {SetName}"
Example: "Jasson Dominguez 2025 Topps Pro Debut"
```

**Premium Query:**
```
"{Player} {Year} {SetName} {premium_keywords} -digital -mini"
Example: "Jasson Dominguez 2025 Topps Pro Debut (refractor,gold,black,orange,red,purple,green,SP,SSP,) -digital -mini"
```
Note: Each exclude_term is prefixed with "-"

**Lot Query:**
```
"{Player} {Year} {SetName} lot"
Example: "Jasson Dominguez 2025 Topps Pro Debut lot"
```

**MILB Query (optional):**
```
"{Player} {Year} {SetName} MILB"
Example: "Jasson Dominguez 2025 Topps Pro Debut MILB"
```

### URL Encoding Rules
- Replace spaces with `+`
- Keep parentheses and commas (eBay accepts them)
- Prefix excluded words with `-`
- Encode special characters as needed

## Output Format

### Markdown Format (Default)
For each player, generate a bulleted list with markdown links:

```markdown
**Player Name**
- [Regular Search](url)
- [Premium Search (Refractors/Parallels)](url)
- [Lot Search](url)
- [MILB Search](url) (if requested)
```

### JSON Format (Optional)
```json
[
  {
    "player": "Player Name",
    "regular_url": "https://...",
    "premium_url": "https://...",
    "lot_url": "https://...",
    "milb_url": "https://..." // optional
  }
]
```

## Implementation Guide

1. **Parse inputs** - Extract player names, year, set name, and options
2. **Build base URL** - Construct URL with BIN and sort parameters
3. **Add seller filter** - Append `&_ssn={seller_id}` if provided
4. **Generate queries** - Build each variant's search query
5. **Encode URLs** - Replace spaces with `+`, handle special chars
6. **Format output** - Present as markdown links or JSON

## Example Usage

**Input:**
```javascript
{
  players: ["Jasson Dominguez", "Jackson Holliday"],
  year: 2025,
  set_name: "Topps Pro Debut",
  variants: ["regular", "premium", "lot"]
}
```

**Output:**
```markdown
**Jasson Dominguez**
- [Regular Search](https://www.ebay.com/sch/i.html?_from=R40&_nkw=Jasson+Dominguez+2025+Topps+Pro+Debut&LH_BIN=1&_sop=15)
- [Premium Search (Refractors/Parallels)](https://www.ebay.com/sch/i.html?_from=R40&_nkw=Jasson+Dominguez+2025+Topps+Pro+Debut+(refractor,gold,black,orange,red,purple,green,SP,SSP,)+-digital+-mini&LH_BIN=1&_sop=15)
- [Lot Search](https://www.ebay.com/sch/i.html?_from=R40&_nkw=Jasson+Dominguez+2025+Topps+Pro+Debut+lot&LH_BIN=1&_sop=15)

**Jackson Holliday**
- [Regular Search](https://www.ebay.com/sch/i.html?_from=R40&_nkw=Jackson+Holliday+2025+Topps+Pro+Debut&LH_BIN=1&_sop=15)
- [Premium Search (Refractors/Parallels)](https://www.ebay.com/sch/i.html?_from=R40&_nkw=Jackson+Holliday+2025+Topps+Pro+Debut+(refractor,gold,black,orange,red,purple,green,SP,SSP,)+-digital+-mini&LH_BIN=1&_sop=15)
- [Lot Search](https://www.ebay.com/sch/i.html?_from=R40&_nkw=Jackson+Holliday+2025+Topps+Pro+Debut+lot&LH_BIN=1&_sop=15)
```

## Notes

- URLs are automatically configured for Buy It Now and newest first sorting
- Premium searches exclude digital cards and minis by default
- Seller filtering helps find cards from trusted sellers
- MILB variant useful for minor league specific searches
- Keep premium_keywords concise to avoid eBay query length limits
