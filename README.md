<h1 align="center">System Design for Data Engineers</h1>

<p align="center">
  120 end to end pipeline case studies for the system design round of a data engineering interview. Plus the eight beat framework for structured answers.
</p>

<p align="center">
  <a href="https://github.com/datadriven-io/system-design-for-data-engineers/stargazers"><img src="https://img.shields.io/github/stars/datadriven-io/system-design-for-data-engineers?style=flat&color=ffd33d" alt="Stars"></a>
  <a href="https://github.com/datadriven-io/system-design-for-data-engineers/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-CC%20BY--SA%204.0-blue.svg" alt="License"></a>
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs welcome">
  <a href="https://datadriven.io"><img src="https://img.shields.io/badge/sandbox-datadriven.io-9333ea.svg" alt="Sandbox"></a>
</p>

<p align="center">
  <a href="#the-eight-beat-framework">Framework</a> ·
  <a href="#case-studies">Case studies</a> ·
  <a href="#reference-architectures">Patterns</a> ·
  <a href="#companion-repos">Companion repos</a>
</p>

---

Most "system design" repos teach you how to design Twitter, TinyURL, or a chat app. None of those questions appear in a data engineering loop.

DE system design rounds ask things like: design Netflix's viewing history pipeline, design a CDC replication into Snowflake with sub minute latency, design a real time fraud scoring pipeline for card transactions, design a regulatory lineage system for an intraday risk pipeline at a bank. The questions reward depth in batch vs stream tradeoffs, storage formats, partitioning, idempotency, late data handling, and the operational reality of running 24/7 pipelines.

This repo has 120 case studies of exactly that kind, plus a framework for structuring answers.

## The eight beat framework

Use this on every question. Roughly the time budget for a 45 minute round.

| # | Beat | Time |
|---|---|---:|
| 1 | **Clarify** volume, latency, freshness, retention, read pattern | 5 min |
| 2 | **Estimate** records per second, bytes per record, total per day | 3 min |
| 3 | **Pick freshness target** (real time, near real time, hourly, daily) | 1 min |
| 4 | **Pick batch vs stream** with arithmetic from beat 2 | 2 min |
| 5 | **Pick storage** (lake, warehouse, lakehouse, OLAP, kv) | 3 min |
| 6 | **Sketch topology** (source, ingest, transform, serve) | 10 min |
| 7 | **Address failure modes** (backfills, replays, late data, dedup) | 10 min |
| 8 | **Talk cost and operations** (monthly spend, on-call burden) | 8 min |

If you are short on time, beats 1, 4, 6, and 7 are the ones interviewers actually score.

### Four numbers to derive on every answer

1. **Volume.** Records per second, peak vs average, total per day.
2. **Bytes.** Per record raw, per record compressed, per day stored.
3. **Latency.** End to end target.
4. **Cost ceiling.** Order of magnitude monthly spend.

Long form treatment: [datadriven.io/data-engineering-system-design](https://datadriven.io/data-engineering-system-design).

## Case studies

Pick three. Design end to end on paper for 30 minutes each before reading the solution. Browse all 120 at [datadriven.io/data-pipeline-interview-questions](https://datadriven.io/data-pipeline-interview-questions).

### Streaming and real time

| Case study | Tests |
|---|---|
| [Card Transaction Streaming Pipeline](https://datadriven.io/interview/card_transaction_streaming_pipeline) | Exactly once, watermarking, fraud feedback |
| [Ad Simulation Platform Pipeline](https://datadriven.io/interview/ad_simulation_platform_pipeline) | Stateful streams, replayability |
| [Streaming Video Lag Detection](https://datadriven.io/interview/streaming_video_lag_detection) | Sliding windows, late events, alerts |
| [Cross Platform TV and Digital Ad Measurement Pipeline](https://datadriven.io/interview/cross_platform_tv_and_digital_ad_measurement_pipeline) | Multi source identity resolution |

### Batch and warehouse

| Case study | Tests |
|---|---|
| [E Commerce Platform Analytics Pipeline](https://datadriven.io/interview/e_commerce_platform_analytics_pipeline_orders_to_warehouse) | Star schemas, late arriving facts |
| [Data Pipeline for Sales Analytics](https://datadriven.io/interview/data_pipeline_for_sales_analytics) | Executive dashboard build |
| [Consumer Goods Trade Promotion Pipeline](https://datadriven.io/interview/consumer_goods_trade_promotion_pipeline_on_gcp) | Multi source enrichment, SCDs |
| [Document Ingestion and Text Extraction Pipeline](https://datadriven.io/interview/document_ingestion_and_text_extraction_pipeline) | Unstructured to structured |

### Lakehouse and data lake

| Case study | Tests |
|---|---|
| [Cost Optimized Clickstream Data Lake](https://datadriven.io/interview/cost_optimized_clickstream_data_lake) | Tiered storage, partitioning, compaction |
| [Cost Efficient Clickstream Analytics with Two Year Retention](https://datadriven.io/interview/cost_efficient_clickstream_analytics_with_two_year_retention) | Hot vs warm vs cold |
| [Databricks Pipeline with Spark Performance Optimization](https://datadriven.io/interview/databricks_pipeline_with_spark_performance_optimization) | Spark internals, Z ordering |
| [Azure Data Factory with Databricks Unity Catalog](https://datadriven.io/interview/azure_data_factory_orchestration_with_databricks_unity_catalog) | Cross plane orchestration |

### CDC and replication

| Case study | Tests |
|---|---|
| [Database Replication and Schema Normalization Pipeline](https://datadriven.io/interview/database_replication_and_schema_normalization_pipeline) | Log based CDC, schema evolution |
| [CDC Connector: Log Based vs Trigger Based](https://datadriven.io/interview/cdc_connector_log_based_vs_trigger_based) | The tradeoff question |
| [Batch ETL: MongoDB to Redshift](https://datadriven.io/interview/batch_etl_mongodb_to_redshift) | Document to relational |
| [Dual Source Inventory Sync Pipeline](https://datadriven.io/interview/dual_source_inventory_sync_pipeline) | Reconciling sources of truth |

### ML and feature pipelines

| Case study | Tests |
|---|---|
| [Device Insurance Claims Pipeline with Real Time Fraud Scoring](https://datadriven.io/interview/device_insurance_claims_pipeline_with_real_time_fraud_scoring) | Online and offline parity |
| [Event Driven Insurance Pipeline with Async Claim Processing](https://datadriven.io/interview/event_driven_insurance_pipeline_with_async_claim_processing) | Async ML scoring |
| [City Wide Bicycle Demand Forecasting Pipeline](https://datadriven.io/interview/city_wide_bicycle_demand_forecasting_pipeline) | Time series feature engineering |

### Regulatory and compliance

| Case study | Tests |
|---|---|
| [Capital Markets Intraday Risk Pipeline with BCBS 239 Lineage](https://datadriven.io/interview/capital_markets_intraday_risk_pipeline_with_bcbs_239_lineage) | Regulatory lineage |
| [EHR Platform Operational Data Pipeline](https://datadriven.io/interview/ehr_platform_operational_data_pipeline) | PHI, audit logs, retention |
| [Energy Trading Market Data Pipeline](https://datadriven.io/interview/energy_trading_market_data_pipeline) | Strict ordering, late arrival |

### IoT and high volume telemetry

| Case study | Tests |
|---|---|
| [Connected Vehicle Telemetry Pipeline](https://datadriven.io/interview/connected_vehicle_telemetry_pipeline_with_iac_deployment) | Geo partitioning, regional failover |
| [Cellular Connectivity and App Log Data Warehouse](https://datadriven.io/interview/cellular_connectivity_and_app_log_data_warehouse) | High cardinality dimensions |
| [Clickstream Pipeline for Apple Product Analytics](https://datadriven.io/interview/clickstream_pipeline_for_apple_product_analytics) | Consumer scale ingestion |
| [AWS Pipeline Auto Scaling for Variable Volume](https://datadriven.io/interview/aws_pipeline_auto_scaling_for_variable_volume) | Diurnal load patterns |

## Reference architectures

The five shapes you should be able to draw from memory.

| Pattern | When | Tradeoff |
|---|---|---|
| **Lambda** | Batch for correctness, stream for freshness, merged in serving | Two codebases |
| **Kappa** | Stream only, replay log to backfill | Hard to debug, log retention cost |
| **Medallion** | Bronze, silver, gold layered lakehouse | Read amplification |
| **CDC into warehouse** | Log based CDC, dbt transforms, near real time | Schema evolution complexity |
| **Event sourced with projections** | Append only log as source of truth, multiple read models | Eventual consistency |

Longer reference: [datadriven.io/data-pipeline-architecture](https://datadriven.io/data-pipeline-architecture).

## Reading list

Read in this order. Depth beats breadth for this round.

1. *Designing Data Intensive Applications*, Martin Kleppmann. Chapters 1, 3, 5, 6, 7, 11.
2. *Streaming Systems*, Tyler Akidau et al. Required for kappa and exactly once questions.
3. *The Data Warehouse Toolkit*, Ralph Kimball. For the dimensional modeling parts.
4. *Fundamentals of Data Engineering*, Joe Reis and Matt Housley. Modern survey.
5. *The Log* by Jay Kreps. Free essay. Foundational.

## Company specific patterns

| Company | Stack signal | Guide |
|---|---|---|
| Netflix | Streaming, OLAP at scale, Iceberg | [companies/netflix/interview](https://datadriven.io/companies/netflix/interview) |
| Uber | Real time, Pinot, Hudi | [companies/uber/interview](https://datadriven.io/companies/uber/interview) |
| Amazon | AWS depth, leadership principles | [companies/amazon/interview](https://datadriven.io/companies/amazon/interview) |
| Google | BigQuery, Dataflow | [companies/google/interview](https://datadriven.io/companies/google/interview) |
| Meta | Presto, warehouse at scale | [companies/meta/interview](https://datadriven.io/companies/meta/interview) |

## Companion repos

- [data-engineering-interview-handbook](https://github.com/datadriven-io/data-engineering-interview-handbook). The flagship DE handbook covering every round.
- [data-engineering-interview-questions](https://github.com/datadriven-io/data-engineering-interview-questions). 1418 SQL, Python, schema, and pipeline problems.
- [data-engineering-cheatsheet](https://github.com/datadriven-io/data-engineering-cheatsheet). One page recall reference.
- [data-engineer-interview-prep](https://github.com/datadriven-io/data-engineer-interview-prep). 8 week structured practice track.
- [data-engineer-interview-handbook](https://github.com/datadriven-io/data-engineer-interview-handbook). 7 day sprint.
- [awesome-data-engineering-interview](https://github.com/datadriven-io/awesome-data-engineering-interview). Curated resource list.

## Contributing

PRs welcome. Add a case study with: prompt, the four numbers, chosen architecture, failure modes, tradeoffs explicitly considered.

## License

[CC BY-SA 4.0](LICENSE). Solutions and sandboxes hosted at [datadriven.io](https://datadriven.io).
