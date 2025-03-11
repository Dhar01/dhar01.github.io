---
title: "Making custom badges using shields.io"
date: 2022-03-18
tags: ["how to"]
draft: false
---

When I wanted to customize my GitHub profile, I visited many developers' GitHub profiles. I saw some badges which seemed cool to me. When I looked at the code behind the shapes, I didn't know what was happening. Later, I tried to copy some of the code and paste them into my code, but it couldn't solve the need.

After some reading and searching, I understood how to make them. I am writing this post as a reference so that one can understand how to make those badges, including me.

## Step 01 : Finding icons/logo

The first thing we need to do to make badges is to find if the logo image is available for [shields.io](https://shields.io/#your-badge). I use [simple icons](https://simpleicons.org/) site. You can search for the logo to see if it's' available on their site.

## Step 02 : The URL

If the icon is available, we are ready to start building badges. The first part of the URL that will precede all your badges is given below:

```
https://img.shields.io/badge/
```

Following the first part, we are going to add a dash (`-`). After the dash, we will write the name which we want to be appear on the badge (*case-sensitive*).

Suppose, we want to write Linux on badge:

```
https://img.shields.io/badge/-Linux
```

To write more than one word, we need to add `%20` instead of space:

```
https://img.shields.io/badge/-Amazon%20Prime
```

## Step 03 : The Color

We need color for our badge background. The `shields.io` accepts named colors and hex codes. Some of the named colors are: *green, brightgreen, yellowgreen, yellow, orange, red, blue, lightgrey, grey/gray, success, important, critical, informational, inactive, black, blueviolet*, etc.

For some more named colors and hex codes, see [here](https://htmlcolorcodes.com/color-names/).

To add the named colors or the hex codes, we need to add a dash (-):

```
https://img.shields.io/badge/-Linux-
```

On Simple Icons website, you will find the official color (*and hex code*) for that specific icon which can be used.

## Step 04 : The Logo

After finding the logo, it's time to add the logo name. There are two types of name one will find there:

1. Single name. *For example: Linux*.
2. Name with more than one word. *For example: Visual Studio Code*.

For single name, it's very simple to add:

```
https://img.shields.io/badge/-Git-F05032?logo=git
```

For more than one word, you have to add a dash (-) between words:

```
https://img.shields.io/badge/-Kali%20Linux-557C94?logo=Kali-Linux
```

## Step 05 : The Logo Color

One can also define a color for logo by doing `&logoColor=<color>` at the end of the url.

```
https://img.shields.io/badge/-Kali%20Linux-557C94?logo=Kali-Linux&logoColor=white
```

## Step 06 : Other Stylings

Other than colors, there are some more styling of the badges:

- The flat style
- The Plastic Button style
- Squared Bold Style
- Squared Style

```
https://img.shields.io/badge/-Kali%20Linux-557C94?logo=Kali-Linux&logoColor=white&style=flat
```

One can also add width for the logo by adding `&logoWidth=<amount-of-pixels>`.

To get a view of my GitHub profile, visit here: [Loknath Dhar](https://github.com/Dhar01)