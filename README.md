# rainbowkit-connector

## How to use

### Install package

`yarn add bybit-rainbowkit-connector`

### Use it in your code

```TSX
import {
  getDefaultWallets,
  connectorsForWallets,
  RainbowKitProvider,
  ConnectButton
} from "@rainbow-me/rainbowkit";
import { configureChains, createConfig, WagmiConfig } from "wagmi";
import { polygon, optimism, arbitrum, bsc, mainnet } from "wagmi/chains";
import { publicProvider } from 'wagmi/providers/public';
import { alchemyProvider } from 'wagmi/providers/alchemy';
import { bybitWallet } from 'bybit-rainbowkit-connector';

const { chains, publicClient, webSocketPublicClient } = configureChains(
  [polygon, optimism, arbitrum, bsc, mainnet],
  [alchemyProvider({ apiKey: process.env.ALCHEMY_ID || "" }), publicProvider()]
);

const { wallets } = getDefaultWallets({
  appName: "My RainbowKit App",
  projectId: "YOUR_PROJECT_ID",
  chains
});

const connectors = connectorsForWallets([
  ...wallets,
  {
    groupName: "Other",
    wallets: [
      bybitWallet(), // add BybitWallet
    ]
  }
]);

const wagmiConfig = createConfig({
  autoConnect: true,
  connectors,
  publicClient,
  webSocketPublicClient,
});

export const App = () => {
  return (
    <WagmiConfig client={wagmiConfig}>
      <RainbowKitProvider chains={chains}>
        <ConnectButton />
      </RainbowKitProvider>
    </WagmiConfig>
  );
};

```
