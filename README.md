# Waku - Lite Protocol Tester runner

NWaku Lite-protocol-tester runner that intends to ease the process of running tester applications on different conditions.
It uses docker compose to set up the environment for lite-protocol-tester with dashboard addon.

## Lite Protocol Tester

This tool is settled in nwaku repository under `apps/liteprotocoltester` directory.
https://github.com/waku-org/nwaku/tree/master/apps/liteprotocoltester

Tool is available as docker image at `wakuorg/liteprotocoltester:latest`.

The aim is to test selected waku network's reliability on message delivery, specifically message delivery using lightpush and filter services.

## Usage

```bash
git clone https://github.com/waku-org/lpt-runner.git
cd lpt-runner

# check Reame.md for more information
# edit .env file to your needs

docker compose up -d

# navigate localhost:3033 to see the lite-protocol-tester dashboard
```

#### Test monitoring

Navigate to http://localhost:3033 to see the lite-protocol-tester dashboard.

## Configuration

### Environment variables for docker compose runs

|   Variable     | Description | Default |
| ---: | :--- | :--- |
| NUM_PUBLISHER_NODES | Number of publisher node copies to run - this can extend stressful testing of the network | 1 |
| NUM_RECEIVER_NODES | Number of receiver node copies to run - to widen reliability test of the network | 1 |
| NUM_MESSAGES   | Number of message to publish, 0 means infinite | 120 |
| DELAY_MESSAGES | Frequency of messages in milliseconds | 1000 |
| PUBSUB | Used pubsub_topic for testing | /waku/2/rs/66/0 |
| CONTENT_TOPIC  | content_topic for testing | /tester/1/light-pubsub-example/proto |
| CLUSTER_ID  | cluster_id of the network | 16 |
| START_PUBLISHING_AFTER | Delay in seconds before starting to publish to let service node connected | 5 |
| MIN_MESSAGE_SIZE | Minimum message size in bytes | 1KiB |
| MAX_MESSAGE_SIZE | Maximum message size in bytes | 120KiB |
| LIGHTPUSH_SERVICE_PEER | Lightpush service node's address |  |
| FILTER_SERVICE_PEER | Filter service node's adress |  |
| LIGHTPUSH_BOOTSTRAP | Alternative to directly specify service peer, bootstrap node is used to gather possible lightpush peer randomly from the network. |  |
| FILTER_BOOTSTRAP | Alternative to directly specify service peer, bootstrap node is used to gather possible filter peer randomly from the network. |  |

### Specifying peer addresses

Service node or bootstrap addresses can be specified in multiadress or ENR form.

### Using bootstrap nodes

There are multiple benefits of using bootstrap nodes. By using them liteprotocoltester will use Peer Exchange protocol to get possible peers from the network that are capable to serve as service peers for testing. Additionally it will test dial them to verify their connectivity - this will be reported in the logs and on dashboard metrics.
Also by using bootstrap node and peer exchange discovery, litprotocoltester will be able to simulate service peer switch in case of failures. There are built in tresholds for service peer failures during test and service peer can be switched during the test. Also these service peer failures are reported, thus extening network reliability measures.
