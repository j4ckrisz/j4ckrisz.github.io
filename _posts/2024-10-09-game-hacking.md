---
title: Game Hacking Part 1
date: 2025-07-23 11:44:00 -0400
description: >-
   Every number in game has a variable, there is a variable for the health, ammo and more. Where are these values and these variables stored? Well, they are stored somewhere on our computer, that's what we call memory addresses. Here in this example, we will be changing those variable values in a game so, we can take advantage of it like, get unlimited health, ammo etc.
author: j4ckris
categories: [Hacking]
---

Every number a in game has a variable, there is a variable for the health, ammo and more. Where are these values and these variables stored? Well, they are stored somewhere on our computer, that's what we call memory addresses. Here in this example, we will be changing those variable values in a game so, we can take advantage of it like, get unlimited health, ammo etc.

To start with it, let's see what we're going to be using.

We're going to be using a game called [Assault Cube](https://assault.cubers.net/) and a Memory Scanner called [CheatEngine](https://www.cheatengine.org/) to start our hacking demonstration.

Let's start by executing the AssultCube setup.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2F7yDeDEFJfeXkY8o0BSY1%2Fgame_hacking1.png?alt=media&token=723605b4-f761-48b6-b322-be75f6fb5b01)

Just click -> next -> next -> next -> next

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FskjYy9470J0HibfNuEbf%2Fgame_hacking2.png?alt=media&token=e6e50fad-3667-4190-8509-92828f58173d)

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FnxkHZDRG80Ai5dOmczN1%2Fgame_hacking3.png?alt=media&token=1c8a8ebe-257d-4de2-ab00-f2212b8d9663)

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FryyJoutDojIDanrFWm4U%2Fgame_hacking4.png?alt=media&token=7fb28bb1-5f9b-460f-82b6-1ac8c85a750d)

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FowFhpHn7SZo9wtdH9Lmd%2Fgame_hacking5.png?alt=media&token=a7a46806-a95f-4fcc-bfdd-eeae2aca5529)

Once you have installed AssultCube, lets continue with the CheatEngine installation.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FXnrRkEXNCRcucKLQac8k%2Fgame_hacking6.png?alt=media&token=dc7df23e-fcae-4f17-8707-b5464b8ac54c)

Just click, next, skip all and finish.

Once you install CheatEngine start AssultCube and get into a map alone without anybody, to be more comfortable with the hackies.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FtcsUXGR8n9nE3Pv16eEr%2Fgame_hacking8.png?alt=media&token=f5fa2bb7-91ab-4860-959e-282f964c6c3b)

Once we enter into our solo lobby, click the monitor on the CheatEngine to start scanning the memory address in the program.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FrJKtGM1tejlddrO83WMm%2Fgame_hacking9.png?alt=media&token=7739c1e0-c3b7-436e-9b76-31692ef0620a)

Click on AssultCube process.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FKjapYNsme2EX7xqleEry%2Fgame_hacking10.png?alt=media&token=386cea84-8b6b-4765-b7ba-e6487c23e04d)

First, we're going to do it's click on the First Scan button to have a Scan that we will be comparing with our others scan's.
In the image below, we're putting the hexadecimal value that we're looking for in our game, it will be explained in a moment.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FY2DjrAWIcHkbgpetNJNQ%2Fgame_hacking11.png?alt=media&token=9401b320-78d2-418f-a26d-0bbfd8fb95b7)

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FRTanelV5ekoCJaFI7jRj%2Fgame_hacking13.png?alt=media&token=93fef5d2-4acc-487c-95fb-43e7bc63d033)

In the image above we can see how I have 7 rounds on my pistol, a pistol maximum round's is 10 in the pistol.

So, I need to look for memory addresses that have a value of 7 (I shotted the pistol 3 times of 10 from the First Scan).

Now in the image below I will be looking for memory addresses that have a value of 7.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FZ2Qq3t6iGCPSrphjRDWi%2Fgame_hacking14.png?alt=media&token=72a28730-864d-42b1-a9ee-98120162046f)

I shot one bullet again, now I have 6 rounds on my magazine, let's look for those.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2F7JbZ2WppDUZ4EMLdwiqK%2Fgame_hacking15.png?alt=media&token=00b8df97-4800-463d-9b38-dfc4179e9c2b)

In the image above we can see how shooting and looking for values lead us to the following 2 variables, one of them will be the ammo memory we're looking for.

Since they're just 2 variables we can try to guest, changing the value to see what is the ammo memory address that is using.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FDw71OZdaZsNA29FgvvgS%2Fgame_hacking16.png?alt=media&token=4da69453-2718-4943-9ffb-ecdc05203e46)

Let's select the 2 variables and added to our address list.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2Fbz3oiIHzOxEMe2vLaa0F%2Fgame_hacking17.png?alt=media&token=d9edc21e-f4f8-4a80-96b9-7d04b7697d9a)

I will be changing the gun value to 9999 to see what is happens.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FM52GeNKGNbuJGM0GPc4s%2Fgame_hacking19.png?alt=media&token=c92756d9-aa71-4d9c-8bed-f66e3f312c56)


 - PD: I changed the value of the rifle because my game time was over so, it restarted.

There we go, I have changed the value to 9999 two times that's why the ammo it's accumulating.

![](https://362618020-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fi9hCCmXtAKNvbIKRqULt%2Fuploads%2FAO9vOjlroEBHDZToj8zw%2Fgame_hacking20.png?alt=media&token=d809fd90-1f44-4c41-9656-959c6f81c64a)

Now to Identify the memory address of the rifle ammo, we can set a description on the CheatEngine.

We Can do the same with the health and other things.
