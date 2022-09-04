# DEX Aggregator 이해하기

<br>

## 1. DEX Aggregator란

### 1-1. DEX의 유동성 모으기

[AMM](https://www.coinbase.com/learn/crypto-basics/what-is-a-dex) 기반의 [DEX](https://www.coinbase.com/learn/crypto-basics/what-is-a-dex)가 잘 작동하려면 유동성을 확보하는 것이 중요합니다. 사용자가 토큰을 스왑할 때, 스왑하는 두 토큰의 유동성 풀이 크면 클 수록 [슬리피지](https://www.coinbase.com/learn/crypto-basics/what-is-a-dex)가 줄어들어 실제 거래가격을 정확하게 예상할 수 있기 때문입니다. 사용자가 예상한 가격과 실제 체결되는 가격의 괴리가 적어야 사용자들이 거래소를 안심하고 이용할 수 있겠지요. [Uniswap](https://app.uniswap.org/#/swap?chain=mainnet)의 경우 스왑 수수료의 대부분을 유동성 풀 제공자들에게 배분함으로써 많은 유동성을 모을 수 있었습니다. Uniswap의 성공 이후 수많은 DEX들이 등장하면서 DEX간 경쟁이 치열해졌습니다. 스왑 수수료가 0인 DEX들도 생겨났는데요, 스왑 수수료가 없기 때문에 유동성 제공자에게 풀 토큰을 민팅해주고, 풀 토큰 [Yield Farming](https://academy.binance.com/ko/articles/what-is-yield-farming-in-decentralized-finance-defi#what-is-yield-farming)을 통해 수익을 얻을 수 있게 함으로써 유동성을 모으고 있습니다.

<br>

### 1-2. DEX Aggregator

DEX Aggregator란, 말그대로 여러 DEX의 유동성 풀들을 한데 모아 제공하는 서비스입니다. DEX Aggregator 개념을 가져온 [1inch](https://app.1inch.io/#/1/swap/ETH/DAI), [0x](https://www.0x.org/) 같은 프로젝트들은 직접 유동성을 모으는 대신, 이미 존재하는 수많은 DEX들의 유동성을 효율적으로 활용하는 방법을 찾는데 집중했습니다. 사용자 입장에서 가장 좋은 가격 조건으로 토큰을 스왑할 수 있도록 여러 DEX의 유동성 풀들을 탐색하여 최적의 Trading 경로를 제시합니다. 스왑 가격뿐만 아니라 슬리피지, 스왑 수수료, Gas 비용이 모두 고려되고요. 최적의 조건에 거래하기 위해 한 번의 스왑 거래를 할 때 다수의 DEX를 통하는데, 예를 들어 ETH-USDC 스왑 거래를 할 때 40%의 거래는 Uniswap V3를 통하고, 나머지 60%는 Balancer를 통하도록 하는 것이지요. 

<br>

## 2. PathFinder

### 2-1. 1inch Aggregation protocol

[1inch](https://app.1inch.io/#/1/swap/ETH/DAI)는 PathFinder 알고리즘을 통해 가장 저렴한 스왑 가격, 최저 슬리피지, 가장 낮은 Gas 비용을 찾아서 제공합니다. [1inch의 GitHub](https://github.com/1inch)을 찾아봤는데 PathFinder 컨트랙트는 찾을 수 없었고요, [Aggregation protocol API](https://docs.1inch.io/docs/aggregation-protocol/api/swagger)를 통해 최적화된 스왑을 이행해주는 Transaction Hash를 받을 수 있다는 것을 알 수 있었습니다. 이 API를 호출해서 두 토큰의 최적화된 스왑 거래 내용이 담긴 Transaction Hash를 받은 후 컨트랙트 호출 시 인자로 넘겨주면 컨트랙트가 해당 Transaction을 실행해줍니다.

<br>

PathFinder 알고리즘에 대한 힌트는 DODO 블로그의 [DODO Research: Reveal the Secrets of the Aggregator — Problem Analysis and Model Building](https://blog.dodoex.io/dodo-research-reveal-the-secrets-of-the-aggregator-problem-analysis-and-model-building-ba0ead85948c)에서 찾을 수 있었습니다. 이 글은 PathFinder 알고리즘에 앞서 Linear Routing, Split Routing의 차이에 대해 설명하는데요, 그 둘을 요약하면 다음과 같습니다.

<br>

### 2-2. Linear Routing

이 방법은 스왑을 위해 풀들을 탐색할 때 하나의 풀씩 순차적으로 통하는 방법인데요, 가령 ETH/USDC 스왑을 위해 Linear Routing 방식으로 스왑 경로를 찾으면, 다음과 같은 결과를 얻을 수 있습니다.

- ETH/USDT → USDT/USDC

이때 Aggregator라면 ETH/USDT 스왑 단계에서는 Uniswap V2의 유동성 풀을 사용하고, USDT/USDC 스왑 단계에서는 Curve V1의 유동성 풀을 사용할 수 있겠습니다.

<br>

### 2-3. Split Routing

WIP


<br>


---

### References

- [DODO Research: Reveal the Secrets of the Aggregator — Problem Analysis and Model Building](https://blog.dodoex.io/dodo-research-reveal-the-secrets-of-the-aggregator-problem-analysis-and-model-building-ba0ead85948c)
- [Why should you integrate 1inch APIs into your service?](https://blog.1inch.io/why-should-you-integrate-1inch-apis-into-your-service-8a3100984885)
- [1inch | Crypto Adventure](https://cryptoadventure.com/researches/1inch/)
