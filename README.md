# Outrun MCP Server

This is the official **Model Context Protocol (MCP)** server for [Outrun](https://0ut.run), an AI-native crypto state machine.

It connects AI assistants (like Claude Desktop) directly to high-resolution crypto market data, allowing them to answer complex questions about volatility, correlations, volume anomalies, and mining risks in real-time.

## Features

- **Real-time Price Data**: Get aggregated prices from 6 major exchanges (Binance, Coinbase, Kraken, etc.).
- **Volatility Analysis**: Calculate standard deviation over specific time windows (e.g., "1h").
- **Wick Detection**: Find assets that have flash-crashed or pumped >X% and returned to baseline.
- **Threshold Checks**: Verify if a price persisted above or below a level for a set duration.
- **Volume Anomalies**: Detect if current volume is significantly higher than the 10-day baseline.
- **Correlation**: Check how two assets move together (Pearson coefficient).
- **Mining Weather**: Monitor heat risks at major global Bitcoin mining hubs.
- **System Status**: Check the health and data lag of the Outrun engine.

## Installation

Add the following configuration to your `claude_desktop_config.json` file:

```json
{
  "mcpServers": {
    "outrun": {
      "command": "npx",
      "args": [
        "-y",
        "outrun-mcp"
      ],
      "env": {
        "OUTRUN_API_KEY": "YOUR_API_KEY_HERE",
        "OUTRUN_API_URL": "https://0ut.run"
      }
    }
  }
}
```

## Configuration

This server requires an API Key.

**How to Buy a Key:**
1.  **Generate Invoice:** `POST /key/buy` (Body: `{ "amount": 1000 }`)
2.  **Pay Invoice:** Use a Lightning wallet to pay.
3.  **Claim Key:** `GET /key/claim/:orderId` -> Returns `{ "apiKey": "..." }`

## Tools & Examples

Once connected, you can ask Claude questions that utilize these tools:

### 1. `get_price` (Calls `GET /j/price`)
Get the current consensus price of an asset.
*   **Prompt:** "What is the price of BTC?"
*   **Prompt:** "Check the price of SOL at 10:00 AM yesterday." (Historical)

### 2. `get_volatility` (Calls `GET /j/volatility`)
Check market stability over a time window.
*   **Prompt:** "Is ETH volatility high right now compared to the last hour?"
*   **Prompt:** "Calculate the 4-hour volatility for DOGE."

### 3. `detect_wick` (Calls `GET /j/wick`)
Find flash crashes or pump-and-dump wicks.
*   **Prompt:** "Did SOL have any flash crashes >5% in the last 24 hours?"
*   **Prompt:** "Check if BTC wicked down more than 3% in the last 6 hours."

### 4. `get_threshold` (Calls `GET /j/threshold`)
Verify if price conditions held true over time.
*   **Prompt:** "Has BTC held above $95,000 for the last hour?"
*   **Prompt:** "Did ETH drop below $2,500 at any point in the last 30 minutes?"

### 5. `get_volume` (Calls `GET /j/volume`)
Detect anomalous volume activity.
*   **Prompt:** "Is there any anomalous volume activity for BTC right now? Is it >3x normal?"
*   **Prompt:** "Check the volume status for PEPE."

### 6. `get_correlation` (Calls `GET /j/correlation`)
Analyze the relationship between two assets.
*   **Prompt:** "Are BTC and ETH moving together? Check their correlation over the last 24 hours."
*   **Prompt:** "Is there a decoupling between SOL and ETH in the last 4 hours?"

### 7. `get_weather` (Calls `GET /j/weather`)
Monitor Bitcoin mining curtailment risks.
*   **Prompt:** "Are mining hubs in Texas at risk of curtailment due to heat?"
*   **Prompt:** "Check the weather risk for the Rockdale mining facility (Code: TX_RDL)."

### 8. `get_status` (Calls `GET /status`)
Check the system health and data freshness.
*   **Prompt:** "Is the Outrun system online and healthy?"
*   **Prompt:** "Check if the crypto data is lagging."

## License

ISC
