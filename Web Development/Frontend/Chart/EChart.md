# Charting Technology Selection

## 1. Purpose
The purpose of this document is to evaluate and propose the most suitable charting technology for developing both a trend chart (time-series chart) and a high-density scatter chart for our frontend web application.

## 2. Targeted Reader
This document is intended for frontend engineers, technical leads, and product managers involved in the decision-making and implementation of the application's charting capabilities.

## 3. Background
We need to implement robust charting solutions capable of visualizing complex datasets efficiently. Specifically, we require an interactive trend chart for time-series data and a scatter chart capable of rendering high-density data (up to 16,000 concurrent points). The chosen technology must support a rich set of interactive features across desktop and tablet devices while maintaining strict performance standards. We are evaluating two well-established open-source libraries: Apache ECharts and Observable's D3.js. 

## 4. Tenets & Criteria
To ensure the selected technology aligns with our product requirements and engineering standards, we will evaluate options based on the following criteria:

### Functional Criteria
The chosen technology must support two primary chart types: **Time-Series Charts** (Trend Charts) and **Scatter Charts**.

#### Time series chart
The trend chart must support the following capabilities:
- **Multiple Series:** Support showing more than one pen.
- **Multiple Axes:** Support showing more than one Y-axis.
- **Dynamic Visibility:** Allow hiding any Y-axis or pen on demand.
- **Styling:** Support customizing axes styling (color, label, font-size, scale) and pen styling (line color, line style, distinct style on hover).
- **Interactivity:** Let the user place a cursor when clicked (a dashed vertical line for marking the X-value of interest).
- **Programmatic Control:** Let the user change the chart's view by time range programmatically using time control.
- **Tooltips:** Show a tooltip when hovering (Desktop) and tapping (Tablet device). The tooltip content must show both X and Y values for the point of interest and the annotation's metadata (customizable tooltip).
- **Zooming:** Let the user zoom in on an area via brushing (selecting an area) on Desktop or pinching on Tablet. When brushing is performed, an overlay rectangle should indicate the zooming area.
- **Data Types:** Support discrete (step) and analog pens.
- **Thresholds:** Change pen color when crossing a certain threshold or going out of range.
- **Annotations:** Allow the user to mark annotations on a pen or on a random point in the chart.
- **Animation:** Provide animation for view transitions and real-time interval updates.

#### Scatter chart
The scatter chart should satisfy all capabilities above, plus the following:
- **Data Capacity:** Support up to 8 data sources, each consisting of a maximum of 2000 data points.

### Non-Functional Criteria
- **Performance:** Must handle real-time updates and view transitions smoothly without degrading the user experience.
- **Customization:** Must provide sufficient flexibility to meet our exact styling and interaction design requirements.
- **Fast to Learn:** The API should be intuitive enough for the development team to pick up quickly, minimizing the learning curve and implementation time.
- **Community & Ecosystem:** Must have active support, rich documentation, and a strong community for troubleshooting.
- **Bundle Size:** Should have a manageable footprint to prevent degrading the initial page load time of the application.

## 5. Options Evaluated
We considered two primary candidates, both maintained by well-established open-source entities:

- **Apache ECharts:** A powerful, declarative charting and data visualization library. It is known for providing rich, interactive, out-of-the-box charts with excellent rendering performance (Canvas or SVG) and a gentle learning curve due to its configuration-driven API.
- **D3.js (Data-Driven Documents):** A low-level, highly flexible JavaScript library for manipulating the DOM based on data. D3 provides ultimate control over the final visual output, making it capable of virtually any bespoke visualization, but at the cost of a steeper learning curve and requiring more manual coding for standard charts.

## 6. Evaluation & Comparison

### 6.1 Functional Comparison
We evaluated both candidates against our functional criteria. The table below outlines whether each library supports the required feature natively or if it requires significantly more custom coding.

| Feature Requirement | ECharts | D3.js | Notes |
| :--- | :--- | :--- | :--- |
| **Showing >1 pen** | Natively | Extra coding | ECharts configures multiple series natively. D3 requires manually appending multiple SVG paths and binding data. |
| **Showing >1 Y-axis** | Natively | Extra coding | ECharts handles multiple `yAxis` configs natively. D3 requires manually rendering and positioning multiple axes. |
| **Hiding Y-axis on demand** | Natively | Extra coding | ECharts dynamically updates `show` property via `setOption`. D3 requires direct DOM manipulation to hide elements. |
| **Hiding pen on demand** | Natively | Extra coding | ECharts provides this out-of-the-box via its Legend component. D3 requires writing custom click events and opacity toggling. |
| **Customizing axes styling** | Natively | Extra coding | ECharts has a comprehensive styling API (color, label, font). D3 requires manual CSS/attribute assignments. |
| **Customizing pen styling** | Natively | Extra coding | ECharts natively supports hover effects, colors, and line styles. D3 requires manual SVG styling and event listeners for hover. |
| **Place cursor on click** | Extra coding | Extra coding | Both require custom logic. ECharts needs an event listener to inject a `markLine`. D3 needs DOM manipulation to draw a line on click. |
| **Change view by time control programmatically** | Natively | Extra coding | ECharts uses built-in `dataZoom` actions. D3 requires calculating new scale domains and re-rendering the chart. |
| **Tooltip on hover (Desktop) & tap (Tablet)** | Natively | Extra coding | ECharts natively handles cross-device tooltip triggers. D3 requires manually wiring `mouseover`/`touchstart` events. |
| **Custom Tooltip content (X/Y, annotations)** | Natively | Extra coding | ECharts uses `formatter` function. D3 requires manually injecting datum into a custom HTML overlay. |
| **Zoom via brushing/pinching** | Natively | Extra coding | ECharts natively supports this via `toolbox.dataZoom`. D3 requires wiring `d3-brush` and `d3-zoom` modules. |
| **Discrete (step) and analog pen** | Natively | Natively / Extra coding | ECharts uses `step: true`. D3 has `d3.curveStep` natively but requires manual integration into the path generator. |
| **Pen color change on threshold** | Natively | Extra coding | ECharts handles this natively via `visualMap`. D3 requires complex SVG `<linearGradient>` stops or splitting paths. |
| **Mark annotation** | Natively | Extra coding | ECharts provides `markPoint` and `markLine` components natively. D3 requires manually calculating coordinates and appending SVG elements. |
| **Animation and real-time updates** | Natively | Extra coding | ECharts handles view transitions natively upon receiving new data. D3 requires building manual update patterns with `d3-transition`. |
| **Data Capacity (8 sources x 2000 pts)** | Natively | Extra coding | ECharts handles 16,000+ points smoothly using its native Canvas engine. D3 requires writing a bespoke Canvas layer to avoid severe SVG DOM performance degradation. |

### 6.2 Non-Functional Evaluation

| Criteria | ECharts | D3.js |
| :--- | :--- | :--- |
| **Performance** | **High.** Uses Canvas natively; handles massive datasets (thousands of points) smoothly without UI blocking. | **Medium.** Tied to SVG DOM by default. Rendering massive datasets requires custom Canvas integration. |
| **Customization** | **High.** Excellent options for standard charts, but slightly restrictive for completely bespoke, non-standard visuals. | **Ultimate.** No restrictions; offers pixel-perfect control over every DOM element and interaction. |
| **Adoption Speed** | **Fast.** Configuration-driven API allows engineers to stand up a complex chart in days. | **Slow.** Steep learning curve requires deep domain knowledge (data joins, scales, complex math) to onboard. |
| **Community & Ecosystem** | **Strong.** Backed by Apache; excellent documentation and massive adoption in enterprise dashboards. | **Massive.** The industry standard for bespoke data visualization; unparalleled examples and community support. |
| **Bundle Size** | **Moderate.** Core is larger, but supports modular tree-shaking to only include used chart components. | **Small.** Highly modular architecture allows importing strictly the mathematical and DOM functions needed. |

## 7. Recommendation

Based on the evaluation above, **we recommend adopting Apache ECharts** as the primary charting technology for our trend chart implementation. We base this decision on the following advantages:

- **Native Feature Support:** It fulfills our required functional capabilities (e.g., multiple axes, brushing/zooming, tooltips) out-of-the-box without requiring custom DOM manipulation.
- **High Performance:** Its Canvas-based rendering engine easily handles massive time-series datasets and real-time updates without dropping frames.
- **Fast Adoption:** The declarative configuration API significantly lowers the learning curve compared to D3.js, allowing the engineering team to deliver a production-ready chart rapidly.
- **Engineering Efficiency:** It minimizes long-term maintenance overhead by providing built-in interactive behaviors, saving us from writing and maintaining complex math and event-handling logic.