# Using Testnet Guide

last updated for testnet2

---

## Why Use Testnet?

The testnet is a wonderful place where you can experiment with the Coolsnady applications without worrying that a mistake will cost you real money. It is actually recommended that people use the testnet to learn the basics of the Coolsnady software and any new features.

Coolsnady is currently on its 2nd Testnet, also known as testnet2. Testnets are periodically reset to help keep a manageable blockchain file size. 

---

## How to Run a Testnet Node

Running a testnet2 node is incredibly easy. You application of choice will need to download the testnet2 blockchain, and you will need to create a new wallet file for testnet2 use. Your mainnet blockchain and wallet files will remain untouched. Switching between the two is incredibly easy.

---

## hcgui 

You can switch hcgui between mainnet and testnet by editing the config.json file and setting network: testnet. Config.json can be located in the following folders:

| OS      | hcgui data directory                           |
| -------:|:---------------------------------------------------:|
| Windows | `%LOCALAPPDATA%\hcgui`                         |
| macOS   | `~/Library/Application Support/hcgui`          |
| Linux   | `~/.config/hcgui`                              |

---

## Command-Line Suite

To launch `hcd` and `hcwallet` on testnet, simply add the `--testnet` flag to your launch command. Alternatively, you could put `testnet=1` in all of your config files, but it's usually much faster to use the startup flag.

On the first launch of `hcd --testnet`, `hcd` will begin downloading the testnet2 blockchain to the `data/testnet2` folder of `hcd`'s home directory.

Before you're able to launch `hcwallet` with the `--testnet` flag, you must create a separate testnet wallet using the `hcwallet --testnet --create` command. The steps are the same as those found in the [hcwallet Setup Guide](/getting-started/user-guides/hcwallet-setup.md). 

To issue commands to both `hcwallet` and `hcd`, you must also add the `--testnet` flag to any of the `hcctl` commands that you use. E.g. you would issue the `hcctl --testnet --wallet getbalance` command to check your testnet balance. 

---

## Acquiring Testnet Coins

You can acquire coins through the [Coolsnady Testnet Faucet](https://faucet.Coolsnady.org). Please return any coins to the address listed at the bottom of that page when you're done playing with the testnet.

---

