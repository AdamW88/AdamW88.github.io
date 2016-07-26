---
layout: post
title: iPhone Charger Demystified
description: "A breakdown of the iPhone's AC adapter, and the technology that goes within"
modified: 2016-07-23
tags: [blog, hardware]
image:
  feature: /RaspberryPi/layout.png
  credit: 
  creditlink:
---

One thing I've noticed as I gain experience in my professional career is the great lengths  some companies go to hide technology from the user. This may seem to not make much sense, as you could easily say 'but Adam, people see technology advance before them all the time. The jump from standard definition to HD, from HDDs to SSDs, the ever improving quality of my Snapchat selfies due to better cameras'. Well, yes...and no.

I'm going to talk about a piece of hardware that I've come to have an extreme amount of respect, admiration, and appreciation for: the iPhone charger.

Yes, that's right, the tiny little brick that everyone has plugged into their wall socket next to their night stand or at their desk at work. The amount of power electronics and safety design that went into your iPhone brick is truly astounding. The remainder of this article is going to explain the electrical portions of the charger.

## Time to talk circuits

In short, your iPhone charger takes between 100 to 240VAC in, and provides a 5VDC output at 1A (5W) via a flyback switch mode power supply. Sounds simple enough, but the actual circuitry Apple has implemented into the charger shows that they engineered this thing to be as safe and efficient as possible. The breakdown of the power flow in the charger is as follows:

-	The AC voltage enters into the charger and is rectified into HV DC by the use of a diode bridge rectifier.
-	This newly rectified DC voltage is then fed into the flyback converter's transformer (which is really an inductor but more on that in a later post), and converted to low voltage AC.
-	The low voltage AC is the rectified back to DC using a high-speed Schottky diode.
-	This low voltage is then passed through a pi filter to remove the ripple and switching transients from the converter and becomes the system output
-	Like all modern SWPSs, the flyback converter  is governed by an IC and a feedback loop monitoring the output voltage.
-	Based on the target output voltage, the duty cycle is adjusted on the low side switching MOSFET to regulate the converter and achieve the desired voltage.

The schematic for the flyback stage is here:

<figure>
	<a href="http://adamw88.github.io/images/PowerElectronics/Flyback_Schematic.jpg"><img src="/images/PowerElectronics/Flyback_Schematic.jpg"></a>
</figure>

You can see the input choke, followed by the diode bridge rectifer, and two RC snubbers to reduce EMI from the DBR. The controller in this charger is the <a href ="http://www.st.com/content/ccc/resource/technical/document/datasheet/b9/c5/7a/59/60/8e/42/14/CD00002330.pdf/files/CD00002330.pdf/jcr:content/translations/en.CD00002330.pdf">STMicroelectronics L6565</a>.

Apple has separated the high voltage and low voltage of the power stage onto two boards within the iPhone charger, and even added some opto-couplers to provide isolation of the feedback signals. These boards are interfaced together with a small ribbon cable carrying feedback signals such as primary current sense, output voltage, and DC input into the controller IC.

The guys over at <a href="http://www.righto.com/2012/05/apple-iphone-charger-teardown-quality.html">righto.com</a> have done a really in depth break-down of the charger, which I definitely recommend checking out if you're super curious about the electrical details of the charger.