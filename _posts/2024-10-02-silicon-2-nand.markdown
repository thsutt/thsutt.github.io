---
layout: post
title:  "Silicon2Nand"
date:   2024-10-02 21:42:47 -0400
categories:
---
![Airport](/assets/10-2-24/banner-1.png)
# Introduction
Disclaimer – I am by no means an expert in any of this. I am writing this as much to enhance my own understanding as for my fictitious blog audience. This isn't an explanation for physicists or electrical engineers, it's for computer scientists who are uneasy about not having some vague intuition for how a Nand gate can be made from wafers of silicon.

I'm about to start working through the Nand2Tetris course, a sequence of projects that starts with Nand gates and builds up to a complete operating system and high level language compiler. Nand gates are the smallest building block of computers that isn't an actual electrical circuit. They still *are* circuits, but when you design a computer, you don't have to know what the circuitry looks like underneath to start combining them into more complex features. A Nand gate, to a computer scientist, simply takes in two inputs of 1 or 0 and spits out a 1 unless both inputs are 1. This is already a super, *super* zoomed in level of abstraction, but to me it's not quite good enough to understand computers from the ground up.  What does "1" and "0" mean? What physical structure implements this logic?

To truly start from the ground up, I'm going to take a peek just below the surface of the nand abstraction at silicon – the stuff that actually comes from the ground – and how it can be manipulated into a low level circuit that computer scientists can draw a box around and call it "nand".

# Silicon
If you're anything like me, you probably only know that silicon is related to computers because of a valley in California. Maybe you also have heard that sand is made out of silicon. Are all these tech companies based in California so they can guzzle up all the silicon-laden sand from its plentiful beaches? 

For now, the beaches remain safe. Silicon is already one of the most abundant elements on earth, and the tech companies are in a race to see just how *little* silicon they can use to create the device that is the underpinning of all modern computer systems: the transistor.

# The Transistor
Before we get into how exactly silicon is used to create a computer, we have to understand what it is we are actually building with the silicon. It's easy to forget while conversing with chatbots or immersed in seamless VR that all computers, even the ones that do incredibly cool and fancy things, are just really, *really* complicated switching devices. The 1s and 0s that computers are supposedly made of, after all, are really the states of a switch: on, or off. We interact with electric switches all the time when we turn on and off the light, so what is so special about a transistor?

A transistor is an electric switch that can turn on and off based off of another electric signal. Instead of a finger flipping the switch on and off, you just use another wire with a voltage applied! Here is a crude diagram of a transistor: 

![transistor](/assets/10-2-24/transistor.jpg)*How can you turn this into a radio to go down the old mine with? A question for another time.*

This is exactly what a computer is made out of. 

As I mentioned before, when we write 1s and 0s to represent the internal state of a computer, we are talking about something being on or off. With a light switch, the light is simply on if the switch is on, and off is the switch is off. However, within a computer, we wouldn't say that the transistor itself is on or off. Instead, any given wire can be viewed as on or off. We consider a wire to be "on" when, if you were to touch a voltage meter to that wire, it would read a value above some threshold, and "off" otherwise. 

Unlike a light switch, where the input to the switch and the overall state of the system are the same, this transistor has three coexisting states – the state of the in wire, the switch wire, and the out wire. These states represent whether the voltage at each wire is high (1) or low (0). 

## ...And It's Made Of Sand?
Transistors are wonderful to have, especially when we can make billions of them and wire them up together to make VR goggles, cell phones, and AI that can detect cancer. But before we get started with wiring up transistors, let's first dig just a bit more into how it all works on the level of the metal. 

Silicon is a semiconductor. Rather than directing half of an orchestra, it is a metal that can change it's level of electrical conductivity. *Exactly* why it can do this is a bit out of scope for this post, but the high level explanation in a chunk of silicon metal, most of the electrons stay caught within the orbit of a given silicon atom's nucleus, but a small number float around within the metal, unattached to a particular atom and free to flow as current if a voltage is applied to the metal. 

In a chunk of copper, the amount of freely floating electrons is quite high, and when a voltage is applied to the copper, these electrons flow easily, creating a strong electric current. In a material that doesn't conduct electricity, there are no floating electrons like this available, so a voltage can't get electrons flowing within the material. Silicon happens to be one a few elements where the amount of available electrons to flow in a current is *just right* for it to be able to sometimes conduct electricity, and sometimes not. A *semi* conductor! The exact circumstances in which it is or is not a conductor will be investigated shortly.

## P and N type

![P and N type silicon](/assets/10-2-24/p-and-n.jpg){: width="75%" }

Thinking about the electrons floating freely in the silicon is important when it comes to understanding how we can manipulate silicon into becoming this magical switching device, the transistor. It turns out that by introducing certain impurities into silicon, we can change its electrical properties. If we add to the silicon an element that has extra electrons of its own, the silicon becomes flush with electrons and has more available to carry current – known as n type silicon. It's like a chocolate chip cookie, with extra electrons floating around like chocolate chips. If we introduce an element that steals electrons away from the silicon, we can create 'holes' in the silicon where there is actually space for an electron to occupy but none available – known as p-type silicon. It's like swiss cheese, with a bunch of holes in it. These two kinds of silicon are essentially opposites – one with two many electrons, and one with two few.  

![NP Junction](/assets/10-2-24/n-p-junction.jpg)

When you squeeze together a piece of p-type and a piece of n-type silicon, something interesting happens. The extra electrons in n-type will hop across the gap and fill the holes in the P type, which means that near the boundary, there are no charges remaining on the n-type side. This is known as a "depletion region," and it means that current is blocked from flowing across the barrier between these two types of silicon.

## MOSFET Setup
By far the most common type of transistor in use today is the MOSFET transistor, which stands for Metallic Oxide Semiconductor Field Effect Transistor. We just talked about the semiconductor part, and we don't need to worry about the metallic oxide part for understanding transistors at this very basic level. So what about the field effect?

Here's a simplified cross-section of a MOSFET transistor:

![Incomplete MOSFET cross section](/assets/10-2-24/mosfet-1.jpg)

On the bottom we have p-type silicon, the swiss cheese. The two rectangles side-by-side are n-type, the chocolate chip cookies. All along the border between these two types is a depletion region across which current cannot flow. Let's consider the left side n-type silicon to be the input wire to the transistor, and the right to be the output wire. We want a current to flow between these two piece of silicon. How can we achieve this?

Well, let's add a wire connected to the top of this transistor, like so: 

![MOSFET Cross Section 2](/assets/10-2-24/mosfet-2.jpg)

If we just run a current of electrons through this wire right now, it will simply flow through the p-type silicon, rushing in to fill the electron holes in the material. However, if we put an insulator (perhaps a metallic oxide) between the wire and the p-type silicon, current will not flow, but an electric field will form around the end of wire (known as the "gate", equivalent to the switch in the earlier transistor diagram). This electric field will tug on the electrons in the P type silicon (though it has been doped to have holes, there are still some remaining free electrons, just very few) and pull them up towards the gate. 

![MOSFET Switch ON](/assets/10-2-24/mosfet-3.jpg)

Since they cannot cross the insulator, they start to gather around the area, filling in the holes in that part of the silicon and eventually creating something that looks a lot like n-type silicon. Now there is a bridge between the two n-type sections, and current can flow between them! 

![MOSFET Switch ON 2](/assets/10-2-24/mosfet-4.jpg)

If you turn off the wire, the electrons drift off back into the silicon, and the bridge evaporates. So now, by applying a charge to one wire, you can control the flow of electricity across two other wires! We have ourselves a transistor!

## Transistor2Nand
When it comes to building our computer out of silicon, we are not quite out of the woods yet with physics and electrical engineering. To build our Nand gate, we need to think a bit more about the flow of electricity through a circuit with some transistors.

Remember, the Nand gate outputs a '1' (high voltage) unless *both* inputs are a 1 (high voltage). Let's sketch out a simple circuit that uses two transistors to achieve something that looks like this:


![Nand circuit](/assets/10-2-24/nand.jpg){: width="50%" }


What's happening here? Remember that the flow in a circuit is always from high voltage to low, and that electricity will always be flowing somewhere. If these two transistors are off - IE no current can go through – then it will flow out through the output wire, which we consider a 1. If either A or B has power, the transistor will open – but if the other remains off, the current will still be blocked. Only when *both* A and B have power, and thus their transistors are both open, will current flow directly from the high voltage to the ground, and bypass our output wire entirely – which we consider an output of 0.


![Nand gate](/assets/10-2-24/nand-abstracted.jpg){: width="50%" }


Now the magic, the moment of conceptual lift off. Let's draw a box around this circuit and just forget about the messy power input – we'll assume from here on out that all our components are implicitly connected to power – and now we have our very first abstraction: the Nand gate. 

## Aside: Photolithography
Remember that the MOSFET diagrams are depicting the cross-section of a chip, as if you placed the chip flat on a table and viewed it from the side. How on earth do we create such a delicate sandwich of silicon layers, and how do we do it billions of times in intricate arrangements squeezed down onto a chip no larger than your fingernail? 

Instead of soldering together wires, a computer circuit is what's called an integrated circuit, meaning the circuit is carved out of a single continuous block of metal. Photolithography refers to the process of literally *etching* a transistor onto a piece of silicon using light. 

In perhaps the most delicate and complex manufacturing operation the world has ever seen, small wafers of silicon are first coated in various layers of photosensitive material. Chip designers draw out all the circuits that will go into the chip onto a sheet that blocks light everywhere except the drawn lines. The light is shined through this sheet, and then focused down to a microscopic point where the drawing is now the true size of the transistors. The light burns away the photosensitive material on the wafer, and then chemicals can be used to etch away the silicon where the photosensitive material once was. 

This process is repeated along with intermediate steps to dope parts of the silicon or add oxide, and the end result is that we now are able to produce a MOSFET transistor smaller than a strand of DNA, and squeeze billions of them into your smartphone and laptop. 

## Conclusion
We will go on to use this nand gate to build even more complicated gates that take many 1s and 0s as inputs and output many 1s and 0s back. These larger gates will eventually stack up to create the computer architecture behind the device you are using to read this post right now, by which point this view of silicon, electrons, voltage, and circuits will have long receded into distant memory. There will be no direct mapping between the higher level ways of thinking about a computer and the low level circuitry we have just explored, only the deep, deep iceberg of abstraction built on top of abstraction.  It is precisely this ability of the human mind to abstract, compartmentalize, and wield abstraction to build further abstractions that enables us to coordinate billions of transistors without needing to understand anything about silicon doping or integrated circuits. Anyone who is serious about being a computer scientist should dive all the down at least once, though, if only to see how deep the rabbit hole truly goes. 


**Sources:**

https://www.youtube.com/watch?v=rkbjHNEKcRw

https://en.wikipedia.org/wiki/MOSFET

https://mathcenter.oxford.emory.edu/site/cs170/nandFromTransistors/

https://nandgame.com/