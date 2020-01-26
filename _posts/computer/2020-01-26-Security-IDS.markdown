---
layout: "post"
title: "Intrusion detection system(IDS)"
author: "mingi hong"
date: 2020-01-26 08:27:59 -0700
categories: "Computer Security"
permalink: /:categories/:title.html
---

Most Intrusion detection systems ( IDS ) are what is known as signature-based. 
This means that they operate in much the same way as a virus scanner, by searching 
for a known identity - or signature - for each specific intrusion event.
And, while signature-based IDS is very efficient at sniffing out known of attack, it does,

Signature-based IDS is only as good as its database of stored signatures.

1. It is easy to fool signature-based solutions by changing the ways in which an attack is made.
  2. signature database must be continually updated and maintained
  3. May fail to identify a unique attacks.

Anomaly-based IDS is complex creature
  1. It detects any traffic that is new or unusual

Signature-based IDS only scratches the surface of what most organizations need to protect against,
Because it relies on spotting a duplication of events or types of attack that have happened before.
Anomaly testing requires trained and skilled personnel, but then so does signature based IDS
And anomaly testing methods can be guaranteed to provide far more effective protection against
hacker incidents. 

Behavior-based IDS 
  1. references a baseline or learned pattern of normal system activity to identify active intrusion attempts.
  2. Deviations from this baseline or pattern cause an alarm to be triggered.
