# PATIENCE-MULTISIMULATION

> GPU Strategy Engine - Algorithmes universels avec MAPPING balises pour acces direct IA et automatisation

![Version](https://img.shields.io/badge/version-2.0-blue)
![GPU](https://img.shields.io/badge/GPU-CUDA%2FcuDF-green)
![License](https://img.shields.io/badge/license-MIT-green)

## Vue d'ensemble

Ce module fournit un moteur de strategies GPU vectorisees pour le trading algorithmique. Il utilise un systeme de balises MAPPING pour permettre un acces direct par les IA et une automatisation complete.

## Architecture

```
PATIENCE-MULTISIMULATION/
├── GPU_STRATEGY_ENGINE.json    # 8 strategies vectorisees
├── MAPPING.json                # Balises acces direct IA
├── TRIGGER_AUTOMATION.json     # Declencheurs automatiques
└── README.md
```

## GPU Strategy Engine

### Strategies Disponibles

| Strategy | Description | Signal |
|----------|-------------|--------|
| EMA_CROSS_RSI | EMA 9/21 crossover + RSI filter | LONG/SHORT |
| BOLLINGER_BREAKOUT | Bollinger Bands breakout | LONG/SHORT |
| MACD_CROSS | MACD histogram crossover | LONG/SHORT |
| STOCHASTIC | Stochastic oscillator oversold/overbought | LONG/SHORT |
| VWAP_BOUNCE | VWAP support/resistance bounce | LONG/SHORT |
| ATR_BREAKOUT | ATR volatility breakout | LONG/SHORT |
| MOMENTUM_ROC | Rate of Change momentum | LONG/SHORT |
| DOUBLE_EMA | Double EMA trend following | LONG/SHORT |

### Format Strategie

```json
{
  "strategy_id": "EMA_CROSS_RSI",
  "gpu_optimized": true,
  "vectorized": true,
  "parameters": {
    "ema_fast": 9,
    "ema_slow": 21,
    "rsi_period": 14,
    "rsi_oversold": 30,
    "rsi_overbought": 70
  },
  "signals": {
    "LONG": "ema_fast > ema_slow AND rsi < 30",
    "SHORT": "ema_fast < ema_slow AND rsi > 70"
  }
}
```

## MAPPING Balises

Le fichier `MAPPING.json` permet aux IA d'acceder directement aux strategies:

```json
{
  "BALISE_BREAKOUT": ["EMA_CROSS_RSI", "BOLLINGER_BREAKOUT", "ATR_BREAKOUT"],
  "BALISE_REVERSAL": ["STOCHASTIC", "VWAP_BOUNCE"],
  "BALISE_MOMENTUM": ["MACD_CROSS", "MOMENTUM_ROC", "DOUBLE_EMA"],
  "BALISE_CONSERVATIVE": ["EMA_CROSS_RSI", "MACD_CROSS"],
  "BALISE_AGGRESSIVE": ["ATR_BREAKOUT", "MOMENTUM_ROC"]
}
```

### Utilisation par IA

```python
# Acces direct via balise
strategies = mapping["BALISE_BREAKOUT"]
# -> ["EMA_CROSS_RSI", "BOLLINGER_BREAKOUT", "ATR_BREAKOUT"]

# Execution GPU vectorisee
for strategy in strategies:
    signals = gpu_engine.execute(strategy, ohlcv_data)
```

## Trigger Automation

Le fichier `TRIGGER_AUTOMATION.json` definit les declencheurs automatiques:

```json
{
  "triggers": [
    {
      "condition": "volume_spike > 200%",
      "action": "execute_balise",
      "balise": "BALISE_BREAKOUT"
    },
    {
      "condition": "rsi < 25",
      "action": "execute_balise",
      "balise": "BALISE_REVERSAL"
    },
    {
      "condition": "trend_strength > 0.8",
      "action": "execute_balise",
      "balise": "BALISE_MOMENTUM"
    }
  ]
}
```

## Integration avec Trading AI Ultimate

Ce module est integre dans le systeme principal:

```python
from gpu_pipeline import TradingPipeline

# Charger les strategies GPU
pipeline = TradingPipeline()

# Analyser un symbole avec toutes les strategies
result = pipeline.analyze_coin("BTC_USDT")

# Resultat:
# {
#   "buy_count": 5,
#   "sell_count": 2,
#   "signal": "LONG",
#   "confidence": 71.4%
# }
```

## Performance GPU

| Metric | CPU | GPU (CUDA) |
|--------|-----|------------|
| 1 coin / 8 strategies | 2.5s | 0.15s |
| 100 coins / 8 strategies | 250s | 8s |
| 800+ coins full scan | 30min | 2min |

## Repository Principal

Ce module fait partie de [Trading AI Ultimate](https://github.com/Turbo31150/trading-ai-n8n-workflows).

## License

MIT License - Turbo31150
