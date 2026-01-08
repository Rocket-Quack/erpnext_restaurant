# ERPNext Restaurant - Feature Options and Workflows

This document explains how Restaurant Settings toggles affect the POS flow. It also includes example workflows.

## Feature Toggles (Effect Summary)
| Toggle | Effect | Typical Use |
| --- | --- | --- |
| use_tables | Enables table/session workflow | Restaurants with seating |
| use_floor_plans | Shows floor plans and table layout | Visual table management |
| use_reservations | Enables reservation flow | Booking management |
| require_table_session | Blocks billing without session | Strict table flow |
| enable_quick_pay | Allows bills without session | Bar/club quick pay |
| enable_split_bills | Multiple bills per session | Split checks |
| enable_discounts | Percent discounts on bills | Employee/loyalty discounts |
| enable_vouchers | Voucher sale and redemption | Gift cards |
| enable_loyalty | Loyalty card earn/redeem | Points programs |
| enable_tips | Tip capture on payment | Service tips |
| enable_container_tracking | Consumption tracking by container | Cocktails/ingredients |
| require_active_shift | Blocks orders if no shift | Cash control |
| require_pos_profile | Blocks POS access without profile | Per-user defaults |

## Access and Shift Gates
```mermaid
flowchart TD
    A[POS Start] --> B{require_pos_profile}
    B -- no --> D{require_active_shift}
    B -- yes --> C{User has POS profile?}
    C -- no --> X[Block POS Access]
    C -- yes --> D
    D -- no --> E[Start Service]
    D -- yes --> F{Active shift exists?}
    F -- no --> Y[Block Ordering]
    F -- yes --> E
```

## Service Flow Selection
```mermaid
flowchart TD
    A[Start Service] --> B{use_tables}
    B -- yes --> C[Select Area/Floor Plan]
    C --> D{use_reservations}
    D -- yes --> E[Seat Reservation -> Table Session]
    D -- no --> F[Open Table Session]
    E --> G[Create Order]
    F --> G
    B -- no --> H{enable_quick_pay}
    H -- yes --> I[Quick Pay Bill]
    H -- no --> J[Order-only or external flow]
```

## Billing Add-ons (Discounts, Vouchers, Loyalty, Tips)
```mermaid
flowchart TD
    A[Order Items] --> B{enable_split_bills}
    B -- yes --> C[Create Multiple Bills]
    B -- no --> D[Single Bill]
    C --> E[Payment]
    D --> E
    E --> F{enable_discounts}
    F -- yes --> F1[Apply Discount]
    F -- no --> G
    F1 --> G{enable_vouchers}
    G -- yes --> G1[Apply Voucher]
    G -- no --> H{enable_loyalty}
    G1 --> H
    H -- yes --> H1[Earn/Redeem Points]
    H -- no --> I{enable_tips}
    H1 --> I
    I -- yes --> I1[Add Tip]
    I -- no --> J[Finalize Bill]
    I1 --> J
    J --> K{enable_container_tracking}
    K -- yes --> L[Create Ingredient Consumption]
    K -- no --> M[Close Bill]
```

## Example Workflows

### Example 1: Dine-In with Reservation and Split Bills
```mermaid
flowchart LR
    A[Reservation] --> B[Seat Guest -> Table Session]
    B --> C[Order Items]
    C --> D[Split Bills]
    D --> E[Apply Voucher/Discount]
    E --> F[Payment + Tips]
    F --> G[Set Cleaning]
    G --> H[Cleaning Done -> Table Available]
```

### Example 2: Takeaway Quick Pay
```mermaid
flowchart LR
    A[Walk-in Takeaway] --> B[Quick Pay Bill]
    B --> C[Order Items]
    C --> D[Apply Discount or Voucher, optional]
    D --> E[Payment]
```

### Example 3: Combo + Modifiers + Consumption
```mermaid
flowchart LR
    A[Select Combo Item] --> B[Pick Combo Components]
    B --> C[Add Modifiers, price deltas]
    C --> D[Order Item Saved]
    D --> E[Bill Paid]
    E --> F[Consumption from Recipes]
```

### Example 4: Bar Service with Staff Login and Shift
```mermaid
flowchart LR
    A[Staff Login] --> B{POS profile required}
    B -- yes --> C[Load POS Profile Defaults]
    B -- no --> D[Use Settings Defaults]
    C --> E{Active Shift Required}
    D --> E
    E -- yes --> F{Active Shift Exists}
    F -- no --> G[Open Shift]
    F -- yes --> H[Start Service]
    G --> H
    H --> I[Quick Pay Bill]
    I --> J[Apply Discount, Voucher, Tip optional]
    J --> K[Payment]
```

### Example 5: Disco Entry Payment, No Tables
```mermaid
flowchart LR
    A[Staff Login] --> B[Start Service]
    B --> C{use_tables}
    C -- no --> D{enable_quick_pay}
    D -- yes --> E[Create Entry Bill]
    E --> F[Payment]
    F --> G[Optional Loyalty Earn]
```

### Example 6: Table Service with Multiple Staff and Split Bills
```mermaid
flowchart LR
    A[Guest Seated] --> B[Open Table Session]
    B --> C[Waiter 1 Order Drinks]
    C --> D[Waiter 2 Order Food]
    D --> E[Create Split Bills]
    E --> F[Cashier Settles Bill 1]
    F --> G[Cashier Settles Bill 2]
    G --> H[Set Cleaning]
    H --> I[Cleaning Done]
```

### Example 7: Reservation to Table, Then Cleaning
```mermaid
flowchart LR
    A[Create Reservation] --> B[Confirm Reservation]
    B --> C[Seat Guest]
    C --> D[Open Table Session]
    D --> E[Order Items]
    E --> F[Payment]
    F --> G[Set Cleaning]
    G --> H[Cleaning Done]
```

## Notes
- Combos use a single combo price; components are tracked for reporting and consumption.
- Modifier options are price deltas only.
- Consumption is recorded on payment to allow order cancellations before prep.
