# splitlog

Unofficial CLI and MCP server to access Coros Training Hub data. All commands support `--json` for structured output.

> Not affiliated with COROS. "COROS" is a trademark of Guangdong COROS Sports Technology Co., Ltd. — referenced descriptively only.

## Commands

```
splitlog login                          # Authenticate (interactive email/password prompt)
splitlog activities [--page N --size N] [--json]  # List recent activities
splitlog activity <LABEL_ID> [--json]   # Detailed activity view (laps, zones, training effect)
splitlog health [--json]                # Health dashboard (running level, training status, race predictor, HRV)
```

## MCP Server

Also available as an MCP server for Claude Desktop and other MCP clients. Exposes 3 tools: `list_activities`, `get_activity`, `get_health`.

```json
{
  "mcpServers": {
    "splitlog": {
      "command": "npx",
      "args": ["-y", "@ibarker34/splitlog", "mcp"]
    }
  }
}
```

## Typical agent workflows

- **Get recent activities**: `splitlog activities --json --size 10`
- **Get activity detail**: First get the `labelId` from the activities list, then `splitlog activity <labelId> --json`
- **Get health/training overview**: `splitlog health --json`

## JSON output structure

### `splitlog activities --json`
Returns `data.dataList[]` with fields: `labelId`, `name`, `sportType`, `distance` (meters), `totalTime` (seconds), `avgHr`, `avgSpeed` (s/km), `ascent`, `date` (YYYYMMDD).

### `splitlog activity <id> --json`
Returns `data.summary` (distance in cm, totalTime in centiseconds, avgSpeed in s/km, aerobicEffect, anaerobicEffect, trainType), `data.lapList[]`, `data.zoneList[]`.

### `splitlog health --json`
Returns `analyse.data.dayList[]` (daily ATI/CTI/RHR/fatigue), `dashboard.data.summaryInfo` (running level, race predictor, HRV, threshold pace/HR), `dashboard_detail.data.currentWeekRecord` (weekly distance/duration targets).

## Setup

Requires authentication first: `splitlog login`. Credentials stored in `~/.config/splitlog/config.json`. Sessions are refreshed automatically when the token expires.
