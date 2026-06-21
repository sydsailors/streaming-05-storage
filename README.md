# streaming-05-storage

[![API Reference](https://img.shields.io/badge/API--Utils-datafun--streaming-purple)](https://denisecase.github.io/datafun-streaming/api/)
[![Workflow Guide](https://img.shields.io/badge/Pro--Guide-pro--analytics--02-green)](https://denisecase.github.io/pro-analytics-02/workflow-b-apply-example-project/)
[![Python 3.14](https://img.shields.io/badge/python-3.14%2B-blue?logo=python)](./pyproject.toml)
[![MIT](https://img.shields.io/badge/license-see%20LICENSE-yellow.svg)](./LICENSE)

> Streaming data analytics: store processed messages.

Streaming analytics requires working with data in motion
and distributed, scalable systems.
This course builds capabilities through working projects.
In the age of generative AI, durable skills are grounded in real work:
setting up a professional environment,
reading and running code,
understanding the logic,
and pushing work to a shared repository.
Each project follows the structure of professional Python projects.
We learn by doing.

## This Project

This project focuses on storing streaming data after it is consumed.

The project uses Kafka to move sales messages from a producer to a consumer.
The consumer reads each message, validates required fields, computes derived values,
writes processed records to CSV, and stores results in DuckDB.

This module adds persistent storage to the streaming workflow.

The goal is to see how consumed messages can be saved for later inspection,
querying, and analysis.

## Working Files

You'll work with just these areas:

- **data/** - input data and generated output files
- **docs/** - the project narrative and documentation
- **src/streaming/** - producer, consumer, and supporting code
- **pyproject.toml** - update authorship & links
- **zensical.toml** - update authorship & links

## Instructions

Follow the
[step-by-step workflow guide](https://denisecase.github.io/pro-analytics-02/workflow-b-apply-example-project/)
to complete:

1. Phase 1. **Start & Run**
2. Phase 2. **Change Authorship**
3. Phase 3. **Read & Understand**
4. Phase 4. **Modify**
5. Phase 5. **Apply**

## Challenges

Challenges are expected.
Sometimes instructions may not quite match your operating system.
When issues occur, share screenshots, error messages, and details about what you tried.
Working through issues is part of implementing professional projects.

## Success

After completing Phase 1. **Start & Run**, you'll have your own GitHub project
running with Kafka.

Use four named terminals:

1. **kafka** - keep the Kafka message broker running
2. **topics** - create, list, or reset Kafka topics
3. **producer** - run the project and producer
4. **consumer** - run the consumer

After the producer and consumer run successfully, you should see:

```shell
========================
Consumer executed successfully!
========================
```

A new file `project.log` will appear in the root project folder
and processed data will appear in data/output/.

## Command Reference

The commands below are used in the workflow guide above.
They are provided here for convenience.

**Important:** the first few times you run a project,
follow the guide with the **complete instructions**.

<details>
<summary>Show command reference</summary>

### In a machine terminal (open in your `Repos` folder)

After you get a copy of this repo in your own GitHub account,
open a machine terminal in your `Repos` folder:

```bash
# Replace username with YOUR GitHub username.
git clone https://github.com/username/streaming-05-storage

cd streaming-05-storage
code .
```

### In VS Code Terminal 1: Start Kafka (kafka)

For full instructions see
[**start kafka**](https://denisecase.github.io/pro-analytics-02/kafka/start-kafka/).

If any command fails,
repeat the steps at
[**install kafka**](https://denisecase.github.io/pro-analytics-02/kafka/install-kafka/)
until starting up is reliable.

Open a new VS Code terminal. Rename it `kafka`.
If running Windows, specify the terminal type as **wsl** or
type `wsl`.
Run the commands one at a time.

Step 1. Verify Java and PATH

```bash
echo "$JAVA_HOME"

"$JAVA_HOME/bin/java" --version
```

Step 2. Rebuild ClusterID (as needed)

```bash
cd ~/kafka

rm -rf /tmp/kraft-combined-logs

KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"

echo "Cluster ID: $KAFKA_CLUSTER_ID"

bin/kafka-storage.sh format --standalone -t "$KAFKA_CLUSTER_ID" -c config/server.properties
```

Step 3. Start kafka server (keep running)

```bash
cd ~/kafka

bin/kafka-server-start.sh config/server.properties
```

### In VS Code terminal 2: Create Topic (topics)

For full instructions see
[**create topic**](https://denisecase.github.io/pro-analytics-02/kafka/create-topic/).

The topic name must match the name defined in your
`.env` file (copy `.env.example` to `.env`).

Open another VS Code terminal. Rename it `topics`.
If running Windows, specify the terminal type as **wsl** or
type `wsl`.
Run the commands one at a time.

```bash
cd ~/kafka

bin/kafka-topics.sh --create \
  --bootstrap-server localhost:9092 \
  --partitions 1 \
  --replication-factor 1 \
  --topic streaming-05-storage-case
```

### In VS Code Terminal 3: Run Project and Producer (producer)

Open another VS Code terminal. Rename it `producer`.
If running Windows, use **PowerShell**.
Run the commands one at a time.

```shell
# reset uv cache only if/when you start getting strange dependency errors
# uv cache clean

uv self update
uv python pin 3.14
uv sync --extra dev --extra docs --upgrade

uvx pre-commit install

git add -A
uvx pre-commit run --all-files
# repeat if changes were made
git add -A
uvx pre-commit run --all-files

# run the producer
clear
uv run python -m streaming.kafka_producer_case

# do chores
uv run ruff format .
uv run ruff check . --fix
uv run python -m pyright
uv run python -m pytest
uv run python -m zensical build

# save progress
git add -A
git commit -m "update"
git push -u origin main
```

### In VS Code Terminal 4: Run Consumer (consumer)

Open another VS Code terminal. Rename it `consumer`.
If running Windows, use **PowerShell**.
Run the commands one at a time.
Clear the terminal, then start the consumer.

### Phase 4

```shell
clear
uv run python -m streaming.kafka_consumer_sailors
```

### Phase 5

```shell
clear
uv run python -m streaming.kafka_consumer_sailorsP5
```

To start fresh, see
[manage topics](https://denisecase.github.io/pro-analytics-02/kafka/manage-topics/)
to delete the topic and recreate it.

</details>

## Notes

- Use the **UP ARROW** and **DOWN ARROW** in the terminal to scroll through past commands.
- Use `CTRL+f` to find (and replace) text within a file.
- You do not need to add to or modify `tests/`. They are provided for example only.
- Many files are silent helpers. Explore as you like, but nothing is required.
- You do NOT not to understand everything; understanding builds naturally over time.

## Troubleshooting >>> or

If you see something like this in your terminal: `>>>` or `...`
You accidentally started Python interactive mode.
It happens.
Press `Ctrl+c` (both keys together) or `Ctrl+Z` then `Enter` on Windows.

## Example Producer Output

The example producer output is unchanged from previous projects.

## Consumer P5 Output

Look for the text `db`:

```text
| C05 | ========================
| C05 | START consumer main()
| C05 | ========================
| C05 | ROOT_DIR = .
| C05 | DATA_DIR = data
| C05 | OUTPUT_CSV = data/output/consumed_sales_sailorsP5.csv
| C05 | OUTPUT_DB = data/output/sales_sailorsP5.duckdb
| C05 | REGIONS_CSV = data/regions.csv
| C05 | PRODUCTS_CSV = data/products.csv
| C05 | CURRENCIES_CSV = data/currencies.csv
| C05 | DISCOUNT_CODES_CSV = data/discount_codes.csv
| C05 | ========================
| C05 | SECTION A. Acquire
| C05 | ========================
| C05 | Loading settings from .env...
| C05 | KAFKA_BOOTSTRAP_SERVERS  = localhost:9092
| C05 | KAFKA_TOPIC              = streaming-05-storage-case
| C05 | KAFKA_GROUP_ID           = streaming-consumer-group-A
| C05 | CONSUMER_TIMEOUT_SECONDS = 10.0
| C05 | CONSUMER_MAX_MESSAGES    = 1000
| C05 | Verifying Kafka connection...
| C05 | Kafka port is reachable.
| C05 | Verifying Kafka topic...
| C05 | Topic 'streaming-05-storage-case' exists.
| C05 | Found 10 message(s) available.
| C05 | Creating Kafka consumer...
| C05 | Subscribed to topic: 'streaming-05-storage-case' (reading from beginning)
| C05 | ========================
| C05 | SECTION C. Consume and Process Messages
| C05 | ========================
| C05 | Initializing output...
| C05 | Output CSV cleared: consumed_sales_sailorsP5.csv
| C05 | Database initialized: sales_sailorsP5.duckdb
| C05 | Loading enrichment reference data...
| C05 | Found 6 region tax rates.
| C05 | Consuming messages...
| C05 | Waiting for up to 1000 message(s).
| C05 | Press CTRL+C to stop early.

| {'currency_code': 'USD', 'customer_id': 'CUST-4150', 'customer_note': 'Gift for my team', 'datetime': '2026-05-04T08:11:00Z', 'device_type': 'tablet', 'discount_code': '', 'is_new_customer': 'false', 'is_online': 'true', 'order_id': 'e7324981-a9f0-419f-b708-d0a333451fff', 'payment_method': 'paypal', 'product_id': 'PY-STREAM-005', 'quantity': '3', 'referral_source': 'paid_search', 'region_id': 'US-TX', 'unit_price': '59.99', '_kafka_key': 'US-TX', '_kafka_partition': 0, '_kafka_offset': 0}
| C05 | subtotal=179.97
| C05 | tax=14.85
| C05 | total=194.82
| C05 | running_total=194.82
| C05 | Wrote valid record to DuckDB:
| C05 |   order=e7324981-a9f0-419f-b708-d0a333451fff
| C05 | MESSAGE ACCEPTED
| C05 | order=e7324981-a9f0-419f-b708-d0a333451fff
| C05 | total=$194.82
| C05 | consumed=1
| C05 | RUNNING STATS
| C05 | total_sales=$194.82
| C05 | average=$194.82
| C05 | min=$194.82
| C05 | max=$194.82
| C05 | {'currency_code': 'USD', 'customer_id': 'CUST-1106', 'customer_note': 'Gift for my team', 'datetime': '2026-05-04T08:23:00Z', 'device_type': 'mobile', 'discount_code': '', 'is_new_customer': 'false', 'is_online': 'true', 'order_id': 'd61943e0-f543-4b5f-9c9a-18605ea4cfe5', 'payment_method': 'paypal', 'product_id': 'PY-DATA-002', 'quantity': '1', 'referral_source': 'paid_search', 'region_id': 'US-TX', 'unit_price': '49.99', '_kafka_key': 'US-TX', '_kafka_partition': 0, '_kafka_offset': 1}
| C05 | subtotal=49.99
| C05 | tax=4.12
| C05 | total=54.11
| C05 | running_total=248.93
| C05 | Wrote valid record to DuckDB:
| C05 |   order=d61943e0-f543-4b5f-9c9a-18605ea4cfe5
| C05 | MESSAGE ACCEPTED
| C05 | order=d61943e0-f543-4b5f-9c9a-18605ea4cfe5
| C05 | total=$54.11
| C05 | consumed=2
| C05 | RUNNING STATS
| C05 | total_sales=$248.93
| C05 | average=$124.47
| C05 | min=$54.11
| C05 | max=$194.82
| C05 | {'currency_code': 'CAD', 'customer_id': 'CUST-2133', 'customer_note': 'Learning at my own pace', 'datetime': '2026-05-04T08:28:00Z', 'device_type': 'desktop', 'discount_code': '', 'is_new_customer': 'false', 'is_online': 'true', 'order_id': '14da1915-8e74-47be-9e10-f7275d31af46', 'payment_method': 'paypal', 'product_id': 'PY-NLP-006', 'quantity': '1', 'referral_source': 'organic', 'region_id': 'CA-QC', 'unit_price': '54.99', '_kafka_key': 'CA-QC', '_kafka_partition': 0, '_kafka_offset': 2}
| C05 | subtotal=54.99
| C05 | tax=8.23
| C05 | total=63.22
| C05 | running_total=312.15
| C05 | Wrote valid record to DuckDB:
| C05 |   order=14da1915-8e74-47be-9e10-f7275d31af46
| C05 | MESSAGE ACCEPTED
| C05 | order=14da1915-8e74-47be-9e10-f7275d31af46
| C05 | total=$63.22
| C05 | consumed=3
| C05 | RUNNING STATS
| C05 | total_sales=$312.15
| C05 | average=$104.05
| C05 | min=$54.11
| C05 | max=$194.82
| C05 | {'currency_code': 'USD', 'customer_id': 'CUST-5333', 'customer_note': 'Highly recommend', 'datetime': '2026-05-04T08:40:00Z', 'device_type': 'mobile', 'discount_code': '', 'is_new_customer': 'false', 'is_online': 'true', 'order_id': 'd77ed935-9b28-431d-bc25-0a3fd98f4154', 'payment_method': 'paypal', 'product_id': 'PY-STREAM-005', 'quantity': '1', 'referral_source': 'organic', 'region_id': 'US-CA', 'unit_price': '59.99', '_kafka_key': 'US-CA', '_kafka_partition': 0, '_kafka_offset': 3}
| C05 | subtotal=59.99
| C05 | tax=5.7
| C05 | total=65.69
| C05 | running_total=377.84
| C05 | Wrote valid record to DuckDB:
| C05 |   order=d77ed935-9b28-431d-bc25-0a3fd98f4154
| C05 | MESSAGE ACCEPTED
| C05 | order=d77ed935-9b28-431d-bc25-0a3fd98f4154
| C05 | total=$65.69
| C05 | consumed=4
| C05 | RUNNING STATS
| C05 | total_sales=$377.84
| C05 | average=$94.46
| C05 | min=$54.11
| C05 | max=$194.82
| C05 | {'currency_code': 'USD', 'customer_id': 'CUST-8573', 'customer_note': 'Learning at my own pace', 'datetime': '2026-05-04T08:41:00Z', 'device_type': 'mobile', 'discount_code': '', 'is_new_customer': 'false', 'is_online': 'true', 'order_id': '24b91995-25f8-4ad7-8fa6-2e4c15750e5e', 'payment_method': 'apple_pay', 'product_id': 'PY-VIZ-003', 'quantity': '1', 'referral_source': 'paid_search', 'region_id': 'US-MO', 'unit_price': '39.99', '_kafka_key': 'US-MO', '_kafka_partition': 0, '_kafka_offset': 4}
| C05 | subtotal=39.99
| C05 | tax=3.2
| C05 | total=43.19
| C05 | running_total=421.03
| C05 | Wrote valid record to DuckDB:
| C05 |   order=24b91995-25f8-4ad7-8fa6-2e4c15750e5e
| C05 | MESSAGE ACCEPTED
| C05 | order=24b91995-25f8-4ad7-8fa6-2e4c15750e5e
| C05 | total=$43.19
| C05 | consumed=5
| C05 | RUNNING STATS
| C05 | total_sales=$421.03
| C05 | average=$84.21
| C05 | min=$43.19
| C05 | max=$194.82
| C05 | {'currency_code': 'CAD', 'customer_id': 'CUST-8062', 'customer_note': '', 'datetime': '2026-05-04T08:54:00Z', 'device_type': 'tablet', 'discount_code': '', 'is_new_customer': 'false', 'is_online': 'true', 'order_id': '1ec0fc8b-998c-4f43-8a66-19e1fa7d5fa8', 'payment_method': 'paypal', 'product_id': 'PY-SQL-004', 'quantity': '1', 'referral_source': 'organic', 'region_id': 'CA-ON', 'unit_price': '44.99', '_kafka_key': 'CA-ON', '_kafka_partition': 0, '_kafka_offset': 5}
| C05 | subtotal=44.99
| C05 | tax=5.85
| C05 | total=50.84
| C05 | running_total=471.87
| C05 | Wrote valid record to DuckDB:
| C05 |   order=1ec0fc8b-998c-4f43-8a66-19e1fa7d5fa8
| C05 | MESSAGE ACCEPTED
| C05 | order=1ec0fc8b-998c-4f43-8a66-19e1fa7d5fa8
| C05 | total=$50.84
| C05 | consumed=6
| C05 | RUNNING STATS
| C05 | total_sales=$471.87
| C05 | average=$78.64
| C05 | min=$43.19
| C05 | max=$194.82
| C05 | {'currency_code': 'CAD', 'customer_id': 'CUST-9348', 'customer_note': 'For my study group', 'datetime': '2026-05-04T09:07:00Z', 'device_type': 'desktop', 'discount_code': '', 'is_new_customer': 'false', 'is_online': 'false', 'order_id': '392dd49b-b385-499c-9b4a-a38387fa39ae', 'payment_method': 'credit_card', 'product_id': 'PY-INTRO-001', 'quantity': '1', 'referral_source': 'paid_search', 'region_id': 'CA-ON', 'unit_price': '29.99', '_kafka_key': 'CA-ON', '_kafka_partition': 0, '_kafka_offset': 6}
| C05 | subtotal=29.99
| C05 | tax=3.9
| C05 | total=33.89
| C05 | running_total=505.76
| C05 | Wrote valid record to DuckDB:
| C05 |   order=392dd49b-b385-499c-9b4a-a38387fa39ae
| C05 | MESSAGE ACCEPTED
| C05 | order=392dd49b-b385-499c-9b4a-a38387fa39ae
| C05 | total=$33.89
| C05 | consumed=7
| C05 | RUNNING STATS
| C05 | total_sales=$505.76
| C05 | average=$72.25
| C05 | min=$33.89
| C05 | max=$194.82
| C05 | {'currency_code': 'USD', 'customer_id': 'CUST-5889', 'customer_note': '', 'datetime': '2026-05-04T09:15:00Z', 'device_type': 'tablet', 'discount_code': '', 'is_new_customer': 'false', 'is_online': 'true', 'order_id': '02782ab7-dd7d-4ebe-8abc-ef81cdc4193b', 'payment_method': 'paypal', 'product_id': 'PY-VIZ-003', 'quantity': '1', 'referral_source': 'organic', 'region_id': 'US-CA', 'unit_price': '39.99', '_kafka_key': 'US-CA', '_kafka_partition': 0, '_kafka_offset': 7}
| C05 | subtotal=39.99
| C05 | tax=3.8
| C05 | total=43.79
| C05 | running_total=549.55
| C05 | Wrote valid record to DuckDB:
| C05 |   order=02782ab7-dd7d-4ebe-8abc-ef81cdc4193b
| C05 | MESSAGE ACCEPTED
| C05 | order=02782ab7-dd7d-4ebe-8abc-ef81cdc4193b
| C05 | total=$43.79
| C05 | consumed=8
| C05 | RUNNING STATS
| C05 | total_sales=$549.55
| C05 | average=$68.69
| C05 | min=$33.89
| C05 | max=$194.82
| C05 | {'currency_code': 'CAD', 'customer_id': 'CUST-4770', 'customer_note': 'Excellent content', 'datetime': '2026-05-04T09:19:00Z', 'device_type': 'desktop', 'discount_code': '', 'is_new_customer': 'false', 'is_online': 'true', 'order_id': '67bd3492-cc96-429a-9a91-1b4d2132d261', 'payment_method': 'credit_card', 'product_id': 'PY-VIZ-003', 'quantity': '1', 'referral_source': 'paid_search', 'region_id': 'CA-ON', 'unit_price': '39.99', '_kafka_key': 'CA-ON', '_kafka_partition': 0, '_kafka_offset': 8}
| C05 | subtotal=39.99
| C05 | tax=5.2
| C05 | total=45.19
| C05 | running_total=594.74
| C05 | Wrote valid record to DuckDB:
| C05 |   order=67bd3492-cc96-429a-9a91-1b4d2132d261
| C05 | MESSAGE ACCEPTED
| C05 | order=67bd3492-cc96-429a-9a91-1b4d2132d261
| C05 | total=$45.19
| C05 | consumed=9
| C05 | RUNNING STATS
| C05 | total_sales=$594.74
| C05 | average=$66.08
| C05 | min=$33.89
| C05 | max=$194.82
| C05 | {'currency_code': 'MXN', 'customer_id': 'CUST-6168', 'customer_note': '', 'datetime': '2026-05-04T09:37:00Z', 'device_type': 'desktop', 'discount_code': '', 'is_new_customer': 'false', 'is_online': 'true', 'order_id': '0a021628-6937-4876-8875-b00c07a39e0f', 'payment_method': 'paypal', 'product_id': 'PY-DATA-002', 'quantity': '1', 'referral_source': 'organic', 'region_id': 'MX-CMX', 'unit_price': '49.99', '_kafka_key': 'MX-CMX', '_kafka_partition': 0, '_kafka_offset': 9}
| C05 | subtotal=49.99
| C05 | tax=8.0
| C05 | total=57.99
| C05 | running_total=652.73
| C05 | Wrote valid record to DuckDB:
| C05 |   order=0a021628-6937-4876-8875-b00c07a39e0f
| C05 | MESSAGE ACCEPTED
| C05 | order=0a021628-6937-4876-8875-b00c07a39e0f
| C05 | total=$57.99
| C05 | consumed=10
| C05 | RUNNING STATS
| C05 | total_sales=$652.73
| C05 | average=$65.27
| C05 | min=$33.89
| C05 | max=$194.82
| C05 | No message received within 10.0s timeout.
| C05 | Producer finished or paused. Stopping consumer.
| C05 | Kafka consumer closed.
| C05 | Saving artifacts...
| C05 | WROTE OUTPUT_CSV = data/output/consumed_sales_sailorsP5.csv
| C05 | WROTE OUTPUT_DB = data/output/sales_sailorsP5.duckdb
| C05 | ========================
| C05 | SECTION E. Exit
| C05 | ========================
| C05 | Summary:
| C05 | Consumed 10 message(s) from topic 'streaming-05-storage-case'.
| C05 | Skipped  0 message(s).
| C05 | OUTPUT_CSV = data/output/consumed_sales_sailorsP5.csv
| C05 |   Total sales:  $652.73
| C05 |   Average sale: $65.27
| C05 |   Minimum sale: $33.89
| C05 |   Maximum sale: $194.82
| C05 | ========================
| C05 | Consumer executed successfully!
| C05 | ========================
```
