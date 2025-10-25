
---

## Tables included (from your model)
- `shipments` — Amount, Boxes, Cost, PID (product id), GID (geo id), Shipdate, Order_Status, Shipment Count (if precomputed)
- `products` — PID, Product, Category, Cost_per_box, other product attributes
- `people` — SPID (salesperson id), FirstName (Sales_person), Picture, Team
- `locations` — GID, Geo (country), Region
- `calendar` — cal_date, month_name, month_num, start_of_month, weekday_name, weekday_num (used for time-intel)

---

## Key DAX measures (examples)

```dax
-- Basic sums
Total Amount = SUM(shipments[Amount])
Total Boxes  = SUM(shipments[Boxes])

-- Profit (absolute) (if you have a 'Profit' column)
Total Profit = SUM(shipments[Profit])

-- Profit % (profit divided by amount; guard against divide by zero)
Profit % = 
DIVIDE( [Total Profit], [Total Amount], 0 )

-- Current Year / Past Year (example using calendar table)
Total Amount CY = 
CALCULATE( [Total Amount], SAMEPERIODLASTYEAR( calendar[cal_date] ) ) -- if you want PY, or use YEAR filters

Total Amount PY = 
CALCULATE( [Total Amount], DATEADD( calendar[cal_date], -1, YEAR ))

-- Shipment Count (distinct shipments)
Shipment Count = DISTINCTCOUNT(shipments[ShipmentID])    -- or use a pre-aggregated column

-- YoY Growth (Amount)
Amount YoY % = 
VAR curr = [Total Amount]
VAR prev = [Total Amount PY]
RETURN IF( prev = 0, BLANK(), (curr - prev) / prev )
