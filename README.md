# PlebLab ATX Lightning Door
Instructions &amp; Resources for how we set up a lightning payments enabled access control for PlebLab.

# Premise
Necessity is the mother of invention right? Wrong.

Laziness is the mother of invention.

We got tired of always running down from the second floor to answer the door bell so we did something about it.

Lightning door is a custom solution we deployed to make it so that anyone that has basic understanding of lightning can let themselves into Pleblab without a key. Simply scan the QR code or NFC at the entrance of Pleblab, pay the nominal invoice, and the lightning bolt lights up and the door unlocks:

<img width="684" alt="image" src="https://github.com/uncleJim21/PlebLabATXLightningDoor/assets/96802642/b15366a6-58fd-4885-b445-364ee378b512">

# Block Diagram
![image](https://github.com/uncleJim21/PlebLabATXLightningDoor/assets/96802642/671531d4-2971-41ff-9887-2c032ae8b094)

# Technical Details
Ok that's a fancy diagram. What's it all mean and what's it all for? 

Let's walk through the process:

0. The mechanical timer switch establishes "open hours" typically from 930AM-630PM by removing controlling power to bitcoin switch.
1. The user scans the static LNURL on the QR code/NFC device
2. The user's wallet resolves the LNURL and makes a request to our LNBits Legends instance
3. That instance provides a concrete Bolt11 invoice for them to pay (~180 sats/5 cents)
4. The user pays the invoice
5. Our LNBits Legends instance registers that and sends a message over websocket to the Bitcoin Switch
6. The Bitcoin Switch toggles a digital output for about 15 seconds
7. The circuitry is configured such that when the digital output triggers it:
  a. Toggles the relay that controls the door's strike/lock
  b. Energizes the 12V circuit which powers a step down converter that turns on our 5V USB based neon light


# FAQs

**What materials do I need to replicate what you did?**
- Bare minimum: ESP32 dev kit & access to the Bitcoin switch repo (both links below)
- Simple commercially available 12V to 5V step down converter
- We got the neon light at a bitcoin bazaar, gutted the USB part and through careful probing determined the +/-

**How did you know where to probe/do "surgery" to the existing system?**
Most commercial access control systems are similar. They are usually controlled by a central control board and move a strike (actuated lock) that locks/unlocks the door. We got access to the control board and using safety best practices probed circuits until we discovered which relay output affects change on the front door. We **triple checked** to make sure we knew how the circuit worked then installed a parallel relay that simply creates "or logic" on the existing circuit giving us the opportunity to override the front door control.

**What is the Bitcoin Switch?**
It is a simple ESP32 based dev kit that allows you to take advantage of its impressive built in WiFi chip, GPIO and other peripherals to connect to a websocket & control local hardware.


# Resources
- Bitcoin Switch repo: https://github.com/lnbits/bitcoinSwitch
- Bitcoin Switch Hardware on Amazon links: https://amzn.to/43ilywc
- Eden Printers - 3D Printed NFC Sign https://www.eden3dprinter.com/shop/nfc-chips
