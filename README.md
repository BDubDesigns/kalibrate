### **Project Specification: Kalibrate - A Kalshi (and eventually others) Trading Terminal**

**Project Category:** Business/Startup & Portfolio/Learning

**1. Executive Summary & Business Strategy**
* **Target Audience:** The professional retail trader who approaches prediction markets with derivative-trading discipline.
* **Business Model:** A "freemium" platform where highly realistic paper trading acts as a free acquisition channel to build a user base. 
* **Monetization:** Premium tiers unlock live execution via API, advanced algorithmic signals, AI-driven trade analysis, and server-side automation.
* **Operational Jurisdiction:** Business operations, hosting, and live deployment will be managed in Mexico to strictly comply with the Washington State prohibition on prediction market infrastructure.

**2. Technical Architecture**
* **Stack:** MERN (MongoDB, Express, React, Node.js) implemented with TypeScript for strict type safety across the application boundary.
* **Data Ingestion:** WebSocket integration for low-latency Level 2 order book updates.
* **Rate Limiting:** Implementation of a token bucket algorithm to queue and throttle outgoing Kalshi API requests, prioritizing order cancellations to manage risk.

**3. Core Trading Features & Execution**
* **The Shadow Engine (Paper Trading):** A high-fidelity simulation environment that ingests the live order book to calculate accurate weighted average prices and simulate realistic market slippage.
* **"Shift-Click" Execution:** A frontend React implementation bypassing standard confirmation modals for rapid order entry, protected by a user-enabled liability waiver.
* **Order Feathering (Batching):** A ladder tool allowing users to spread large positions across multiple price levels, leveraging Kalshi's API to submit up to 20 orders in a single HTTP request.
* **Smart Order Routing (Optimal Entry & Exit):** An automated pricing execution engine that scans for the mathematically cheapest path to a position. 
    * *Optimal Entry:* In mutually exclusive, zero-tie markets, the engine calculates whether buying "Yes" on Outcome A is cheaper than buying "No" on Outcome B, executing the most cost-effective route.
    * *Synthetic Exits:* When closing a position, the engine evaluates liquidity and spread to determine whether it is more profitable to directly sell the existing position (e.g., Sell "Yes") or to take the opposing side (e.g., Buy "No") to lock in the spread and neutralize market exposure.

**4. Advanced Alpha Signals (Premium Features)**
* **AI Trade Post-Mortem Analytics:** A personalized learning engine that evaluates the user's completed trades against historical order book data. The AI acts as a trading coach, identifying where the user went wrong, highlighting optimal exit windows they missed, or flagging when market conditions (like low liquidity or high spread) dictated they shouldn't have entered the market at all.
* **Institutional Wall Detection:** Algorithmic highlighting of the order book when a price level's resting liquidity exceeds the moving average by a configurable Z-score.
* **Iceberg Tracking:** Correlation of Time & Sales with the order book to detect hidden liquidity, specifically when an Ask or Bid size immediately refreshes after a trade.
* **Arbitrage Scanner:** An automated monitor for mutually exclusive markets (Dutch Booking) that alerts the user when the sum of Ask prices drops below $1.00 (accounting for fees).

**5. Security, Onboarding, and Compliance**
* **BYOK (Bring Your Own Key) Architecture:** Users supply their own API keys, reducing the platform's status to a data visualizer rather than a commercial vendor.
* **Zero-Knowledge Storage:** API keys are held exclusively in the client's `sessionStorage` or an encrypted vault, ensuring the backend never holds plaintext custody of user funds.
* **Frictionless Onboarding:** A built-in, browser-based RSA key generator to guide non-technical users through Kalshi's complex API setup without requiring command-line tools.
* **Strict Geofencing:** Hardcoded IP blocks for Washington State and Nevada to prevent live trading access within prohibited jurisdictions.
