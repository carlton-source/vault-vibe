# VaultVibe: Intelligent DeFi Portfolio Manager

[![Stacks](https://img.shields.io/badge/Stacks-2.0-blue.svg)](https://stacks.co/)
[![Clarity](https://img.shields.io/badge/Clarity-Smart%20Contract-orange.svg)](https://clarity-lang.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## Overview

VaultVibe is a next-generation decentralized finance platform built on the Stacks blockchain that automatically optimizes yield farming strategies across multiple protocols while maintaining risk-adjusted returns for Bitcoin-based digital assets. The platform leverages algorithmic portfolio rebalancing and dynamic allocation strategies to maximize earnings while providing users with transparent, secure, and efficient yield optimization services.

## Key Features

### 🚀 Core Functionality

- **Multi-protocol Yield Aggregation**: Seamlessly distribute assets across multiple DeFi protocols
- **Automated Risk Management**: Intelligent rebalancing based on market conditions
- **Dynamic Fee Optimization**: Adaptive fee structures to maximize user returns
- **Emergency Protection Mechanisms**: Circuit breakers and emergency shutdown capabilities
- **Institutional-grade Security**: Comprehensive validation and authorization systems

### 🔧 Technical Features

- **SIP-010 Token Compatibility**: Full support for Stacks token standard
- **Whitelisted Token System**: Secure token validation and approval process
- **Real-time Yield Calculation**: Block-based reward calculations
- **Portfolio Optimization**: Advanced allocation algorithms
- **Transparent Operations**: All functions are auditable on-chain

## Architecture

### Smart Contract Components

#### Core Data Structures

- **User Deposits**: Track individual user investments and deposit history
- **Protocol Registry**: Manage supported yield farming protocols
- **Strategy Allocations**: Define and update allocation percentages
- **Token Whitelist**: Maintain approved token contracts
- **Rewards Tracking**: Monitor pending and claimed rewards

#### Key Functions

##### Protocol Management

```clarity
(define-public (add-protocol (protocol-id uint) (name (string-ascii 64)) (initial-apy uint)))
(define-public (update-protocol-status (protocol-id uint) (active bool)))
(define-public (update-protocol-apy (protocol-id uint) (new-apy uint)))
```

##### Asset Management

```clarity
(define-public (deposit (token-trait <sip-010-trait>) (amount uint)))
(define-public (withdraw (token-trait <sip-010-trait>) (amount uint)))
(define-public (claim-rewards (token-trait <sip-010-trait>)))
```

##### Administrative Controls

```clarity
(define-public (set-platform-fee (new-fee uint)))
(define-public (set-emergency-shutdown (shutdown bool)))
(define-public (whitelist-token (token principal)))
```

## Installation & Setup

### Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) - Clarity development environment
- [Stacks CLI](https://docs.stacks.co/docs/write-smart-contracts/cli-wallet-quickstart) - Command line interface
- Node.js 16+ (for testing framework)

### Local Development

1. **Clone the repository**

   ```bash
   git clone https://github.com/carlton-source/vault-vibe.git
   cd vault-vibe
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Run contract validation**

   ```bash
   clarinet check
   ```

4. **Execute test suite**

   ```bash
   npm test
   ```

### Contract Deployment

#### Testnet Deployment

```bash
clarinet deploy --testnet
```

#### Mainnet Deployment

```bash
clarinet deploy --mainnet
```

## Usage Examples

### For Users

#### Depositing Assets

```clarity
;; Deposit 1000000 satoshis (0.01 BTC equivalent)
(contract-call? .vault-vibe deposit .sbtc-token u1000000)
```

#### Withdrawing Assets

```clarity
;; Withdraw 500000 satoshis
(contract-call? .vault-vibe withdraw .sbtc-token u500000)
```

#### Claiming Rewards

```clarity
;; Claim accumulated yield rewards
(contract-call? .vault-vibe claim-rewards .sbtc-token)
```

### For Administrators

#### Adding New Protocol

```clarity
;; Add a new yield protocol with 5% APY
(contract-call? .vault-vibe add-protocol u6 "Arkadiko Vault" u500)
```

#### Updating Platform Fee

```clarity
;; Set platform fee to 1.5% (150 basis points)
(contract-call? .vault-vibe set-platform-fee u150)
```

#### Token Whitelisting

```clarity
;; Whitelist a new token contract
(contract-call? .vault-vibe whitelist-token 'SP1H1733V5MZ3SZ9XRW9FKYGEZT0JDGEB8Y634C7R.sbtc-token)
```

## Configuration

### Environment Variables

- `MIN_DEPOSIT`: Minimum deposit amount (default: 100,000 satoshis)
- `MAX_DEPOSIT`: Maximum deposit amount (default: 1,000,000,000 satoshis)
- `PLATFORM_FEE`: Platform fee rate in basis points (default: 100 = 1%)

### System Limits

- **Maximum Protocol ID**: 100
- **Maximum APY**: 10,000 basis points (100%)
- **Platform Fee Cap**: 1,000 basis points (10%)

## Security Features

### Access Control

- **Contract Owner Privileges**: Only contract owner can manage protocols and system settings
- **Token Validation**: All tokens must be whitelisted before use
- **Emergency Shutdown**: Circuit breaker functionality for emergency situations

### Risk Management

- **Deposit Limits**: Minimum and maximum deposit constraints
- **APY Validation**: Reasonable bounds on protocol APY rates
- **Total Allocation Checks**: Ensures allocations don't exceed 100%

### Audit Considerations

- All public functions include comprehensive validation
- State changes are atomic and reversible
- Clear separation between user and administrative functions

## Testing

### Running Tests

```bash
# Run all tests
npm test

# Run specific test file
npm test -- vault-vibe.test.ts

# Run with coverage
npm run test:coverage
```

### Test Categories

- **Unit Tests**: Individual function validation
- **Integration Tests**: Multi-function workflow testing
- **Security Tests**: Access control and validation testing
- **Edge Cases**: Boundary condition testing

## API Reference

### Read-Only Functions

#### `(get-protocol (protocol-id uint))`

Returns protocol information including name, status, and APY.

#### `(get-user-deposit (user principal))`

Returns user's deposit amount and last deposit block.

#### `(get-total-tvl)`

Returns the total value locked in the platform.

#### `(is-whitelisted (token <sip-010-trait>))`

Checks if a token is whitelisted for use.

### Public Functions

#### Deposit Functions

- `deposit`: Add assets to the platform
- `withdraw`: Remove assets from the platform
- `claim-rewards`: Claim accumulated yield

#### Administrative Functions

- `add-protocol`: Register new yield protocol
- `update-protocol-status`: Enable/disable protocols
- `update-protocol-apy`: Update protocol yield rates
- `set-platform-fee`: Adjust platform fees
- `whitelist-token`: Approve tokens for use

## Error Codes

| Code | Constant | Description |
|------|----------|-------------|
| 1000 | ERR-NOT-AUTHORIZED | Caller lacks required permissions |
| 1001 | ERR-INVALID-AMOUNT | Amount is outside valid range |
| 1002 | ERR-INSUFFICIENT-BALANCE | Insufficient user balance |
| 1003 | ERR-PROTOCOL-NOT-WHITELISTED | Protocol not approved |
| 1004 | ERR-STRATEGY-DISABLED | Strategy or system disabled |
| 1005 | ERR-MAX-DEPOSIT-REACHED | Deposit exceeds maximum limit |
| 1006 | ERR-MIN-DEPOSIT-NOT-MET | Deposit below minimum requirement |
| 1007 | ERR-INVALID-PROTOCOL-ID | Protocol ID out of range |
| 1008 | ERR-PROTOCOL-EXISTS | Protocol already registered |
| 1009 | ERR-INVALID-APY | APY outside valid range |
| 1010 | ERR-INVALID-NAME | Protocol name invalid |
| 1011 | ERR-INVALID-TOKEN | Token contract invalid |
| 1012 | ERR-TOKEN-NOT-WHITELISTED | Token not approved for use |

## Contributing

### Development Workflow

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Add tests for new functionality
5. Ensure all tests pass (`npm test`)
6. Commit your changes (`git commit -m 'Add amazing feature'`)
7. Push to the branch (`git push origin feature/amazing-feature`)
8. Open a Pull Request

### Code Standards

- Follow Clarity best practices
- Include comprehensive comments
- Add unit tests for new functions
- Maintain backwards compatibility
- Document API changes

## Roadmap

### Phase 1: Core Implementation ✅

- [x] Basic deposit/withdrawal functionality
- [x] Protocol management system
- [x] Token whitelisting
- [x] Reward calculation engine

### Phase 2: Advanced Features 🚧

- [ ] Multi-token portfolio support
- [ ] Advanced rebalancing algorithms
- [ ] Governance token integration
- [ ] Flash loan protection

### Phase 3: Enterprise Features 📋

- [ ] Institutional vaults
- [ ] Custom strategy builder
- [ ] Cross-chain bridge integration
- [ ] Automated market making
