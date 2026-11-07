# How to connect GetYourGuide to Claude Code, ChatGPT and any AI agent

**Yes, Claude, ChatGPT and other AI agents can work in your GetYourGuide account.** GetYourGuide's official apps in ChatGPT and Claude help travelers find and book tours, not manage a supplier account. Cotlet.dev connects your own signed-in GetYourGuide supplier account to any AI agent, so the AI lists bookings and updates availability for you.

## What you can ask

> List all bookings for my Lisbon sunset sailing tour this Saturday with guest names and group size.

> Block availability for the 9 am Old Town walking tour from December 24 to 26.

> Add a 4 pm slot to the Colosseum skip-the-line tour every Friday in December, 12 spots each.

> How many bookings did each of my tours get last week? Show cancellations separately.

> Cut Sunday's kayak tour to 8 spots, we only have one guide that day.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its GetYourGuide tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"getyourguide":{"command":"uvx","args":["getyourguide-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t getyourguide-mcp .
docker run -i --rm -e COTLET_API_KEY getyourguide-mcp
```

Tools: `getyourguide_guide`, `getyourguide_status`, `getyourguide_open`, `getyourguide_read`, `getyourguide_click`, `getyourguide_type`, `getyourguide_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install getyourguide-mcp        # or run it once: uvx getyourguide-mcp connect
getyourguide-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 getyourguide_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 getyourguide_agent.py status    # is GetYourGuide ready in your Cotlet.dev account
python3 getyourguide_agent.py guide     # GetYourGuide screens and sign-in link
python3 getyourguide_agent.py read      # open GetYourGuide, read the page, close the browser
python3 getyourguide_agent.py tools     # list Cotlet.dev tools
python3 getyourguide_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from GetYourGuide users

**Does GetYourGuide have an API?**

GetYourGuide has a Supplier API, but it is built for reservation systems and channel managers that pass its connectivity testing, not for a tour operator who wants a key (checked 11 Oct 2026). Every system must implement the mandatory endpoints, and the docs describe no API for customer messages or reviews. Cotlet.dev works in your own signed-in GetYourGuide account, so your AI agent can list bookings and update availability with no API key and no code.

**How do I connect GetYourGuide to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to GetYourGuide once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no GetYourGuide API key.

**Is there a way to use Claude with GetYourGuide?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own GetYourGuide account.

**Is there a GetYourGuide MCP server?**

GetYourGuide has an official Claude connector for travelers booking tours and activities. For your own supplier account, Cotlet.dev is a hosted MCP server that works in GetYourGuide for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

## Find it on

- [GetYourGuide on Cotlet.dev](https://cotlet.dev/integrations/getyourguide-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=getyourguide&utm_content=find)
- [PyPI package getyourguide-mcp](https://pypi.org/project/getyourguide-mcp/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-getyourguide-to-ai-agents-guide)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/getyourguide-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=getyourguide

Start free: https://cotlet.dev/signup?app=getyourguide&utm_source=github&utm_medium=readme&utm_campaign=getyourguide
