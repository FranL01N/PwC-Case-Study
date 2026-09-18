# PwC × TUM Inventory Management Case Study — Adios

> **Student analysis / portfolio project**  
> This repository documents my analysis of the PwC × TUM Inventory Management Case Study for the fictional sports-goods retailer **Adios**.  
>
> The current scope covers **Tasks A–C: inventory transparency, root-cause analysis, and inventory optimization levers**.

---

## 1. Business Problem

Adios is facing increasing cost pressure while sales are declining. Management therefore wants to reduce working capital and sees inventory as a key lever.

However, the company currently lacks sufficient transparency on:

- where inventory is held,
- which locations and products carry excessive stock,
- why excess inventory has accumulated,
- and which actions could reduce inventory efficiently.

The case therefore asks three main questions:

### Task A — Inventory Transparency

Create transparency on the supply network and inventory levels, identify where inventories are too high, calculate basic inventory KPIs, identify problematic products, and estimate inventory-reduction potential.

### Task B — Reasons for High Inventory

Explain why high inventory occurs, including general drivers and fashion / retail-specific drivers, describe the mechanism from cause to excess stock, and link those causes to resulting costs.

### Task C — Inventory Optimization Levers

Translate the findings into concrete inventory-reduction measures, explain potential cost savings and additional costs / risks, and consider levers beyond core inventory management.

---

## 2. Analytical Logic

The analysis follows a three-step consulting logic:

### 1. Create transparency before recommending actions

Build a clean **Site × Product × Month** dataset, map the supply network, calculate inventory KPIs, and quantify where the largest inventory opportunities exist.

### 2. Separate observed evidence from hypotheses

Investigate whether excess inventory is associated with:

- demand decline,
- local allocation mismatch,
- intermittent / seasonal demand,
- or other possible supply-side and inventory-policy drivers.

Potential causes that cannot be proven with the available dataset are explicitly treated as **hypotheses rather than confirmed root causes**.

### 3. Convert root causes into implementable levers

Separate:

- locally stranded dead stock,
- active excess inventory,
- and stock that could potentially be rebalanced across warehouses.

The optimization measures are then reconciled into one management-level financial view to avoid double counting.

---

## 3. Data Preparation

The Excel case file contains four main input sheets:

- `Site data`
- `Products`
- `Demand`
- `Inventory`

Demand and inventory are merged using:

```text
Product Code × Site Code × Date
```

Product attributes and site information are subsequently added to create the analytical dataset.

The notebook also performs validation checks for:

- row counts,
- merge uniqueness,
- missing key attributes,
- and key consistency.

The resulting analysis dataset contains:

**1,800 monthly Site × Product observations**

---

## 4. Key Assumptions and Definitions

The main inventory-performance analysis uses the latest complete year:

**Analysis year: 2025**

Where trend evidence is required, 2025 is compared with 2024.

---

### Months of Supply (MOS)

**MOS = Average Inventory / Average Monthly Demand**

A target of:

**3.0 months of supply**

is used as a **case assumption for scenario analysis**.

It is not intended as a universal industry benchmark.

---

### Inventory Turns

**Inventory Turns = Annual Demand / Average Inventory**

---

### Dead Stock

A Site × Product position is classified as dead stock when:

```text
2025 Annual Demand = 0
AND
December 2025 Current Inventory > 0
```

This is deliberately defined at the **local warehouse level**.

Therefore, a product may be dead stock at one warehouse while still having demand elsewhere in the network.

---

### Excess Inventory

For products with positive demand:

```text
Target Inventory
= Average Monthly Demand × Target MOS
```

```text
Excess Inventory
= max(Average Inventory − Target Inventory, 0)
```

```text
Reduction Potential (€)
= Excess Inventory × Product Value
```

---

### Weighted Average Cost of Capital (WACC)

A **10% WACC** is used as a case assumption.

```text
Potential Annual Capital-Cost Effect
= Inventory Reduction Potential × WACC
```

---

# Task A — Inventory Transparency and Descriptive Analysis

## Question

**Where is inventory held, which Site × Product positions are problematic, and how much inventory-reduction potential exists?**

---

## Approach

Task A establishes the analytical baseline by:

1. Visualizing production and warehouse locations.
2. Reviewing Hamburg demand and inventory trends.
3. Performing product-level drill-downs.
4. Calculating 2025 Site × Product inventory KPIs.
5. Identifying dead stock and high-MOS positions.
6. Estimating inventory-reduction potential under the 3-month MOS assumption.
7. Ranking warehouses by modeled opportunity.

---

## Main Findings

| Metric | Result |
|---|---:|
| Current inventory value | **€630,874** |
| Dead-stock value | **€55,622** |
| Dead-stock share of current inventory | **8.8%** |
| Gross modeled reduction potential | **€194,670** |
| Gross reduction potential vs. average inventory value | **31.5%** |
| Gross annual capital-cost effect at 10% WACC | **€19,467** |

---

### Gross Reduction Potential by Site

| Rank | Site | Reduction Potential |
|---:|---|---:|
| 1 | Hamburg | **€47,960** |
| 2 | Frankfurt | **€43,320** |
| 3 | Munich | **€42,209** |
| 4 | Berlin | **€36,189** |
| 5 | Kassel | **€24,992** |

---

## Observed Pattern

Hamburg provides a visible example of the overall inventory problem:

- demand declined,
- while inventory did not adjust proportionally.

Product-level drill-down also reveals sharp demand deterioration for products such as:

- `Shoes_S3_Summer24`
- `Jacket_J3_WorldCup24`

This suggests that inventory is reacting too slowly to changes in actual demand.

---

## Task A Conclusion

The network contains a material inventory-reduction opportunity.

However, the initial **€194.7k gross opportunity** combines different types of stock.

Before recommending disposal or reduction, it is necessary to distinguish between:

- active excess inventory,
- locally dead stock,
- and inventory that may still be useful elsewhere in the network.

This becomes the focus of Tasks B and C.

---

# Task B — Reasons for High Inventory

## Question

**Why is inventory high, what mechanisms create excess stock, and what costs result?**

---

## Approach

Task B moves from **symptom → root cause** using four analytical tests.

### Test 1 — Demand vs. Inventory Development

Compare 2024 → 2025:

- annual demand,
- average inventory value,
- and warehouse-level developments.

### Test 2 — Local Dead Stock vs. Network Demand

Check whether SKUs classified as dead stock at one warehouse still have demand at other warehouses.

### Test 3 — Intermittent / Seasonal Demand

Identify active Site × Product positions with zero-demand months or highly uneven demand patterns.

### Test 4 — Evidence vs. Hypothesis

Separate causes directly supported by the dataset from operational causes that would require additional data.

The analysis also distinguishes between:

- variable costs,
- fixed / step-fixed costs,
- mixed costs,
- and implementation costs.

---

## Main Finding 1 — Inventory Did Not Adjust to Declining Demand

From 2024 to 2025:

| Metric | Change |
|---|---:|
| Network demand | **−8.2%** |
| Average inventory value | **+5.1%** |

Furthermore:

**4 of 5 warehouses**

show the clearest mismatch pattern:

> demand declined while inventory value increased.

This provides strong evidence that inventory levels did not respond sufficiently to changing demand.

The exact operational reason cannot be proven without forecast, replenishment, and order-history data.

---

## Main Finding 2 — Local Dead Stock Is Partly an Allocation Problem

The analysis identifies:

**8 local dead-stock Site × Product positions**

with a combined value of:

**€55,622**

However:

**8 / 8 of these SKUs still had demand at other warehouses in 2025.**

This is an important distinction.

The products are not necessarily obsolete at the **network level**.

Instead, the evidence suggests a:

**local allocation / cross-site rebalancing mismatch**

where inventory remains at locations without demand while the same product is still demanded elsewhere.

---

## Main Finding 3 — Fashion Demand Creates Residual-Stock Risk

The analysis identifies:

**5 active Site × Product positions**

with at least one zero-demand month during 2025.

Several show strongly intermittent demand patterns.

This is consistent with the characteristics of fashion and sporting-goods retail, where products may have:

- short selling windows,
- seasonal demand,
- event-driven demand,
- and rapid lifecycle changes.

Static replenishment policies can therefore create residual inventory when demand disappears faster than inventory policies adjust.

---

## Main Finding 4 — Additional Causes Are Plausible but Not Proven

Several additional inventory drivers are plausible:

- long or volatile lead times,
- excessive safety-stock settings,
- minimum order quantities,
- large order batches,
- product-location complexity,
- and conservative service-level policies.

However, the available dataset does not contain:

- lead-time history,
- service-level targets,
- safety-stock parameters,
- MOQ data,
- supplier information,
- or detailed order history.

These factors are therefore treated as **hypotheses rather than confirmed causes**.

---

## Cost Implications

Using the 10% WACC assumption:

| Cost Indicator | Estimated Value |
|---|---:|
| Annual financing cost of average inventory | **~€61,707** |
| Annual financing cost associated with dead stock | **~€5,562** |

Additional relevant inventory costs include:

- warehousing,
- handling,
- markdowns,
- obsolescence,
- scrapping,
- and internal transfers.

Not all of these costs are immediately avoidable.

For example, warehouse rent and permanent warehouse capacity may behave as **fixed or step-fixed costs** rather than fully variable costs.

---

## Task B Conclusion

The strongest evidence indicates three main drivers of excessive inventory:

### 1. Insufficient inventory response to declining demand

Inventory remained high even when demand declined.

### 2. Local allocation / rebalancing mismatch

Products with no demand at one location may still be needed elsewhere.

### 3. Fashion-related intermittent and seasonal demand

Short product lifecycles and uneven demand increase the risk of residual inventory.

Supply uncertainty and conservative inventory policies may also contribute, but additional operational data would be required to verify them.

---

# Task C — Inventory Optimization Levers

## Question

**Which actions can reduce inventory, how much value can they address, and what costs or risks accompany those measures?**

---

## Approach

Task C deliberately avoids treating every inventory opportunity as the same type of saving.

The gross opportunity is first separated into two non-overlapping inventory categories.

| Opportunity Bucket | Value | Share |
|---|---:|---:|
| Dead stock | **€55,622** | **28.6%** |
| Active excess inventory | **€139,048** | **71.4%** |
| **Total gross modeled opportunity** | **€194,670** | **100%** |

The analysis then:

1. Screens other warehouses as potential receivers for locally dead stock.
2. Calculates receiving capacity relative to the 3-month inventory target.
3. Builds a concrete source → receiver transfer plan.
4. Separates reusable stock from residual dead stock.
5. Ranks active excess by product and site.
6. Links each lever to financial effects, implementation costs, and risks.
7. Reconciles all measures to avoid double counting.

---

## Lever 1 — Rebalance Usable Dead Stock Before Disposing of It

The receiver-screening logic identifies:

**9 potential source → receiver pairs**

for locally dead stock.

Of the total:

**€55,622 local dead-stock value**

approximately:

**€21,604**

can potentially be rebalanced internally.

This leaves:

**€34,018**

as residual dead stock after the modeled transfers.

---

### Important Interpretation

The **€21,604 transferred inventory is not counted as an immediate network inventory reduction**.

The inventory still exists within Adios after the transfer.

Its potential benefits instead include:

- avoiding future replenishment at the receiving warehouse,
- reducing future purchasing requirements,
- decreasing obsolescence risk,
- and improving inventory utilization.

---

## Lever 2 — Reduce Active Excess Inventory

Active excess inventory represents:

**€139,048**

of modeled direct reduction potential.

### Active-Excess Opportunity by Site

| Site | Active-Excess Reduction Potential |
|---|---:|
| Frankfurt | **€37,572** |
| Berlin | **€30,693** |
| Munich | **€28,327** |
| Hamburg | **€26,390** |
| Kassel | **€16,066** |

The recommended action is to:

- reduce replenishment,
- temporarily pause replenishment,
- or lower future replenishment quantities

for the largest excess Site × Product positions until inventory approaches the modeled target.

Any implementation should still consider:

- expected future demand,
- product lifecycle,
- and required service levels.

---

## Lever 3 — Dispose of Residual Dead Stock Economically

After modeled internal rebalancing:

**€34,018**

of residual dead stock remains.

The recommended disposition sequence is:

```text
Internal reuse
      ↓
Outlet / markdown
      ↓
Scrap as last resort
```

At a 10% WACC, eliminating this residual inventory corresponds to an estimated annual capital-cost effect of:

**~€3,402**

before considering:

- markdown revenue,
- disposal costs,
- or scrap recovery value.

---

## Lever 4 — Use Intermittent-Demand Signals in Replenishment

The analysis identifies:

**5 active Site × Product positions**

with zero-demand months.

Of these:

**4 also have active excess inventory.**

This suggests that replenishment logic should explicitly consider:

- inactive demand periods,
- demand variability,
- product seasonality,
- and product lifecycle signals.

This lever is **not added separately to the quantified financial opportunity**, because its potential overlaps with the active-excess bucket.

---

# 5. Final Quantified Opportunity

After distinguishing internal rebalancing from actual network inventory reduction:

| Metric | Result |
|---|---:|
| Gross modeled inventory opportunity | **€194,670** |
| Stock potentially reusable through internal rebalancing | **€21,604** |
| Direct network inventory-reduction opportunity | **€173,066** |
| Potential annual capital-cost effect at 10% WACC | **€17,307** |

The direct **€173.1k inventory-reduction opportunity** consists of:

```text
€139.0k Active Excess Inventory
+
€34.0k Residual Dead Stock
=
€173.1k Direct Network Inventory Reduction
```

The entire opportunity therefore reconciles as:

```text
€21.6k Internal Rebalancing
+
€173.1k Direct Network Reduction
=
€194.7k Gross Modeled Opportunity
```

This separation prevents internal transfers from being incorrectly counted as immediate inventory savings.

---

# 6. Recommended Action Sequence

## Priority 1 — Stop Additional Excess Build-Up

Tighten or temporarily pause replenishment for the largest active-excess Site × Product positions.

---

## Priority 2 — Reuse Locally Stranded Stock

Execute justified source → receiver transfers when:

- another warehouse has actual demand,
- the receiving site is below its modeled inventory target,
- and transfer economics are reasonable.

---

## Priority 3 — Remove Residual Dead Stock

Use:

1. outlet channels,
2. markdowns,
3. targeted promotions,

before considering scrapping.

---

## Priority 4 — Introduce Intermittent-Demand Replenishment Rules

Use:

- zero-demand periods,
- demand variability,
- seasonality,
- and product lifecycle signals

when deciding replenishment timing and quantities.

---

## Priority 5 — Establish Recurring Inventory Control

Management should continuously monitor:

- Months of Supply (MOS),
- dead-stock share,
- active excess inventory,
- inventory turns,
- and demand-versus-inventory development.

The objective is not only to reduce current inventory, but also to prevent inventory from rebuilding after the initial reduction.

---

# 7. What Is Not Quantified

The available dataset is sufficient to estimate:

- inventory value,
- working-capital reduction,
- and capital-cost effects.

However, several operational effects cannot be quantified reliably with the available information.

These include:

- warehouse / storage cost reduction,
- handling cost reduction,
- cross-site transportation cost,
- markdown / outlet recovery,
- scrap / disposal cost,
- and future replenishment avoided through rebalancing.

Quantifying these effects would require additional information such as:

- warehouse cost rates,
- shipment and transportation data,
- selling prices,
- discount levels,
- disposal costs,
- and future purchase / replenishment plans.

---

# 8. Overall Conclusion

The case is not simply a **"too much inventory"** problem.

The analysis identifies a combination of three underlying issues:

1. **Inventory reacts too slowly to declining demand.**
2. **Stock remains at locations where local demand has disappeared.**
3. **Seasonal and intermittent demand creates residual-stock risk.**

Under the case assumptions, the analysis identifies approximately:

**€173k of direct network inventory-reduction opportunity**

corresponding to approximately:

**€17.3k annual capital-cost effect at a 10% WACC**

while another:

**€21.6k of locally stranded inventory**

may potentially be reused elsewhere in the network instead of immediately being disposed of.

The main management implication is therefore to combine:

**immediate inventory reduction**

with:

**better cross-site allocation**

and:

**more demand-responsive replenishment**

rather than applying a uniform inventory cut across the network.

---

# 9. Repository Structure

```text
.
├── pwc.ipynb
├── 202601_TUM Inventory Case Study Data.xlsx
├── 2026_2101_Inventory Case_TUM.pdf
└── outputs/
    ├── B*.csv
    ├── B*.png
    ├── C*.csv
    ├── C*.png
    └── ...
```

The notebook automatically resolves the project root and saves generated analytical outputs to:

```text
outputs/
```

---

# 10. How to Run

Recommended Python packages:

```bash
pip install pandas matplotlib plotly openpyxl
```

Then open:

```text
pwc.ipynb
```

and run the notebook:

```text
top → bottom
```

Task B uses KPI tables generated in Task A, while Task C builds on the validated outputs from Tasks A and B.

---

# 11. Current Project Status

- [x] Task A — Inventory transparency and descriptive analysis
- [x] Task B — Root-cause analysis
- [x] Task C — Inventory optimization levers
- [ ] Task D — Management presentation / executive storyline

---

## Disclaimer

This repository is a student case-study analysis based on materials provided for the **PwC × TUM Inventory Management Case Study**.

All quantitative optimization results depend on the assumptions described above, particularly the **3-month MOS target** and **10% WACC assumption**, and should therefore be interpreted as scenario-based analytical estimates rather than universal inventory benchmarks.
