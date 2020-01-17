---
layout: post
title: Bitcoin on Node.js — do it yourself
description: ''
date: '2014-09-10T04:28:00.000Z'
categories: []
keywords: []
slug: /@sagivo/bitcoin-on-node-js-do-it-yourself
---

([to the project page in github](https://github.com/sagivo/accept-bitcoin "accept bitcoin"))

Bitcoin is huge, we know it.

While bitcoin ecosystem is starting to develop, more and more companies are offering solutions for developers to collect bitcoins while getting a cut out of this. The big vision of “you own your money” is disrupt buy services like [coinbase](http://coinbase.com) who hold your money for you.

I don’t like it.

I believe a tool to accept money online should be free. This is why I decided to develop a tool like that. Developing a way to accept bitcoins online involve a lot of challenges. Let’s assume you build an online store to sell unicorns made out from bear. Here are some challenges you’ll face:

*   **Who sent me the money?** if you ask your users to pay to your address, how do you know who of them paid? You prefer not to exposed your address at all and save it offline in [cold storage](https://en.bitcoin.it/wiki/Cold_storage).
*   **How can i be sure i got the money?** How can i know the money is actually there?
*   **I want a complete control on events **— you want to know when you got your payment, or expire your offer after some time.
*   **I don’t want to install any bitcoin client **— in order to communicate with the bitcoin network you have to install client on your servers. this client basically sync the entire bitcoin transactions ever made just in order to know that joe have money to pay for your bear unicorn. This client consume a lot of CPU resources, network traffic and storage and could be overkill for simple small size sites.
*   **I want it to be open-sourced** — bitcoin is free and open, so as the tool used to build it.

Having all those challenges in mind, i decided to develop [accept-bitcoin](https://github.com/sagivo/accept-bitcoin) library. This tool allow any developer to quickly, safely and efficiently accept bitcoins in their projects. Accept-bitcoin (AC) was built on top of [bitcore](https://github.com/bitpay/bitcore) library and offer answer to the challenges above. The process is simple:

_A user want to buy your unicorn bear item -> AC generate a random_ **_new_** _bitcoin address just for this sale -> a user get the address and pay -> you get a notification once a payment was made in a minimum security level you decide -> you thank the user for paying -> AC transfers the income to your safe (and secret) address -> user gets his unicorn bear item, you happy_.

This flow solve the challenges above:

*   AC creates ad-hoc address for each sale. Each customer pays to different generated address — this way you can know who paid for what.
*   You can specify your own confirmation level — The bitcoin miners will confirm it for you. AC will monitor the current confirmations level and notify you once the minimum level you want has been reached (if at all).
*   You don’t need to install any bitcoin RPC client! AC will communicate with the bitcoin network for you using tools like [blockr](http://blockr.io/). AC will communicate with the network only to monitor income funds and nothing else.
*   [It’s all open source! Fork it, star it, contribute to it. Hooray!!](https://github.com/sagivo/accept-bitcoin)

So how can you use this tool? Here’s a sample to demonstrate how easy it is:

```js
var settings = {network: 'live'};
var acceptBitcoin = require('accept-bitcoin');
ac = new acceptBitcoin('YOUR_BITCOIN_ADDRESS', settings);
//generate new key for transaction
key = ac.generateAddress({alertWhenHasBalance: true});
console.log("Hello buyer! please pay to: " + key.address());
key.on('hasBalance', function(amount){
  console.log("thanks for paying me " + amount); //do stuff
  //transfer the amount recived to your account
  key.transferBalanceToMyAccount(function(err, d){
    if (d.status === 'success') console.log("Cool, the bitcoins are in my private account!");
  });
});
```

You can find more instructions and info on the [accept-bitcoin project site](https://github.com/sagivo/accept-bitcoin).