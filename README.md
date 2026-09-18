PwC × TUM Inventory Management Case Study — Adios

Student analysis / portfolio project.
This repository documents my analysis of the PwC × TUM inventory management case study for the fictional sports-goods retailer Adios. The current scope covers Tasks A–C: inventory transparency, root-cause analysis, and inventory optimization levers.

1. Business problem

Adios is facing cost pressure while sales are declining. Management therefore wants to reduce working capital and sees inventory as a key lever, but does not yet have sufficient transparency on where inventory is held or why excess stock exists.

The case asks three questions:

Task A — Inventory transparency: Create transparency on the supply network and inventory levels, identify where inventories are too high, calculate basic inventory KPIs, identify problematic products, and estimate inventory-reduction potential.

Task B — Reasons for high inventory: Explain why high inventory occurs, including general drivers and fashion / retail-specific drivers, describe the mechanism from cause to excess stock, and link the causes to resulting costs.

Task C — Inventory optimization levers: Translate the findings into concrete inventory-reduction measures, explain the cost-saving potential and additional cost / risk of each measure, and consider levers beyond core inventory management.

Case brief source: PwC × TUM Inventory Management case study, pp. 24–27.

2. Analytical logic

The analysis follows a three-step consulting logic:

Create transparency before recommending actions.
Build a clean Site × Product × Month dataset, map the network, calculate inventory KPIs, and quantify where the largest inventory opportunities exist.

Separate observed evidence from hypotheses.
Diagnose whether excess inventory is associated with demand decline, local allocation mismatch, or intermittent / seasonal demand. Potential supply-side or policy causes are explicitly kept as hypotheses where the provided data cannot prove them.

Convert root causes into implementable levers and quantify them without double counting.
Separate dead stock from active excess, test whether locally stranded inventory can be reused at other warehouses, quantify residual stock that may require disposition, and reconcile all opportunities into one management-level financial view.

3. Data preparation

The Excel case file contains four input sheets:

Site data

Products

Demand

Inventory

Demand and inventory are merged on:

Product Code × Site Code × Date

Product attributes and site attributes are then added to create the analytical dataset.

The notebook includes validation checks for row counts, merge uniqueness, and missing key attributes. The resulting analysis dataset contains 1,800 monthly Site × Product observations.

4. Key assumptions and definitions

The analysis uses the latest complete year, 2025, as the main inventory-performance period and compares it with 2024 where trend evidence is required.

Months of Supply (MOS)

MOS = Average Inventory / Average Monthly Demand

A 3.0-month target MOS is used as a case assumption for scenario analysis, not as a universal industry benchmark.

Inventory turns

Inventory Turns = Annual Demand / Average Inventory

Dead stock

A Site × Product position is classified as dead stock when:

2025 Annual Demand = 0
AND
December 2025 Current Inventory > 0

This is a local warehouse definition. A product can be dead stock at one site while still having demand elsewhere in the network.

Excess inventory

For products with demand:

Target Inventory = Average Monthly Demand × Target MOS

Excess Inventory = max(Average Inventory − Target Inventory, 0)

Reduction Potential (€) = Excess Inventory × Product Value

Cost of capital

A 10% WACC (Weighted Average Cost of Capital) is used as a case assumption:

Potential Annual Capital-Cost Effect
= Inventory Reduction Potential × WACC

Task A — Inventory transparency and descriptive analysis

Question

Where is inventory held, which Site × Product positions are problematic, and how much inventory-reduction potential exists?

Approach

Task A builds the baseline by:

visualizing production and warehouse locations;

reviewing Hamburg demand and inventory trends as requested in the case;

calculating 2025 Site × Product KPIs;

identifying dead stock and high-MOS positions;

estimating reduction potential under the 3-month MOS assumption;

ranking sites by modeled opportunity.

Main findings

Metric

Result

Current inventory value

€630,874

Dead-stock value

€55,622

Dead-stock share of current inventory

8.8%

Gross modeled reduction potential

€194,670

Gross reduction potential vs. average inventory value

31.5%

Gross annual capital-cost effect at 10% WACC

€19,467

Gross reduction potential by site

Rank

Site

Reduction potential

1

Hamburg

€47,960

2

Frankfurt

€43,320

3

Munich

€42,209

4

Berlin

€36,189

5

Kassel

€24,992

Hamburg is a visible example of the problem: demand fell substantially while inventory did not adjust proportionally. Product-level drill-down also reveals sharp demand deterioration for products such as Shoes_S3_Summer24 and Jacket_J3_WorldCup24.

Task A conclusion

The network contains a material inventory-reduction opportunity, but the gross €194.7k estimate mixes different types of inventory. Before recommending disposal, dead stock needs to be checked against demand elsewhere in the network and active excess must be separated from locally stranded stock.

Task B — Reasons for high inventory

Question

Why is inventory high, what mechanisms create excess stock, and what costs result?

Approach

Task B moves from symptom to root cause using four tests:

Compare 2024 → 2025 demand and average inventory value at network and site level.

Check whether locally dead SKUs still have demand at other warehouses.

Identify active Site × Product positions with intermittent or seasonal demand.

Separate data-backed causes from hypotheses that require additional operational data.

The analysis also classifies inventory-related costs as variable, fixed / step-fixed, mixed, or implementation costs.

Main findings

1. Inventory did not adjust to declining demand

From 2024 to 2025:

Network demand changed −8.2%.

Average inventory value changed +5.1%.

4 of 5 warehouses show the clearest mismatch pattern: demand declined while inventory value increased.

This is a strong signal that replenishment or inventory parameters did not respond quickly enough to demand changes. The exact planning cause cannot be proven without forecast and order data.

2. Local dead stock is partly an allocation problem

The analysis identifies:

8 local dead-stock Site × Product positions

total value: €55,622

8 / 8 of the same SKUs still had demand at other warehouses in 2025

This means the stock should not automatically be treated as obsolete at network level. The evidence supports a local allocation / cross-site rebalancing mismatch.

3. Fashion demand creates residual-stock risk

There are 5 active Site × Product positions with zero-demand months during 2025. Several show highly intermittent demand, consistent with short selling windows, seasonal products, or event-driven products.

4. Additional causes remain plausible but unproven

The case background makes the following drivers plausible:

long or volatile lead times;

high safety-stock settings;

minimum order quantities or large order batches;

broader product-location complexity.

However, lead-time history, service levels, safety-stock parameters, MOQ data, and order history are not available in the analyzed dataset, so these remain hypotheses rather than proven root causes.

Cost implications

At the 10% WACC assumption:

estimated annual financing cost of average inventory: ~€61,707

estimated annual financing cost tied to dead stock: ~€5,562

Other relevant costs include warehousing, handling, markdown / obsolescence, scrap, and transfer cost. Some of these are not immediately avoidable because warehouse rent and permanent capacity can be fixed or step-fixed.

Task B conclusion

The strongest evidence indicates that excess inventory is driven mainly by:

insufficient inventory response to falling demand;

local allocation / rebalancing mismatch;

fashion-related intermittent and seasonal demand.

Supply uncertainty and conservative inventory policies may contribute, but require additional operational data to validate.

Task C — Inventory optimization levers

Question

Which actions can reduce inventory, how much value can they address, and what costs or risks accompany the measures?

Approach

Task C deliberately avoids treating all inventory opportunity as the same type of saving.

The gross opportunity is first split into two non-overlapping buckets:

Opportunity bucket

Value

Share

Dead stock

€55,622

28.6%

Active excess inventory

€139,048

71.4%

Total gross modeled opportunity

€194,670

100%

The analysis then:

screens other warehouses as potential receivers for locally dead stock;

calculates receiving capacity relative to the 3-month inventory target;

builds a concrete source → receiver transfer plan;

separates remaining dead stock from stock that can be reused internally;

ranks active excess by product and site;

links each lever to its financial effect, implementation cost, and risk;

reconciles the results to avoid double counting.

Main findings

1. Rebalance usable dead stock before disposing of it

The receiver-screening logic identifies 9 potential source → receiver pairs.

Of the €55,622 local dead stock:

€21,604 can potentially be rebalanced internally;

€34,018 remains as residual dead stock after the modeled transfers.

The €21,604 transfer value is not counted as an immediate network inventory reduction, because the stock remains inside the company. Its benefit is potential future replenishment avoidance and lower obsolescence risk.

2. Reduce active excess inventory

Active excess inventory represents €139,048 of modeled reduction potential.

Active-excess opportunity by site:

Site

Active-excess reduction potential

Frankfurt

€37,572

Berlin

€30,693

Munich

€28,327

Hamburg

€26,390

Kassel

€16,066

The recommended measure is to reduce or temporarily pause replenishment for the highest-excess Site × Product positions until inventory approaches the modeled target, while checking demand and service requirements.

3. Dispose of residual dead stock economically

After modeled rebalancing, €34,018 of dead stock remains.

Recommended sequence:

Internal reuse → Outlet / markdown → Scrap as last resort

At 10% WACC, removing the residual dead stock corresponds to approximately €3,402 annual capital-cost effect, before considering markdown recovery or disposal costs.

4. Use intermittent-demand signals in replenishment

Five active Site × Product positions contain zero-demand months; four of them also have active excess.

The recommended policy is to explicitly use inactive periods and demand variability when setting replenishment timing instead of applying a static inventory rule.

This potential is not added separately to the financial total because it overlaps with the active-excess bucket.

5. Final quantified opportunity

After distinguishing internal rebalancing from actual network inventory reduction:

Metric

Result

Gross modeled inventory opportunity

€194,670

Stock potentially reusable through internal rebalancing

€21,604

Direct network inventory-reduction opportunity

€173,066

Potential annual capital-cost effect at 10% WACC

€17,307

The direct €173.1k opportunity consists of:

€139.0k active excess inventory

€34.0k residual dead stock

This reconciles exactly with the gross opportunity:

€21.6k internal rebalancing
+ €173.1k direct network reduction
= €194.7k gross modeled opportunity

6. Recommended action sequence

Stop additional excess build-up
Tighten or temporarily pause replenishment for the largest active-excess Site × Product positions.

Reuse locally stranded stock
Execute justified source → receiver transfers where other sites have demand and inventory below the modeled target.

Remove residual dead stock
Use outlet / markdown channels first and scrap only stock that can no longer be sold economically.

Introduce intermittent-demand replenishment rules
Use zero-demand periods, demand variability, and product lifecycle signals in replenishment decisions.

Establish recurring inventory control
Regularly review MOS, dead-stock share, active excess, and demand-versus-inventory development so that inventory does not rebuild after the initial reduction.

7. What is not quantified

The available dataset is sufficient to estimate working-capital and capital-cost effects, but not all operational economics.

The following effects are therefore deliberately left unquantified:

warehouse / storage cost reduction;

handling cost reduction;

cross-site transport cost;

markdown / outlet recovery;

scrap / disposal cost;

future replenishment avoided through rebalancing.

Quantifying these would require cost rates, shipment data, selling-price / discount data, disposal costs, and future purchase or replenishment plans.

8. Overall conclusion

The case is not simply a “too much inventory” problem.

The analysis shows a combination of:

inventory reacting too slowly to demand decline;

stock being held at locations where local demand has disappeared;

seasonal / intermittent demand creating residual-stock risk.

Under the case assumptions, the analysis identifies approximately €173k of direct network inventory-reduction opportunity, equivalent to roughly €17.3k annual capital-cost effect at a 10% WACC, while another €21.6k of locally stranded stock may be reused elsewhere in the network rather than immediately disposed of.

The main management implication is therefore to combine immediate stock reduction with better cross-site allocation and more demand-responsive replenishment, rather than using blanket inventory cuts.

9. Repository structure

.
├── pwc.ipynb
├── 202601_TUM Inventory Case Study Data.xlsx
├── 2026_2101_Inventory Case_TUM.pdf
└── outputs/
    ├── B*.csv / B*.png
    ├── C*.csv / C*.png
    └── ...

The notebook is designed to be run from either the project root or a notebooks/ folder. It resolves the project root automatically and saves generated analysis outputs to outputs/.

10. How to run

Recommended Python packages:

pip install pandas matplotlib plotly openpyxl

Then open and run:

pwc.ipynb

Run the notebook top-to-bottom so that Task B inherits the KPI tables created in Task A and Task C inherits the validated outputs from Tasks A and B.

11. Current project status

Task A — Inventory transparency and descriptive analysis

Task B — Root-cause analysis

Task C — Inventory optimization levers

Management-ready final presentation / executive storyline
