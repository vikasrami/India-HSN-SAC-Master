# India HSN & SAC Master

A complete, clean, ready-to-use JSON file containing **22,473** Indian GST classification codes — **21,792 HSN codes** (goods) and **681 SAC codes** (services).

## What is this?

**HSN (Harmonized System of Nomenclature)** codes classify goods for GST taxation in India. Every product sold in India needs an HSN code on its tax invoice.

**SAC (Service Accounting Code)** codes do the same for services.

This file combines both into a single, clean JSON that any application can use — no parsing, no cleanup, no HTML entities, no Excel artifacts.

## Data

**File:** `hsn_sac_master.json`

**Format:**
```json
[
  {
    "code": "87089900",
    "description": "OTHER",
    "type": "HSN",
    "gstRate": 18
  },
  {
    "code": "998719",
    "description": "Maintenance and repair services of other machinery and equipment",
    "type": "SAC",
    "gstRate": 18
  }
]
```

**Fields:**

| Field | Type | Description |
|---|---|---|
| `code` | string | HSN (2-8 digits) or SAC code (4-6 digits) |
| `description` | string | Official description from CBIC |
| `type` | string | `"HSN"` for goods, `"SAC"` for services |
| `gstRate` | number | IGST rate in % (0, 5, 12, 18, 28). CGST = SGST = gstRate / 2 |

**Stats:**

| Metric | Value |
|---|---|
| Total codes | 22,473 |
| HSN codes (goods) | 21,792 |
| SAC codes (services) | 681 |
| 8-digit granular HSN | 14,564 |
| 6-digit HSN | 6,422 |
| 4-digit HSN headings | 1,379 |
| File size | ~3.1 MB |

## Usage

### JavaScript / Node.js
```javascript
const codes = require('./hsn_sac_master.json');

// Find by code
const bearing = codes.find(c => c.code === '84821010');

// Search by description
const results = codes.filter(c => 
  c.description.toLowerCase().includes('bearing')
);

// Get all SAC codes
const sacCodes = codes.filter(c => c.type === 'SAC');

// Get GST rate for a code
const rate = codes.find(c => c.code === '87089900')?.gstRate; // 18
```

### Python
```python
import json

with open('hsn_sac_master.json') as f:
    codes = json.load(f)

# Search
bearings = [c for c in codes if 'bearing' in c['description'].lower()]
```

### SQL (PostgreSQL)
```sql
-- Import into a table
CREATE TABLE hsn_sac_codes (
    code VARCHAR(8) PRIMARY KEY,
    description TEXT,
    type VARCHAR(3),
    gst_rate DECIMAL(5,2)
);

-- Load using your preferred method (COPY, pg_read_file, application insert)
```

## Data Sources

- **HSN codes:** CBIC (Central Board of Indirect Taxes and Customs) HSN Master
- **SAC codes:** CBIC SAC Master
- **GST rates:** As per GST Council notifications effective 2025-26
- **Cleaned:** HTML entities, Excel artifacts (`_x000D_`), smart quotes, and non-breaking spaces removed

## Important Notes

- **Rates are base rates.** Some codes have conditional rates depending on end-use, value thresholds, or product specifications. For example, apparel below ₹1,000 is taxed at 5% while above ₹1,000 is 12%. Always verify with a tax professional for edge cases.
- **Cess is not included.** Compensation cess (on luxury goods, tobacco, aerated drinks) is a separate levy not reflected in the `gstRate` field.
- **This is not legal advice.** Use this data as a reference. For compliance, verify codes and rates against the latest CBIC notifications at [cbic.gov.in](https://www.cbic.gov.in).

## Updates

HSN/SAC rates change via CBIC gazette notifications, typically 1-2 times per year. This file will be updated when new notifications are issued.

| Date | Notification | Changes |
|---|---|---|
| Sep 2025 | Notification 9/2025-CT(Rate) — GST 2.0 | Rate restructuring across 7 schedules |

## Contributing

Found an incorrect code, missing description, or wrong rate? Open an issue or submit a PR with:
- The HSN/SAC code
- The correct value
- The CBIC notification reference

## License

MIT — use freely in any project.

## Maintained by

[NexDrive](https://github.com/nexdrive) — Building ERP infrastructure for Indian MSMEs.
