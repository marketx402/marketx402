# Market x402 Protocol

One line of code to access any API. No keys, no limits, no gatekeepers.

```
app.use(
  // Define how much you want to charge, and where you want the funds to land
  marketMiddleware("YourWalletAddress", { "/your-endpoint": "$0.01" })
);
```

That’s it! See `examples/typescript/servers/express.ts` for a complete implementation example. Instructions for running on Base-Sepolia are below.

## Philosophy

APIs are the lifeblood of the internet — yet today, access to them is trapped behind paywalls, billing dashboards, and API keys that must be managed manually. This architecture was never designed for autonomous AI systems.

Developers and agents must constantly:

- Create accounts
- Manage billing setups
- Store API keys in `.env` files

It’s repetitive, high-friction, and ultimately prevents AI systems from ever being truly independent.

Market x402 fixes this.

We believe in a world where any AI agent can call any API instantly, without manual setup or permission walls — enabling fully autonomous systems to transact and build in real time.

## Principles

- Open Standard: The Market x402 protocol will never rely on a single entity or provider.
- HTTP Native: Designed to work seamlessly with the existing web stack — no extra SDKs or complex requests.
- Chain & Token Agnostic: Market x402 welcomes contributions for new chains and signing standards, so long as they meet our published acceptance criteria.
- Trust-Minimized: Payments and requests should always honor user intent — facilitators and servers can never move funds outside intended flows.
- AI-First Design: Abstraction is everything. Developers and AI clients shouldn’t have to worry about gas, RPCs, or wallets. Market x402 handles the plumbing — you focus on logic.

## Ecosystem

The Market x402 ecosystem is rapidly expanding. Explore the growing list of projects and services powered by x402:

- AI & Agent Integrations
- Third-party API Market Endpoints
- Infrastructure & Developer Tooling
- Learning and Community Resources

Want to add your project? See our demo README for submission instructions.

Roadmap: see `ROADMAP.md`.

## Core Terms

Term | Description
--- | ---
Resource | Any internet-accessible endpoint (API, RPC, file server, etc.)
Client | Any agent or user requesting and paying for a resource
Facilitator Server | The verifier/settler that handles payment authentication and blockchain interaction
Resource Server | The HTTP server providing the requested resource to the client

## Technical Goals

- Fully permissionless and secure for both clients and servers
- Gasless transactions for both sides
- One-line server integration
- Flexible speed/guarantee tradeoffs
- Extensible payment and chain support

## V1 Protocol Overview

The x402 protocol defines a universal standard for API payments over HTTP, built on the existing HTTP 402 (Payment Required) status code.

It defines:

- How servers declare pricing and payment requirements
- How clients send payment headers (`X-PAYMENT`)
- How servers verify, settle, and respond
- How facilitators confirm and settle on-chain transactions

### Payment Flow

1. Client → Resource Server: Request a resource.
2. Server → Client: Responds with `402 Payment Required` and `PaymentRequirements`.
3. Client → Server: Sends request with `X-PAYMENT` header (encoded payment payload).
4. Server → Facilitator: Optionally verifies payment via `/verify`.
5. Facilitator → Blockchain: Confirms and settles transaction.
6. Server → Client: Returns resource + `X-PAYMENT-RESPONSE` header confirming settlement.

## Type Specifications

### Payment Required Response

```
{
  "x402Version": 1,
  "accepts": [paymentRequirements],
  "error": "optional error message"
}
```

### paymentRequirements

```
{
  "scheme": "exact",
  "network": "solana",
  "maxAmountRequired": "0.01",
  "resource": "/api/data",
  "description": "Access API Data",
  "mimeType": "application/json",
  "payTo": "YourWalletAddress",
  "maxTimeoutSeconds": 10,
  "asset": "USDC",
  "extra": null
}
```

### Payment Payload

```
{
  "x402Version": 1,
  "scheme": "exact",
  "network": "solana",
  "payload": {}
}
```

## Facilitator API

See the `examples` directory for facilitator patterns and the `specs` for request/response shapes. Implementations for multiple languages live under `typescript/`, `python/`, `go/`, and `java/`.

## License

Licensed under the [Apache License, Version 2.0](LICENSE). See `NOTICE` for required attribution details.
