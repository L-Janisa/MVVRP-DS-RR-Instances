# MVVRP-DS&RR benchmark instances
This repository provides 44 benchmark instances for the multi-visit vehicle routing problem with drone stations and resource rebalancing (MVVRP-DS&RR). Node coordinates are adapted from the Solomon (1987) vehicle routing benchmark dataset. The instances are organized into four folders, each containing 11 text files:

- `Instances/CUT_10/`: `10.01.txt` to `10.11.txt`
- `Instances/CUT_25/`: `25.01.txt` to `25.11.txt`
- `Instances/CUT_50/`: `50.01.txt` to `50.11.txt`
- `Instances/CUT_100/`: `100.01.txt` to `100.11.txt`

## File format

Each `.txt` file contains a header followed by one record per node, with five whitespace-separated columns:

| Column | Description |
| --- | --- |
| `CUST NO.` | Unique node identifier; node `0` is the central depot. |
| `XCOORD.` | X-coordinate on a 100 × 100 coordinate grid. |
| `YCOORD.` | Y-coordinate on a 100 × 100 coordinate grid. |
| `DEMAND` | Signed net inventory `q_i` at a customer: positive for surplus resources available for pickup and negative for shortages requiring delivery. This field is `0` at drone stations. At the central depot (node `0`), it specifies the baseline initial inventory `H`, rather than customer demand. |
| `TYPE` | Node category, as defined below. |

### Node types

| `TYPE` | Description |
| --- | --- |
| `0` | Central depot. |
| `1` | Candidate fixed drone station; activation is determined by the optimization model. |
| `2` | Truck-only customer under the baseline drone payload and endurance limits. |
| `3` | Drone-eligible customer that may be served by either a truck or a station-based drone. |

The baseline drone payload capacity is 30 weight units, and the flight endurance is 60 min. Customer service eligibility (`TYPE = 2` or `3`) is specified for these baseline conditions.

For every instance, the baseline depot inventory satisfies `H = -sum(q_i)`, where the sum is taken over customer nodes only.

## Distances and travel times

Each coordinate unit represents **1 km**. Both trucks and drones use Euclidean distances between nodes:

$$
d_{ij} = \sqrt{(x_i-x_j)^2 + (y_i-y_j)^2}.
$$

Travel time, in minutes, is calculated as

$$
t_{ij} = \frac{60d_{ij}}{v},
$$

where `v` is the travel speed in km/h: **30 km/h for trucks** and **48 km/h for drones**. No road-network distance correction is applied.
