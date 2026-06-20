# Product Overview — WBMS (WeighBridge Management System)

## What It Is
A weightbridge management web app for manufacturing companies to record, track, and audit the weight of every vehicle entering and leaving the facility. Every transaction is captured digitally — replacing paper logs, manual spreadsheets, and informal records — so the company always has an accurate, timestamped weight ledger.

## Users / Personas
- **WB Operator** (Weighbridge Operator) — gate staff who perform weight entry transactions.
- **Supervisor** — has separate access for monitoring and reporting.

## Core Problem
Manufacturing facilities receive and dispatch large volumes of goods by vehicle (trucks, lorries). Without a digital system, weight entries are recorded on paper or in ad-hoc spreadsheets, leading to:
- Data loss and illegible records
- Difficulty reconciling inbound vs. outbound weights
- No real-time visibility for supervisors
- Audit and compliance risk from incomplete logs

## Core Solution
A purpose-built weightbridge app that:
1. Records every vehicle's entry and exit weight with timestamp, vehicle ID, driver, and commodity type.
2. Calculates net weight (gross minus tare) automatically.
3. Stores a complete, searchable digital log accessible to supervisors at any time.
4. Generates weight transaction reports for audit, billing, or compliance use.

## Weighbridge Transaction Logic

### Transaction Types
Every weighbridge transaction is one of two types, selected by the operator BEFORE the first weighing — because the sequence determines which reading is labelled Bruto vs. Tara:

| Type | First Weight | Second Weight | Netto |
|---|---|---|---|
| **Receiving** | Bruto (Gross) | Tara (Tare) | Bruto − Tara |
| **Dispatch / Sales** | Tara (Tare) | Bruto (Gross) | Bruto − Tara |

- The operator selects the transaction type at the start; this controls the form flow and labels.
- **Vehicle number (plate number)** must be recorded on every transaction.
- **Netto = Bruto − Tara** in all cases, regardless of type.

This logic directly governs the weight entry form: the type selection gates the first weighing step, and field labels (Bruto / Tara) are shown according to which sequence applies.

### First Weighing Form — Receiving Mode (Bruto Entry)

When the operator selects **Receiving** and proceeds to the first weighing step, the following fields are required:

1. **Vendor name** — the vendor/supplier the incoming vehicle is coming from.
2. **FDO number** — Receiving Delivery Order number; the document reference for this delivery.
3. **Vehicle plate number (No. Plat)** — the truck or lorry's registration plate.
4. **Driver name (Nama Supir)** — the name of the driver operating the vehicle.
5. **Bruto weight** — the gross weight captured from the weight indicator. The indicator must be in a **stable (not moving) state** before the value can be captured or saved; the form must enforce this and prevent saving while the indicator is still fluctuating.

**On save:** the system records that the vehicle has entered the facility and is ready for unloading (pembongkaran). This is the **first save** that creates the open transaction — a two-step record that will be completed by the second weighing (Tara) later.

### Second Weighing Form — Receiving Mode (Tara / Berat Kosong Entry)

After the vehicle has been unloaded (pembongkaran), it returns to the weighbridge for the second weighing:

1. **Tara weight (Berat Kosong)** — the weight of the empty truck/lorry captured from the weight indicator. The indicator must be stable (not moving) before saving.
2. **On save:** the system completes the transaction — Netto is auto-calculated (Bruto − Tara), the transaction record is closed.
3. **Print out:** a weight ticket / slip timbang is generated and printed automatically after save. This serves as the official proof of delivery and exit clearance.
4. **Vehicle exits:** the vehicle is cleared to leave the facility once the slip is printed.

**Complete Receiving Transaction Lifecycle:**
- Step 1: Vehicle arrives → First weighing (Bruto) → fields: Vendor, FDO, Plate, Driver, Bruto weight → Save → vehicle enters for unloading
- Step 2: Vehicle returns empty → Second weighing (Tara/Berat Kosong) → Save → Netto calculated → Print slip → Vehicle exits

## Key Features (MVP)
- Weight entry form with type selection (Receiving or Dispatch / Sales) before first weighing; field labels adapt to sequence
- Vehicle plate number recorded on every transaction
- Automatic Netto calculation (Bruto − Tara) regardless of transaction type
- Vehicle & driver registration / quick-select
- Timestamp and operator ID captured per transaction
- Transaction history log (searchable, filterable by type, date, vehicle)
- Weight ticket / slip timbang print (auto-generated on second weighing save; serves as proof of delivery and exit clearance)
- Daily / date-range report export
- Dashboard summary (total vehicles today, total tonnage, pending transactions)

## Positioning
A straightforward, reliable operational tool built for the factory floor and the manager's desk. Not a complex ERP — a focused, fast weightbridge log that just works.

## Brand & Tone
- Name: **WeighBridge Manager** (working title — to be confirmed)
- Tone: Professional, precise, industrial-operational. Clear labels, no jargon, built for non-technical gate staff.
- Color direction: industrial/industrial-clean — deep slate or navy primary, amber or orange accent for alert/action states.

## Scope & Constraints
- Web app (responsive, tablet-friendly for gate operators)
- Single facility (multi-site can be a future phase)
- No integration with external ERP in MVP
- Data stored and accessible to authenticated company users only

## Strategic Principles
- Every transaction must be immutable once saved (operator can flag for review, supervisor approves correction)
- Speed of entry matters — operators need to complete a weight transaction in under 60 seconds
- Supervisors must be able to pull any day's log without operator involvement
