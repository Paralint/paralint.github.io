---
layout: page
title: Enigma machine secret engine for Hashicorp Vault
---

I wrote a plugin for Hashicorp Vault that implements an Enigma machine. It serves as a self contained but complete example that you 
can use to build your own plugins.

This page will hold a step by step guide to writing your own Vault secret engine plugin, covering:

 1. The build environment
 1. Organize the code
 1. Your first function
 1. Debug your plugin with Delve
 1. Unit testing
 1. Accepting parameters
 1. Different paths for different purposes
 1. Persist state and secrets
 1. Upgrading your plugin without loosing all of your secrets

Stay tuned, but until then, have a look at [the plugin source on Github](https://github.com/ixe013/benigma)!
