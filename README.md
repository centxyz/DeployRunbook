# DeployRunbook

[![CI](https://github.com/centxyz/DeployRunbook/actions/workflows/ci.yml/badge.svg)](https://github.com/centxyz/DeployRunbook/actions/workflows/ci.yml)

DeployRunbook is a browser deployment runbook for Solidity contracts. It compiles source through [SolcQueue](https://github.com/centxyz/SolcQueue), encodes typed constructor arguments, connects to an injected EIP-1193 wallet, estimates gas, requests deployment, waits for the transaction receipt, and verifies runtime bytecode at the resulting address.

Private keys never enter the application. The connected wallet performs approval and signing. Verified deployments are stored only in browser local storage.

## Run

Start SolcQueue on port 3000 in a separate terminal:

```bash
git clone https://github.com/centxyz/SolcQueue.git
cd SolcQueue
npm install
npm start
```

Then start the browser application:

```bash
git clone https://github.com/centxyz/DeployRunbook.git
cd DeployRunbook
npm install
npm run dev
```

Open the displayed URL in a browser with an injected wallet such as MetaMask. Choose the intended wallet network before deployment.

The default compiler URL is `http://localhost:3000`. Set it in the interface if SolcQueue runs elsewhere. Compilation can be tested without connecting a wallet; signing and deployment require an injected wallet. The compiler API retains its legacy `SolidityStackDiamond` service identifier for compatibility.

## Workflow

1. Enter source, filename, and target contract name.
2. Compile and review the selected artifact.
3. Enter constructor values as a JSON array in ABI order.
4. Connect the wallet and approve the deployment transaction.
5. DeployRunbook waits for the receipt and confirms deployed bytecode with `eth_getCode`.

## Verify

```bash
npm test
npm run build
```

Tests cover validation, dynamic constructor encoding, compiler integration, wallet connection, gas estimation, submission, receipt polling, bytecode verification, and deployment history records.

## Security boundaries

- DeployRunbook does not audit source code or guarantee contract safety.
- Compiler API and wallet network selection are independent; verify both before signing.
- A verified deployment means runtime bytecode exists at the receipt address, not that the source is secure or externally verified.

## License

MIT © cent

## Current limitations

- Deployment requires a compatible compiler service, an injected wallet, a funded account, and a suitable EVM network.
- Runtime bytecode presence is a basic verification check, not source-code verification or a security audit.
- Gas estimates, wallet prompts, and receipts depend on the connected provider and live chain state.
