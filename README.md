# Erasure Coding Simulator

An interactive single-page simulator that demonstrates how erasure coding works in distributed cloud storage systems.

This project was built as a student course presentation and visualizes how data is:

- split into data shards
- converted into ASCII values
- encoded with parity using `GF(2^8)` arithmetic
- distributed across multiple servers
- recovered after one or two server failures

## Overview

The simulator walks through the core ideas behind erasure coding in a step-by-step visual format:

1. A 4-letter input word is converted into four data values.
2. Two parity values are generated using a fixed generator matrix.
3. The six total pieces are distributed across six simulated cloud servers.
4. The user can destroy up to two servers.
5. The app reconstructs lost information using XOR or matrix inversion over `GF(2^8)`.

The goal is educational: to make the mathematics behind object storage systems easier to understand through animation and interaction.

## Features

- Interactive browser-based UI with no build step
- Visual explanation of XOR and Galois Field math
- Encoding using a generator matrix
- Simulation of data and parity distribution across 6 servers
- Failure simulation for up to 2 lost nodes
- Recovery flow for:
  - 1 lost data or parity shard
  - 2 lost shards using matrix inversion
- Clean, presentation-friendly interface built with Tailwind CSS

## How It Works

### Data Layout

The simulator uses:

- `4` data shards
- `2` parity shards
- `6` total storage nodes

Input is restricted to exactly `4` characters. Each character is converted to its ASCII value and treated as one data shard.

### Generator Matrix

The encoding logic uses this generator matrix:

```text
[1 0 0 0]
[0 1 0 0]
[0 0 1 0]
[0 0 0 1]
[1 1 1 1]
[1 2 3 4]
```

The first four rows represent the original data shards. The last two rows generate parity shards:

- `P1 = D1 XOR D2 XOR D3 XOR D4`
- `P2 = (1*D1) XOR (2*D2) XOR (3*D3) XOR (4*D4)` in `GF(2^8)`

### Recovery Logic

- If one shard is lost, the simulator rebuilds it using the surviving shards and parity.
- If two shards are lost, the simulator selects four surviving rows, builds a reduced matrix, inverts it, and solves for the original data vector.
- If a third shard is destroyed, recovery is no longer possible in this model.

## Tech Stack

- `HTML`
- `JavaScript`
- `Tailwind CSS` via CDN
- `Font Awesome` via CDN
- Google Fonts via CDN

Everything runs directly in the browser. No backend or package manager is required.

## Running Locally

### Option 1: Open directly

Open `index.html` in any modern web browser.

### Option 2: Use a local static server

If you prefer running it from a local server, you can use any static file server.

Examples:

```bash
python -m http.server
```

or

```bash
npx serve .
```

Then open the local URL shown in your terminal.

## Using the Simulator

1. Enter a 4-letter word.
2. Click `Generate Math`.
3. Review the generated data and parity values.
4. Click `Send to Cloud Servers`.
5. Click one or two server cards to simulate failures.
6. Click `Execute Matrix Recovery`.
7. Review the math explanation and restored output.

## Project Structure

```text
.
|-- index.html   # Full application UI, styling, and JavaScript logic
|-- README.md    # Project documentation
```

## Educational Focus

This project is designed to explain concepts, not to serve as a production erasure coding library. The implementation is intentionally visual and compact so the encoding and recovery process can be followed in a classroom or demo setting.

## Limitations

- Accepts exactly `4` characters as input
- Uses a fixed `4+2` erasure coding layout
- Keeps all logic in a single HTML file
- Depends on external CDNs for styling and icons
- Focuses on conceptual clarity over production-grade performance or extensibility

## Credits

Course project presented by:

- Anshit Sinha
- Anushttha 
- Apoorv Sisodia
- Nongmaithem Zeno


---

Case study: [anshit.is-a.dev/works/erasure-coding](https://anshit.is-a.dev/works/erasure-coding) · Built by [Anshit Sinha](https://anshit.is-a.dev)
