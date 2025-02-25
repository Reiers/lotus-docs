---
title: "Set up"
description: "The process of storing and retrieving data using the Filecoin network is slightly different from how most storage platforms work. This tutorial walks you through the whole end-to-end process of keeping your data and then getting it back when you need it! This tutorial should take you about an hour to complete."
lead: "The process of storing and retrieving data using the Filecoin network is slightly different from how most storage platforms work. This tutorial walks you through the whole end-to-end process of keeping your data and then getting it back when you need it! This tutorial should take you about an hour to complete."
draft: false
menu:
    tutorials:
        parent: "tutorials-store-and-retrieve"
aliases:
    - /docs/tutorials/store-and-retrieve/
weight: 110
toc: true
---

## Before we start

The process is split into three main parts: the set-up, storing your data and retrieving your data. Each section has several sub-processes that we need to follow.

| Section | Sub-tasks |
| --- | --- |
| Set up | 1. Get access to a Lotus full-node.<br> 2. Start a Lotus lite-node on your local computer.<br> 3. Get a FIL address.<br> 4. Sign up for Filecoin Plus. |
| Store data | 1. Package your data.<br> 2. Import your data into Lotus.<br> 3. Find a storage provider through the Filecoin Plus Registry.<br> 4. Create a storage deal.<br> 5. Wait for the deal to complete. |
| Retrieve data | 1. Create a retrieval deal.<br> 2. Download your data.|

It will take about an hour to complete this tutorial. While there aren't too many steps involved, there's a bit of waiting around for the network to process your requests.

{{< alert icon="tip" >}}
This tutorial uses the Filecoin mainnet. Everything you'll see over the next hour is happening on a production network with other users storing and retrieving data. But don't worry, this tutorial won't cost you anything! It's just important to know that you'll be dealing with real storage providers, real data, and real transactions.
{{< /alert >}}

### Take notes

There are a few things to remember throughout this tutorial, such as Miner IDs and addresses. There is a table at the end of each section showing the information you should record:

| Variable | Description | Example |
| --- | --- | --- |
| Miner ID | The unique identifier for each storage provider. | `f01000`

The above table is an example of what you will see throughout the tutorial; you don't have to copy it down.

### Terms and phrases

This tutorial contains some words and phrases that you might not be familiar with. Refer back to this table if you encounter something you don't understand:

| Word | Definition |
| --- | --- |
| Address | A string of letters and numbers that other users can send FIL to. |
| Block explorer | A service, usually a website, that lets you view details of a blockchain such as transactions, deals, and addresses. |
| Deal | An agreement between two computers about what to do with some data. |
| FIL | The shorthand representation of the filecoin cryptocurrency. For example: _We charge 0.5 FIL per GiB._ |
| Filecoin (upper-case `F`) | The network that transactions and storage deals take place on. For example: _Museums can use the Filecoin network to store their digital archives._ |
| filecoin (lower-case `f`) | The cryptocurrency that the Filecoin network runs on. For example: _You can use filecoin to pay for your transactions._ |
| Miner | An alternate name for a _storage provider_. |
| Private key | A string of letters and numbers that programs use to interact with the Filecoin network. Keep your private key safe, and don't share it with anyone. |
| Storage deal | An agreement between a storage provider and a client about what data to store, how long for, and how much the storage provider can charge for storage. |
| Retrieval deal | An agreement between a storage provider and a client about how much the storage provider can charge to send data to a client. |
| Storage client | The user that wants to store something on the Filecoin network. In this tutorial, _you_ are the storage client. |
| Storage provider | A computer on the Filecoin network offering storage space to other users who want to store data. Storage providers are sometimes called _miners_. |
| Wallet | A collection of addresses. Think of each wallet as a folder and each address as a single file in that folder. |

## Set up

Before you begin storing any data on the Filecoin network, you need to run through a few steps to get everything set up. This section covers getting access to a Lotus full-node, creating a Lotus lite-node on your computer, getting a FIL address, and signing up to Filecoin+.

{{< alert icon="tip" >}}
Programs that interact with the Filecoin network are called _implementations_, and Lotus is a command-line interface (CLI) implementation. There are other implementation being created alongside Lotus, however Lotus is the only Filecoin implementation created and maintained by Protocol Labs.
{{< /alert >}}

### Things to note

As you're going through this section, make a note of the following variables:

| Variable | Description | Example |
| --- | --- | --- |
| Your Filecoin address | The public part of your Filecoin address. This address is what other users can use to send your FIL. | `f1fwavjcfb32nxbczmh3kgdxhbffqjfsfby2otloi` |

## Prerequisites

If you are using macOS you must have [Homebrew](https://brew.sh) installed. If you are using Linux you must have [Snapd](https://snapcraft.io/docs/installing-snapd) installed.

### Access a full-node

A Lotus full-node is a computer running the `lotus daemon`. Full-nodes are unique because they have complete access to the Filecoin blockchain. The computer specifications required to run a Lotus full-node are relatively high and might be out of reach for most end-user laptops and PCs.

Usually, we'd have to _spin up_ a full-node, but we're going to use a Lotus full-node provided by Protocol Labs for this tutorial. This node, called `api.chain.love`, is only for practice sessions like this tutorial and should not be relied upon for any production or development purposes.

## Install a lite-node

A lite-node lets your computer interact with the Filecoin network without having to run a resource-intensive full-node! Lite-nodes can do things like sign messages and talk to storage providers, but any processes that need data from the blockchain must come from a full-node. Luckily, lite-nodes automatically route any blockchain-based requests to a full-node. For this tutorial, you're going to run a Lotus lite-node on your local computer and have it connect to a full-node managed by Protocol Labs.

To install a Lotus lite-node on your computer, you must have the tools required to _build_ a Lotus binary from the GitHub repository.

### macOS

{{< alert icon="callout">}}
You can install Lotus on MacOS 10.11 El Capitan or higher. You must have [Homebrew](https://brew.sh/) installed.
{{< /alert >}}

1. Add the `filecoin-project/lotus` Homebrew tap:

    ```shell
    brew tap filecoin-project/lotus
    ```

1. Install Lotus:

    ```shell
    brew install lotus
    ```

1. Lotus is now installed on your computer.

### Linux

There are two simple ways to install Lotus on Linux:

- [AppImage](#appimage)
- [Snap](#snap)

#### AppImage

1. Update and upgrade your system:

    ```shell
    sudo apt update -y && sudo apt upgrade -y
    ```

1. Download the latest `AppImage` file from the [Lotus GitHub releases page](https://github.com/filecoin-project/lotus/releases/):

    ```shell
    wget https://github.com/filecoin-project/lotus/releases/download/v1.11.1/Lotus-v1.11.1-x86_64.AppImage
    ```

1. Make the `AppImage` executable:

    ```shell
    chmod +x Lotus-v1.11.1-x86_64.AppImage
    ```

1. Move the `AppImage` to `/usr/local/bin` and rename it `lotus`:

    ```shell
    sudo mv Lotus-v1.11.1-x86_64.AppImage /usr/local/bin/lotus
    ```

[Head onto the next section to run your Lotus lite-node ↓](#run-a-lotus-lite-node)

#### Snap

{{< alert >}}
You must have [Snapd](https://snapcraft.io/docs/installing-snapd) installed.
{{< /alert >}}

1. To install Lotus using Snap, run:

    ```shell
    snap install lotus-filecoin
    ```

[Head onto the next section to run your Lotus lite-node ↓](#run-a-lotus-lite-node)

## Run a Lotus lite-node

Now that you have Lotus ready to run, you can start a Lotus lite-node on your computer and connect to the `api.chain.love` Lotus full-node!

{{< alert >}}
Just as a reminder, `api.chain.love` is a Lotus full-node managed by Protocol Labs. It's ideal for use in this tutorial, but should not be used in a development or in a production environment.
{{< /alert >}}

1. Open a terminal windows and run the `lotus daemon --lite` command, using `api.chain.love` as the full-node address:

    ```shell with-output
    FULLNODE_API_INFO=wss://api.chain.love lotus daemon --lite
    ```

    ```
    ...
    2021-06-16T02:00:08.390Z        INFO    markets loggers/loggers.go:56   module ready   {"module": "storage client"}
    2021-06-16T02:00:08.392Z        INFO    markets loggers/loggers.go:56   module ready   {"module": "retrieval client"}
    2021-06-16T02:00:18.190Z        INFO    basichost       basic/natmgr.go:91      DiscoverNAT error:no NAT found
    ...
    ```

1. MacOS users may see a warning regarding Lotus. Select **Accept incoming connections** if you see a warning.
1. The Lotus daemon will continue to run. You must run further commands from a separate terminal window.

Next up is [getting a FIL address ↓](#get-a-fil-address)

## Get a FIL address

Filecoin addresses are similar to regular bank account numbers. Other users can use your address to send you FIL, and you can use your address to pay storage providers for storing and retrieving your data.

There are two parts to a Filecoin address: the public address and the private key. You can freely share your public address with anyone, but you should never share your private key. We're not going to view any private keys in this tutorial, but it's essential to understand the difference between your public address and your private key.

1. Open a new terminal window and create an address using the `lotus wallet new` command:

    ```shell with-output
    lotus wallet new
    ```

    ```
    f1fwavjcfb32nxbczmh3kgdxhbffqjfsfby2otloi
    ```

    Lotus outputs your public address. Public addresses always start with `f1`.

1. Make a note of this address. We'll use it in an upcoming section.

## Backup your address

Your address is made up of two parts: your _public address_ and your _private key_. The public address is what you see when you run `lotus wallet new`, and you're safe to share that address with whoever you want. Your private key, however, must be kept secret and secure. If you lose your private key, you lose access to any FIL stored in that address.

It is incredibly important that you backup your addreses. Storing a copy of your addresses on another device is a great way to ensure you don't lose access to your funds.

1. If your public address `f1...` is still in the terminal window, copy it to your clipboard. If not, list the addresses associated with your Lotus node and copy your public address:

    ```shell with-output
    lotus wallet list
    ```

    ```
    Address                                    Balance  Nonce  Default
    f1nau67e6k6ggdwluatfz4waexetjfrqmx6fil3nq  0 FIL    0      X
    ```

1. Use `lotus wallet export` to export your private key, replacing `f1...` with your public key:

    ```shell
    lotus wallet export f1... > my_address.key
    ```

    This will create a new file called `my_address.key` in the current directory.

Once you have your address in a file, you can copy it to another drive, securely send it to another computer, or even print it out. It's important to keep this file safe. If anything happens to your Lotus node, you can still access your funds using this file.

