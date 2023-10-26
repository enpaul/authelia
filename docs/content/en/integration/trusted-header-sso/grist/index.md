---
title: "Grist"
description: "Trusted Header SSO Integration for Grist"
lead: ""
date: 2023-10-26T17:30:00+5:00
draft: false
images: []
menu:
integration:
parent: "trusted-header-sso"
weight: 420
toc: true
community: true
---

## Introduction

This is a guide on integration of __Authelia__ and [Grist] via the trusted header SSO authentication.

As with all guides in this section it's important you read the [introduction](../introduction.md) first.

## Tested Versions

* Authelia: v4.37
* Grist: 1.1

## Before You Begin

This example makes the following assumptions:

* You have setup Grist according to the best practices specified in the [Grist self-managed documentation](https://support.getgrist.com/self-managed/)
* You have setup Authelia with your reverse proxy to handle [forward header authentication](../../proxies/forwarded-headers/index.md)
* __Application Root URL:__ `https://grist.example.com`
* __Authelia Root URL:__ `https://auth.example.com`
* __Authelia Service Name:__ `authelia`

## Configuration

To configure [Grist] to trust the `Remote-Email` header do the following:

1. Set the `GRIST_FORWARD_AUTH_HEADER` environment variable for the Grist instance to `Remote-Email`
2. Configure the reverse proxy to pass requests for the URL `https://grist.example.com/auth/login` to `http://authelia:9091/api/verify?rd=https%3A%2F%2Fauth.example.com%2F`
3. Configure the reverse proxy to pass requests for the URL `https://grist.example.com/logout` to `http://authelia:9091/logout?rd=https%3A%2F%2Fauth.example.com%2F`

*__Important:__ Ensure that the value of `GRIST_DEFAULT_EMAIL` is set to the email of a user account that can 
authenticate using Authelia. Once forward authentication is enabled Grist will not allow the default user to
login except by authenticating through Authelia.


[Grist]: https://www.getgrist.com/
