# online-som-rl-strategy-pinescript

Implements PineScript-strategy based on self-organizing maps and reinforcement learning to trade price movements for use in TradingView.

This is an educational learning project only.

DISCLAIMER: DO NOT USE THIS SCRIPT WHEN YOU TRADE WITH YOUR REAL MONEY. I DO NOT GUARANTEE THAT THIS STRATEGY IS A PROFITABLE ONE LONG TERM. USE IT ON YOUR OWN RISK.

## How to use

Actual strategy is in file `./online-som-rl-strategy.txt`.

1. Open any chart in <https://tradingview.com/>.
2. Open the Pine Editor.
3. Copy & paste the code in file `./online-som-rl-strategy.txt` into the editor.
4. Use the "Add to chart"-button.

## How to use claude code in this project

On Windows: A .devcontainer-config is provided, so that you can use claude in a linux-container.

Install claude-cli first:

```bash
npm install -g @anthropic-ai/claude-code
```

Then, run claude-cli:

```bash
claude
```
