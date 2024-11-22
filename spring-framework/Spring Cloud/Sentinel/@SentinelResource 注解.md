### @SentinelResource 注解

[annotation-support | Sentinel (sentinelguard.io)](https://sentinelguard.io/zh-cn/docs/annotation-support.html)

<font color="red">**若 blockHandler 和 fallback 都进行了配置，则被限流降级而抛出 `BlockException` 时只会进入 `blockHandler` 处理逻辑**</font>。若未配置 `blockHandler`、`fallback` 和 `defaultFallback`，则被限流降级时会将 `BlockException` **直接抛出**。

@SentinelResource 只负责处理 BlockException



### blockHandler

**作用**: `blockHandler` 用于处理由 **限流**、**熔断降级**、或**系统保护** 等 Sentinel 规则触发的异常情况。

**触发条件**: 当资源请求被 Sentinel 的规则（例如限流规则、熔断规则、热点参数规则等）触发阻塞时，Sentinel 会调用 `blockHandler` 方法。

**使用场景**: 当资源的访问频率超过配置的阈值（如 QPS 超过限制，错误比例超过限制等），Sentinel 会触发阻塞策略，而不会让该请求继续执行。此时，`blockHandler` 用于处理这种被阻塞的请求。



### fallback

**作用**: `fallback` 用于处理业务执行过程中抛出的 **运行时异常**，也用于**降级处理**。

**触发条件**: 当被保护的资源发生 **运行时异常** 或启用了 Sentinel 的降级规则时，会触发 `fallback`。

**使用场景**: 适用于业务逻辑执行时发生异常（如网络故障、数据库连接失败等）或者定义的降级规则触发时。`fallback` 用于对这种异常情况进行处理，以保证服务的可用性，避免系统崩溃或返回未处理的错误信息。