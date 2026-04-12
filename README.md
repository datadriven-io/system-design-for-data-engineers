# System Design for Data Engineers

> Long form, end to end pipeline case studies for the system design round of a data engineering interview. Not microservices. Not request paths. Pipelines.

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC_BY--SA_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

## The problem this repo solves

Almost every "system design" repo on GitHub teaches you how to design Twitter, design TinyURL, design a chat app. None of those questions appear in a data engineering loop.

What appears in a DE loop is:

- "Design Netflix's viewing history pipeline."
- "Design a cost optimized clickstream data lake with two year retention."
- "Design a CDC pipeline that replicates Postgres into Snowflake with sub minute latency."
- "Design a real time fraud scoring pipeline for card transactions."
- "Design a regulatory lineage system for an intraday risk pipeline at a bank."

These questions reward depth in **data flow tradeoffs**, not microservice topology. They reward knowing the difference between batch and stream not as a vocabulary item but as a real cost and correctness decision. They reward knowing storage formats, partitioning strategies, idempotency patterns, and the operational reality of running a 24/7 pipeline.

This repo is a curated set of long form case studies that prepare you for exactly that kind of round.

## Related repositories

This repo is the **system design specialist**. The companion repos cover the rest of the loop:

- **[data-engineering-interview-handbook](https://github.com/datadriven-io/data-engineering-interview-handbook)** is the **flagship handbook** for the full DE interview loop.
- **[data-engineering-interview-questions](https://github.com/datadriven-io/data-engineering-interview-questions)** is the **1400+ question bank** for SQL, Python, and schema design rounds.
- **[data-engineer-interview-prep](https://github.com/datadriven-io/data-engineer-interview-prep)** is an **8 week structured practice schedule**.
- **[data-engineering-cheatsheet](https://github.com/datadriven-io/data-engineering-cheatsheet)** is the **night before recall reference** including pipeline patterns, Spark, Airflow, dbt, and Kafka quick references.
- **[awesome-data-engineering-interview](https://github.com/datadriven-io/awesome-data-engineering-interview)** is the **curated resource list**.
- **[data-engineer-interview-handbook](https://github.com/datadriven-io/data-engineer-interview-handbook)** is the **7 day sprint** version of the handbook.

## Contents

- [The DE system design framework](#the-de-system-design-framework)
- [Case studies by domain](#case-studies-by-domain)
  - [Streaming and real time](#streaming-and-real-time)
  - [Batch and warehouse](#batch-and-warehouse)
  - [Lakehouse and data lake](#lakehouse-and-data-lake)
  - [CDC and replication](#cdc-and-replication)
  - [ML and feature pipelines](#ml-and-feature-pipelines)
  - [Regulatory and compliance](#regulatory-and-compliance)
  - [IoT and high volume telemetry](#iot-and-high-volume-telemetry)
- [Reference architectures](#reference-architectures)
- [Concept primers](#concept-primers)
- [Books and papers worth reading](#books-and-papers-worth-reading)
- [Contributing](#contributing)

## The DE system design framework

Memorize this. Use it as the spine of every answer. The single biggest reason candidates fail this round is that they jump to drawing boxes before they have clarified the problem.

### The eight beats

Every DE system design answer should hit these in order. Spend roughly the time budget shown.

| Beat | What you do | Time budget (45 min round) |
|---|---|---|
| 1. Clarify | Ask volume, latency, freshness, retention, read pattern | 5 min |
| 2. Estimate | Records per second, bytes per record, total per day, storage growth | 3 min |
| 3. Pick freshness target | Real time, near real time, hourly, daily | 1 min |
| 4. Pick batch vs stream | Justify with the numbers from beat 2 and 3 | 2 min |
| 5. Pick storage | Lake, warehouse, lakehouse, OLAP store, kv store | 3 min |
| 6. Sketch topology | Source, ingest, transform, serve | 10 min |
| 7. Failure modes | Backfills, replays, late data, dedup, schema evolution | 10 min |
| 8. Cost and operations | Steady state spend, on call burden, monitoring | 8 min |

If you are short on time, beats 1, 4, 6, and 7 are the ones interviewers actually score. The rest are warmups and finishers.

### The four numbers you should always derive

Any answer that does not include these is a red flag to interviewers.

1. **Volume.** Records per second, peak vs average, total per day.
2. **Bytes.** Per record raw, per record after serialization, per day stored.
3. **Latency.** End to end from event time to serve time. Target and tolerance.
4. **Cost ceiling.** Order of magnitude for monthly spend. Even a wrong answer here is better than no answer.

A longer treatment of the framework is at <https://datadriven.io/data-engineering-system-design>.

## Case studies by domain

Each case study is a real interview prompt. Open the link, design the system end to end on paper for 30 minutes, then read the worked solution. The full library has 120 case studies, browseable at <https://datadriven.io/data-pipeline-interview-questions>.

### Streaming and real time

The hardest and most valuable category. Streaming questions reward depth in exactly once semantics, watermarks, late data, and fault tolerance.

- **[Card Transaction Streaming Pipeline](https://datadriven.io/interview/card_transaction_streaming_pipeline)**. Real time fraud feedback. Tests exactly once semantics, watermarking, and how to feed model scores back into the transaction path.
- **[Ad Simulation Platform Pipeline](https://datadriven.io/interview/ad_simulation_platform_pipeline)**. Synthetic event generation with sub second feedback loops. Tests stateful stream processing and replayability.
- **[Real Time Analytics with Backfill Sync](https://datadriven.io/interview/realtime_analytics_backfill_sync)**. The classic "lambda vs kappa" question, in a form that actually requires you to defend a choice.
- **[Streaming Video Lag Detection](https://datadriven.io/interview/streaming_video_lag_detection)**. Sliding window aggregation, late event handling, alert fanout.
- **[Cross Platform TV and Digital Ad Measurement Pipeline](https://datadriven.io/interview/cross_platform_tv_and_digital_ad_measurement_pipeline)**. Multi source identity resolution in near real time.

### Batch and warehouse

Batch is unfashionable but still ~70 percent of production pipelines. Knowing when batch is the right answer is itself a signal.

- **[E Commerce Platform Analytics Pipeline](https://datadriven.io/interview/e_commerce_platform_analytics_pipeline_orders_to_warehouse)**. Orders, returns, cancellations into a star schema warehouse. Tests SCD handling and late arriving facts.
- **[Data Pipeline for Sales Analytics](https://datadriven.io/interview/data_pipeline_for_sales_analytics)**. The "build me an executive dashboard" prompt, done correctly.
- **[Consumer Goods Trade Promotion Pipeline on GCP](https://datadriven.io/interview/consumer_goods_trade_promotion_pipeline_on_gcp)**. Multi source enrichment and slowly changing dimensions.
- **[Document Ingestion and Text Extraction Pipeline](https://datadriven.io/interview/document_ingestion_and_text_extraction_pipeline)**. Unstructured data into a structured warehouse.

### Lakehouse and data lake

The hot 2026 category. Iceberg, Delta, and Hudi are all fair game.

- **[Cost Optimized Clickstream Data Lake](https://datadriven.io/interview/cost_optimized_clickstream_data_lake)**. Multi tier storage, partitioning, compaction, file size strategy.
- **[Cost Efficient Clickstream Analytics with Two Year Retention](https://datadriven.io/interview/cost_efficient_clickstream_analytics_with_two_year_retention)**. Hot vs warm vs cold tier design.
- **[Databricks Pipeline with Spark Performance Optimization](https://datadriven.io/interview/databricks_pipeline_with_spark_performance_optimization)**. Spark internals, shuffle reduction, Z ordering.
- **[Azure Data Factory Orchestration with Databricks Unity Catalog](https://datadriven.io/interview/azure_data_factory_orchestration_with_databricks_unity_catalog)**. Cross plane orchestration with governance.
- **[Data Platform IaC with Semantic Layer](https://datadriven.io/interview/data_platform_iac_with_semantic_layer)**. The "design the platform, not the pipeline" prompt.

### CDC and replication

CDC is increasingly the answer to the "how do you get data out of an operational database without breaking it" question. Know it cold.

- **[Database Replication and Schema Normalization Pipeline](https://datadriven.io/interview/database_replication_and_schema_normalization_pipeline)**. Log based CDC with schema evolution.
- **[CDC Connector: Log Based vs Trigger Based](https://datadriven.io/interview/cdc_connector_log_based_vs_trigger_based)**. The tradeoff question. Memorize the answer.
- **[Batch ETL: MongoDB to Redshift](https://datadriven.io/interview/batch_etl_mongodb_to_redshift)**. Schema evolution from a document store into a relational warehouse.
- **[Dual Source Inventory Sync Pipeline](https://datadriven.io/interview/dual_source_inventory_sync_pipeline)**. Reconciling two sources of truth, handling conflicts.

### ML and feature pipelines

DE/ML platform overlap is increasingly common in DE loops, especially at consumer companies.

- **[Device Insurance Claims Pipeline with Real Time Fraud Scoring](https://datadriven.io/interview/device_insurance_claims_pipeline_with_real_time_fraud_scoring)**. Online and offline feature parity.
- **[Event Driven Insurance Pipeline with Async Claim Processing](https://datadriven.io/interview/event_driven_insurance_pipeline_with_async_claim_processing)**. Async ML scoring with delayed labels.
- **[City Wide Bicycle Demand Forecasting Pipeline](https://datadriven.io/interview/city_wide_bicycle_demand_forecasting_pipeline)**. Time series feature engineering at the platform layer.

### Regulatory and compliance

These come up at finance, healthcare, and any company that has been audited. The signal is that you understand lineage, retention, and audit logs as first class concerns.

- **[Capital Markets Intraday Risk Pipeline with BCBS 239 Lineage](https://datadriven.io/interview/capital_markets_intraday_risk_pipeline_with_bcbs_239_lineage)**. The benchmark for regulatory lineage questions.
- **[EHR Platform Operational Data Pipeline](https://datadriven.io/interview/ehr_platform_operational_data_pipeline)**. PHI handling, audit logs, retention enforcement.
- **[Energy Trading Market Data Pipeline](https://datadriven.io/interview/energy_trading_market_data_pipeline)**. Real time with strict ordering and late arrival handling.

### IoT and high volume telemetry

When the question starts with "millions of devices" or "trillions of events per day", this is the playbook.

- **[Connected Vehicle Telemetry Pipeline with IaC Deployment](https://datadriven.io/interview/connected_vehicle_telemetry_pipeline_with_iac_deployment)**. Geo partitioning, sparse data, regional failover.
- **[Cellular Connectivity and App Log Data Warehouse](https://datadriven.io/interview/cellular_connectivity_and_app_log_data_warehouse)**. High cardinality dimension explosion.
- **[Clickstream Pipeline for Apple Product Analytics](https://datadriven.io/interview/clickstream_pipeline_for_apple_product_analytics)**. Event ingestion at consumer app scale.
- **[AWS Pipeline Auto Scaling for Variable Volume](https://datadriven.io/interview/aws_pipeline_auto_scaling_for_variable_volume)**. Diurnal and bursty load patterns.

## Reference architectures

These are the "shapes" you should be able to draw from memory. Every case study above is a variation on one of these patterns.

### Pattern 1: Lambda

Batch path for correctness, stream path for freshness. Both write to a serving layer that merges them. Pros: bulletproof. Cons: two codebases.

### Pattern 2: Kappa

Stream only. Reprocess from the log when you need to backfill or correct. Pros: one codebase. Cons: hard to debug, requires log retention long enough for full reprocessing.

### Pattern 3: Medallion (bronze/silver/gold)

Lakehouse layered model. Bronze is raw, silver is cleaned, gold is business ready. Pros: clean separation of concerns, easy to reason about. Cons: read amplification.

### Pattern 4: CDC into warehouse

Log based CDC from operational store, landed into staging tables, transformed by dbt or similar into the warehouse model. Pros: low impact on operational store, near real time. Cons: schema evolution is annoying.

### Pattern 5: Event sourced with projections

Append only event log as source of truth. Multiple read models projected from the log for different query patterns. Pros: full history, replayable, supports many read patterns. Cons: complexity, eventual consistency.

A longer reference of each pattern with diagrams is at <https://datadriven.io/data-pipeline-architecture>.

## Concept primers

Read these before you tackle the case studies. Each is a short lesson with worked examples.

### Storage and format

- [Data warehouse vs data lake](https://datadriven.io/data-warehouse-vs-data-lake)
- [Star schema vs snowflake schema](https://datadriven.io/star-schema-vs-snowflake)
- [Data lakehouse architecture](https://datadriven.io/data-lakehouse)
- [Modern data stack overview](https://datadriven.io/modern-data-stack)

### Modeling

- [Data modeling field guide](https://datadriven.io/data-modeling)
- [Dimensional modeling lesson](https://datadriven.io/learn/data-modeling-dimensional)
- [Slowly changing dimensions](https://datadriven.io/learn/data-modeling-scd)
- [Event stream modeling](https://datadriven.io/learn/data-modeling-event-streams)

### Pipeline patterns

- [Batch vs stream processing](https://datadriven.io/batch-vs-stream-processing)
- [ETL vs ELT](https://datadriven.io/etl-vs-elt)
- [Data pipeline architecture](https://datadriven.io/data-pipeline-architecture)
- [Data mesh](https://datadriven.io/data-mesh)
- [Data fabric](https://datadriven.io/data-fabric)

### Operations

- [Data quality](https://datadriven.io/data-quality)
- [Data observability](https://datadriven.io/data-observability)
- [Data governance](https://datadriven.io/data-governance)

## Books and papers worth reading

For this round, depth beats breadth. Read these in order.

1. **Designing Data Intensive Applications**, Martin Kleppmann. The single most useful book in print for this round. Read chapters 1, 3, 5, 6, 7, 11.
2. **Streaming Systems**, Tyler Akidau, Slava Chernyak, Reuven Lax. The deepest treatment of streaming semantics in print. Required for kappa and exactly once questions.
3. **The Data Warehouse Toolkit**, Ralph Kimball. For the dimensional modeling parts of pipeline design.
4. **Fundamentals of Data Engineering**, Joe Reis and Matt Housley. Best modern survey of the field.
5. **The Log: What every software engineer should know about real time data's unifying abstraction**, Jay Kreps. Free online. Foundational.

## Company specific patterns

Each company tends to ask system design questions that look like their actual stack. Use these guides as cheat sheets.

| Company | Stack signal | Guide |
|---|---|---|
| Netflix | Streaming, OLAP at scale, Iceberg | <https://datadriven.io/companies/netflix/interview> |
| Uber | Real time, geo partitioning, Pinot, Hudi | <https://datadriven.io/companies/uber/interview> |
| Amazon | Leadership principles plus deep AWS | <https://datadriven.io/companies/amazon/interview> |
| Google | BigQuery patterns, Dataflow | <https://datadriven.io/companies/google/interview> |
| Meta | Presto, warehouse at scale | <https://datadriven.io/companies/meta/interview> |

## Contributing

Have a case study to add? Open a PR with:

1. The problem prompt (one paragraph, vague enough to require clarification)
2. The four numbers (volume, bytes, latency, cost ceiling)
3. The chosen architecture and why
4. The failure modes addressed
5. The tradeoffs explicitly considered

Worked solutions live in `/case-studies/<slug>/solution.md`. Stub solutions are welcome and will be polished by maintainers.

## Frequently asked questions

### Is system design for data engineers different from software engineering system design?

Yes, very. Software engineering system design is about request paths, microservices, caching layers, and load balancing. Data engineering system design is about data flows: batch vs stream tradeoffs, storage formats, partitioning strategies, idempotency, late data handling, and the operational reality of running a 24/7 pipeline. Asking how to design Twitter is the wrong rehearsal for asking how to design Netflix's viewing history pipeline. This repo focuses entirely on the DE flavor.

### What is the difference between batch and stream processing in interviews?

Batch processes a bounded chunk of data on a schedule. Stream processes an unbounded series of events as they arrive. The interview question is rarely "which is better". It is "given these freshness, volume, and cost constraints, which would you pick and why". Memorize the four numbers (volume, bytes, latency, cost) so you can defend the choice with arithmetic instead of opinion.

### Should I learn Kafka for a data engineering interview?

If you are interviewing for any role that touches event ingestion or streaming, yes. You should be able to explain partitions, consumer groups, offsets, exactly once semantics, and rebalancing. The [Kafka section of the cheatsheet](https://github.com/datadriven-io/data-engineering-cheatsheet#kafka) covers the minimum.

### What is the medallion architecture and why does it matter in interviews?

Medallion is a lakehouse pattern with three layers: bronze (raw landed data), silver (cleaned and conformed), gold (business ready aggregates). It matters in interviews because it gives you a clean way to talk about separation of concerns in a pipeline answer, and because Databricks-shop interviewers will expect you to know it by name. See [Reference architectures](#reference-architectures).

### How do I prepare for a system design interview at a streaming company like Netflix or Uber?

Pick three case studies from the [Streaming and real time](#streaming-and-real-time) section, design each one end to end on paper for 45 minutes, then read the worked solution. Repeat with three more if you have time. Focus on watermarks, late data handling, and the difference between event time and processing time. Both Netflix and Uber expect candidates to talk about exactly once semantics fluently.

### What is the eight beat framework?

A structured rubric for answering any DE system design question: clarify, estimate, pick freshness, pick batch vs stream, pick storage, sketch topology, address failure modes, talk cost and operations. See [The eight beats](#the-eight-beats). Memorize it. Use it as the spine of every answer.

### How is data engineering system design tested at FAANG companies?

Each FAANG company has a slightly different flavor: Netflix focuses on streaming and OLAP at scale, Uber focuses on real time and geo partitioning, Amazon focuses on AWS depth and leadership principles, Google focuses on BigQuery and Dataflow patterns, Meta focuses on Presto and warehouse at scale. Each [company specific guide](#company-specific-patterns) has the details.

### Is this repo free?

Yes. CC BY-SA 4.0. Case studies and solutions can be copied, forked, and republished as long as you credit the source and share derivatives under the same license.

## License

CC BY-SA 4.0. Case study environments and solutions are hosted at <https://datadriven.io>.
