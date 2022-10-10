# Cosmos SDK, Application-specific, Tendermint

<br>

1. Cosmos SDK란, Application-specific 블록체인
2. 블록체인 구조: 상태 머신, Deterministic & Replicated, Tendermint
3. Cosmos SDK `BaseApp`

<br>

## 1. Cosmos SDK란, Application-specific 블록체인

Cosmos SDK는 [PoS](https://en.wikipedia.org/wiki/Proof_of_stake) 블록체인을 빠르게 개발할 수 있는 프레임워크입니다. Cosmos SDK의 목표 중 하나가 누구나 커스텀 블록체인을 쉽게 만들 수 있도록 하는 것인만큼, Cosmos SDK를 사용해서 만든 블록체인들은 일반적으로 [Application-specific](https://docs.cosmos.network/main/intro/why-app-specific) 블록체인입니다. Application-specific 블록체인은 말그대로 애플리케이션에 특화된 블록체인을 의미하는데, 이는 어떤 종류의 애플리케이션이든 컨트랙트(계약)으로 만들어 배포할 수 있는 [Ethereum] 블록체인과 대조됩니다. 가령, Ethereum 블록체인 위에서 작동하는 서비스들은 종류가 참 다양하지요. DeFi 애플리케이션인 [Uniswap](https://app.uniswap.org/#/swap?chain=mainnet)과 [Curve](https://curve.fi/)가 유명하고요, 메타버스 애플리케이션인 [Sandbox](https://www.sandbox.game/en/)와 [Decentraland](https://decentraland.org/)도 있고요, [CryptoPunks](https://www.larvalabs.com/cryptopunks)와 같은 NFT 프로젝트도 있지요. 이와 반대로, Application-specific 블록체인은 하나의 애플리케이션을 위한 커스텀 블록체인으로, 애플리케이션의 특성에 최적화된 블록체인을 말합니다. Cosmos SDK는 Application-specific 블록체인을 쉽게 만들도록 고안된 프레임워크로, 가령 블록 Validator 수, Transaction 처리량, Account 모델 등에서 상당한 자유도를 제공하고, 암호화 라이브러리도 자유롭게 선택할 수 있습니다.

> The goal of the Cosmos SDK is to allow developers to easily create custom blockchains from scratch that can natively interoperate with other blockchains. - [High-level Overview | Cosmos SDK Documentation](https://docs.cosmos.network/main/intro/overview)

<br>

## 2. 블록체인 구조: 상태 머신, Deterministic & Replicated, Tendermint

### 2-1. 상태 머신

블록체인을 설명하는 다양한 말들이 있지만, Computer science 관점에서 블록체인은 그저 상태(State) 머신입니다. 어떤 한 시점에 특정한 하나의 상태(State)를 가지는 머신으로, 블록체인에서 각 시점은 블록의 높이(Block height)로 구분됩니다. 그러니까 블록 높이 1에서 상태 값이 `S`라면, 다음 블록인 블록 높이 2에서는 일부 값에 변경이 있는 상태 `S'`를 갖게 됩니다. 매 블록이 추가될 때마다 Transaction들이 일어나고 그에 따라 각 블록 높이에서 계속해서 다른 상태 값을 갖게 됩니다. 

<br>

### 2-2. Deterministic & Replicated

또 하나 기억할 것은 블록체인이 Deterministic 하다는 것인데, 블록체인에서 블록은 변경되거나 삭제되지 않고 계속해서 추가만 될 뿐이므로, 특정 블록 높이에서 결정된 상태 값은 불변합니다! 그러니까 블록체인이라는 것은 이 연결된 블록들의 각 높이에서의 상태 값들을 저장하고있는 저장소이면서, 새로운 상태 값을 가지는 블록이 계속해서 추가될 수 있도록 하는 일종의 프로그램입니다. 그리고 이 블록체인은 여러 컴퓨터들에 의해 공유되고 합의되고 복제(Replicate)됩니다.

> At its core, a blockchain is a replicated deterministic state machine. - [Blockchain Architecture](https://docs.cosmos.network/main/intro/sdk-app-architecture)

<br>

## 2-3. Tendermint

Cosmos SDK를 사용해서 블록체인을 만드는 것이 (비교적) 쉬운 이유 중 하나는, 블록체인 전체를 0부터 100까지 구현할 필요가 없기 때문입니다. 새로운 블록 생성이 Propose되고, 블록을 검증하는 노드들로 전파되고, 합의가 이루어지는 부분들을 모두 Tendermint 엔진에 위임하기 때문에, Cosmos SDK를 사용해서 블록체인을 만드는 개발자들은 애플리케이션의 상태값이 변경되는 부분의 개발에만 집중할 수 있습니다.

> Thanks to the Cosmos SDK, developers just have to define the state machine, and Tendermint will handle replication over the network for them. (...ABBR...) Tendermint is an application-agnostic engine that is responsible for handling the networking and consensus layers of a blockchain. - [Blockchain Architecture](https://docs.cosmos.network/main/intro/sdk-app-architecture)

<br>

```zsh
                ^  +-------------------------------+  ^
                |  |                               |  |   Built with Cosmos SDK
                |  |  State-machine = Application  |  |
                |  |                               |  v
                |  +-------------------------------+
                |  |                               |  ^
Blockchain node |  |           Consensus           |  |
                |  |                               |  |
                |  +-------------------------------+  |   Tendermint Core
                |  |                               |  |
                |  |           Networking          |  |
                |  |                               |  |
                v  +-------------------------------+  v
```

<br>

Tendermint 엔진은 블록에 포함된 Transaction 코드를 [ABCI](https://github.com/tendermint/tendermint/blob/v0.34.x/spec/abci/README.md) Interface를 통해 애플리케이션에 전달합니다. 따라서 Tendermint 엔진을 사용하는 블록체인을 만들기 위해서는 반드시 ABCI를 구현해야하고, Cosmos SDK는 ABCI 구현 코드가 내장된 프레임워크입니다.

Tendermint 엔진이 핸들링하는 Transaction 코드는 Byte로 인코딩되어있는데, 따라서 애플리케이션에서 ABCI의 `DeliverTx` 구현체를 통해 전달받은 Transaction 데이터는 `[]bytes` 타입입니다. 애플리케이션에서는 이 데이터를 Decode해서 `messages`를 추출하고, 각 `message`를 적절한 Module로 라우팅해줍니다. 각 Module로 라우팅된 Transaction이 성공적으로 실행되면 드디어 상태(State) 값에 변경이 일어나게 됩니다.

<br>

## 3. Cosmos SDK `BaseApp`

[`BaseApp`](https://docs.cosmos.network/main/core/baseapp)은 Cosmos SDK 애플리케이션의 코어 부분을 구현하기 위한 타입으로, 여기에 Tendermint 엔진과 상호작용하기 위한 ABCI 구현체와 `message`들을 각 Module로 라우팅하는 부분도 포함되어 있습니다. 일반적으로 개발자들은 다음과 같이 `BaseApp`을 포함하는 애플리케이션 구조체를 작성하게 됩니다.

```go
type App struct {
  // reference to a BaseApp
  *baseapp.BaseApp

  // list of application store keys

  // list of application keepers

  // module manager
}
```

(WIP)

<br>

---

### References

- [High-level Overview | Cosmos SDK Documentation](https://docs.cosmos.network/main/intro/overview)
- [Blockchain Architecture](https://docs.cosmos.network/main/intro/sdk-app-architecture)
