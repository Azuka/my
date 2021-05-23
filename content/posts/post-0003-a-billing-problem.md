---
title: "A billing problem based on assumptions"
date: 2021-05-23T08:38:40-07:00
draft: false
tags:
  - sms
---

My partner and I started [SingleSend](https://www.singlesend.com) a little over a year ago.

Everything we knew about SMS messages was that we had a 160-character limit, so dividing the number of characters in
a message by 160 and rounding up would tell us how many messages to bill for. That held true until we got a $750 bill from
Twilio last month which didn't quite match the sending statistics we saw.

My first thought was malicious actors: I immediately rotated our credentials before taking a look at the logs from Twilio which fortunately, are
quite exhaustive. We found no rogue phone numbers in use, and could trace the SMS IDs back to individual campaign message details in our database.

Our Twilio bill did mention something called "segments" which led us to [Twilio's good post on the subject](https://www.twilio.com/blog/2017/03/what-the-heck-is-a-segment.html). The short summary is: most SMS messages use an encoding format called [GSM-7](https://en.wikipedia.org/wiki/GSM_03.38#GSM_7-bit_default_alphabet_and_extension_table_of_3GPP_TS_23.038_/_GSM_03.38) for which the 160 characters per message rule _mostly_ holds true: however, once you introduce certain special characters, including emojis, the encoding switches to [UCS-2](https://en.wikipedia.org/wiki/Universal_Coded_Character_Set).

Twilio has a very convenient [segment calculator](https://twiliodeved.github.io/message-segment-calculator/) that helped troubleshoot.

Here are two sample SMS messages:

`Hello, this is a very simple message that should fit into exactly one text. Adding a real :) somehow triples the number of segments, which isn't fun to find out`

`Hello, this is a very simple message that should fit into exactly one text. Adding 😭 triples the number of segments: finding that out on your bill is quite fun.`

Both have 160 characters, but the second one contains 3 segments while the first has only 1. When you translate that to a 5000-member campaign, the losses weren't pretty. That was a slightly expensive lesson to learn, but it's interesting how assumptions like this don't get noticed until you put in a multiplying factor.

I'll hopefully tell a story about cloud network transfer costs sometime.
