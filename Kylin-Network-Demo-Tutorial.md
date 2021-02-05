# Preparing the Runtime Environment
## Installing the rust environment

**Kylin network** is developed based on Substrate, so the Rust language is chosen. Before running the node, you need to install the relevant environment.

The recommendation is to use rustup to install and manage the rust language. For MacOS or Linux/Unix systems, you can use the following method to install.

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

For Windows system, you need to download the offline installation package to install it (ref: https://forge.rust-lang.org/infra/other-installation-methods.html). The installation package is
https://static.rust-lang.org/rustup/dist/i686-pc-windows-gnu/rustup-init.exe


In addition to installing the Rust environment, you also need to prepare the wasm environment. After the rustup installation is complete, the following command can be used to install the required wasm toolchain.

```
rustup target add wasm32-unknown-unknown --toolchain nightly
rustup target add wasm32-unknown-unknown --toolchain nightly-2020-10-05
```
## Install the nodejs environment

The front-end is written in React and requires a nodejs and yarn environment to run.
Download the installation package for the corresponding system from the nodejs website (https://nodejs.org/en/download/)
Install yarn correctly according to the documentation (https://classic.yarnpkg.com/en/docs/install) and make sure it works correctly.

## Install the contract development environment

After installing the Rust environment, you need to install some dependencies to prepare for contract development.

```
rustup component add rust-src --toolchain nightly
rustup target add wasm32-unknown-unknown --toolchain stable
```

Continue installing the [ink! CLI](https://substrate.dev/substrate-contracts-workshop/#/0/setup?id=ink-cli). The final tool we will be installing is the ink! command line utility which will make setting up Substrate smart contract projects easier.
You can install the utility using Cargo with:

```
cargo install cargo-contract --vers 0.8.0 --force --locked
```

# Download and compile

The **Kylin Network** project consists of node, module, data proxy, and front-end. The steps to install and run them are described below.

## Compiling nodes and modules
First, download the node project from the official repository of **Kylin Network**

```
$ git clone --recursive https://github.com/Kylin-Network/kylin-node.git 
```
Currently, the project is managed using submodule, so the above command will download the full code and modules.
Once the download is complete, go into the directory and compile.

```
$ cd kylin-node
$ cargo build
```

Cargo will automatically resolve dependencies based on the project's Cargo.lock.

After the compilation is complete, the runnable binaries will be generated in the `target/debug` directory

# Compiling the front-end

Download the front-end project from the official **Kylin Network** repository

```
$ git clone https://github.com/Kylin-Network/kylin-market-frontend.git
```

Install the dependencies using yarn

```
cd kylin-market-frontend
yarn install
```

## Compiling the data proxy

The data proxy is used to collect data from third-party services and provide a uniform interface and controlled output format for OCW modules to call. The data proxy is currently written in Rust, but can be rebuilt in another framework or language to ensure a consistent interface.

```
$ git clone https://github.com/Kylin-Network/sample-data-fetcher.git
cd sample-data-fetcher
cargo build
```

## Compiling contracts

Kylin's contracts are written in ink!and include a marketplace service index contract and some test demos. Compile the contracts by following these steps.

```
git clone https://github.com/Kylin-Network/kylin-contracts.git
cd kylin-contracts/oracle_market
cargo +nightly contract build
```

## Docker
Kylin also provides Docker to compile node and modules, front end and data proxy.

### Dockfile

```
docker build -f Dockerfile -t kylin-node .
```

### Tutorial

For more details, please see the [Kylin Network Demo Docker Tutorial]()




