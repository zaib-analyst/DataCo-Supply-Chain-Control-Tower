🚚 DataCo Supply Chain Control Tower | Executive Power BI Dashboard

An executive-grade, high-performance Power BI dashboard designed to monitor global freight logistics, regional fulfillment delays, product profitability, and order-level telemetry in real time. Built using the DataCo Supply Chain Dataset, this solution equips supply chain leaders with actionable insights across three core analytical pages and a high-density drill-through utility view.

📸 Dashboard Preview

1. Executive Overview & Order Telemetry

2. Regional Logistics & Delivery Performance

3. Product Analytics & Commercial Intelligence

🎨 Theme & UI Architecture

The dashboard is built on a custom 10-color dark palette designed to reduce visual fatigue, ensure high contrast, and establish immediate status recognition across all pages.

Canvas Background: #020B24 (Deep Navy)

Visual Card Containers: #061536 (Slate Blue Tint)

Primary Accent / Metrics: #0066FF (Electric Blue)

Operational Speed / Time: #19E6FF (Cyan Accent)

Success / Healthy Health: #00C853 (Neon Forest Green)

Warning / Variance Caution: #FFC400 (Amber)

Critical Risk / Late Shipments: #FF3B4D (Danger Red)

🔑 Key Features & Architecture

📊 Page 1: Executive Overview & Order Telemetry

Time-Intelligence Combo Chart: Evaluates quarterly revenue performance (Total Sales) against period-over-period growth using a continuous Quarter-Year axis and line-over-bar rendering (QoQ Growth Trend %).

Selected Order Telemetry Panel: An interactive inspection panel displaying real vs. scheduled transit times, delay variance (days), order status, and immediate cross-filtering feedback.

Delivery Details Grid: A granular table equipped with conditional formatting for On Time, Advance Shipping, and Late Delivery status flags.

Global Reset Bookmark: A single-click Clear All Filters button that resets all page slicers (Select Year, Select Region, Select Shipping Mode) back to default.

Cross-Page Drillthrough: Context-aware View Details action button routing selected order telemetry to the underlying utility table.

🌎 Page 2: Regional Logistics & Delivery Performance

Fulfillment Key Metrics: Tracks On-Time In-Full (OTIF %), Late Delivery Risk Rate %, Avg Order Processing Time (Days), and Total Orders.

Geographic Fulfillment Analysis: Clustered bar chart identifying regional delay bottlenecks and shipping corridor performance.

Shipping Mode Matrix: 100% stacked breakdown evaluating delivery success rates across Standard Class, First Class, Second Class, and Same Day modes.

Regional Matrix Table: Detailed cross-analysis of regional volume, delay variances, and profit margins.

📦 Page 3: Product Analytics & Commercial Intelligence

Category Margin Comparison: Combo chart tracking Top 10 Product Categories by sales volume alongside gross margin percentages.

Loss Mitigation: Dedicated horizontal ranking chart identifying the Top 5 Loss-Making Products.

Profit Decomposition Tree: Root-cause analysis visual breaking down profit variance across customer segments and product departments.

Order Risk & Cancellation Matrix: Tracks product cancellations and late risk rates to isolate problematic SKUs.

🔍 Page 4: Order Details (Drillthrough Utility)

Low-latency, high-density table view passing explicit Order Id context from Page 1 to inspect item-level quantities, margins, shipping schedules, and customer details. Includes a standard navigation return button.

📐 Data Modeling & DAX Engine

Star Schema Optimization

Built on DataCoSupplyChainDataset with custom DAX calculated columns directly on the fact table to ensure flawless continuous sorting without date hierarchy reliance:

Quarter-Year = "Q" & FORMAT ( 'DataCoSupplyChainDataset'[order date (DateOrders)], "q yyyy" )


QuarterSort = 
YEAR ( 'DataCoSupplyChainDataset'[order date (DateOrders)] ) * 10 
    + INT ( FORMAT ( 'DataCoSupplyChainDataset'[order date (DateOrders)], "q" ) )


Core DAX Measures

1. On-Time In-Full (OTIF) %

OTIF % = 
VAR OnTimeOrders = 
    CALCULATE (
        COUNT ( 'DataCoSupplyChainDataset'[Order Id] ),
        'DataCoSupplyChainDataset'[Delivery Status] IN { "Advance shipping", "Shipping on time" }
    )
VAR TotalOrders = COUNT ( 'DataCoSupplyChainDataset'[Order Id] )
RETURN
    DIVIDE ( OnTimeOrders, TotalOrders, 0 )


2. Late Delivery Risk Rate %

Late Risk Rate % = 
VAR LateOrders = 
    CALCULATE (
        COUNT ( 'DataCoSupplyChainDataset'[Order Id] ),
        'DataCoSupplyChainDataset'[Delivery Status] = "Late delivery"
    )
VAR TotalOrders = COUNT ( 'DataCoSupplyChainDataset'[Order Id] )
RETURN
    DIVIDE ( LateOrders, TotalOrders, 0 )


3. QoQ Sales Growth %

QoQ Growth Trend % = 
VAR CurrentSales = [Total Sales]
VAR CurrentDate = MAX ( 'DataCoSupplyChainDataset'[order date (DateOrders)] )
VAR PreviousQuarterDate = EDATE ( CurrentDate, -3 )

VAR PreviousSales = 
    CALCULATE (
        [Total Sales],
        FILTER (
            ALLSELECTED ( 'DataCoSupplyChainDataset' ),
            MONTH ( 'DataCoSupplyChainDataset'[order date (DateOrders)] ) = MONTH ( PreviousQuarterDate )
                && YEAR ( 'DataCoSupplyChainDataset'[order date (DateOrders)] ) = YEAR ( PreviousQuarterDate )
        )
    )

RETURN
    IF (
        ISBLANK ( CurrentSales ) || ISBLANK ( PreviousSales ),
        BLANK (),
        DIVIDE ( CurrentSales - PreviousSales, PreviousSales )
    )


📁 Repository Structure

├── assets/
│   ├── Executive_overview_dashboard.png
│   ├── Regional_logistics_dashboard.png
│   └── Product_analytics_dashboard.png
├── data/
│   └── DataCoSupplyChainDataset.csv
├── DataCo_Supply_Chain_Control_Tower.pbix
├── README.md
└── LICENSE


🚀 How to Open & Run the Project

Clone the Repository:

git clone https://github.com/your-username/DataCo-Supply-Chain-Control-Tower.git


Download Prerequisites:

Ensure you have the latest version of Power BI Desktop installed.

Launch the Dashboard:

Open Freight_Control_Tower.pbix.

If prompted for data source paths, re-link DataCoSupplyChainDataset.csv located inside the data/ folder.

Interact:

Use top-level synced slicers (Year, Region, Shipping Mode).

Select individual order rows in the grid on Page 1 and click View Details to trigger drillthrough context.

📄 License

This project is licensed under the MIT License — see the LICENSE file for details.
