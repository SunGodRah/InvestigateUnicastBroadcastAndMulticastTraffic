# 📡 Investigate Unicast, Broadcast, and Multicast Traffic

## Topology

In this lab, we're going to explore how different types of network traffic—unicast, broadcast, and multicast—behave in a network. Most of the traffic you'll encounter is unicast, but understanding the distinctions between these types of traffic is crucial.

### Objectives
1. Generate Unicast Traffic
2. Generate Broadcast Traffic
3. Investigate Multicast Traffic

---

## Part 1: Generate Unicast Traffic

Let's kick things off by generating unicast traffic using a simple ping command. 

First, I hop over to **PC1** and open the **Command Prompt** from the **Desktop** tab. I enter the command `ping 10.0.3.2` to initiate the ping. As expected, the ping succeeds, confirming that the network is functioning as intended.


<p align="center">
  <img src="https://github.com/user-attachments/assets/e5ebc107-584d-49d2-b538-03df79d874fb" height="100%" width="100%" alt=""/>
</p>

Now, let's switch to **Simulation Mode**. I click on the **Simulation** tab and make sure that only ICMP and EIGRP events are selected in **Edit Filters**. Once that’s set up, I run the ping command again from **PC1** to `10.0.3.2` and start analyzing the traffic.

<p align="center">
  <img src="https://github.com/user-attachments/assets/c2e3bd51-5f7c-4cd1-ad77-4f9ec4318ac5" height="100%" width="100%" alt=""/>
</p>

As the packet travels from **PC1** to **Router3**, I use the **Capture/Forward** button to step through each stage of the journey. Watching the ICMP echo request and reply move between the devices is pretty fascinating. The packet travels through **PC1** -> **Switch** -> **Router1** -> **Router3** and then back through the same path. 

<p align="center">
  <img src="https://github.com/user-attachments/assets/299367a8-4b91-473c-ae6f-c335094d9bdd" height="100%" width="100%" alt=""/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/f45f210a-fb80-4165-8f92-3ba1f2a9aa8d" height="100%" width="100%" alt=""/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/ed17c27d-c5f0-4c57-93dd-38b81a87120b" height="100%" width="100%" alt=""/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/45d7fc9e-4274-4bba-a6da-a5b50e05eb71" height="100%" width="100%" alt=""/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/97a062f0-4d50-448d-a94e-d39d31b9bfbc" height="100%" width="100%" alt=""/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/c7475a1d-c8b6-4bb2-a1c6-9754129cba54" height="100%" width="100%" alt=""/>
</p>

To dive deeper, I open up the PDU information for the first event in the **Simulation Panel Event List**. Here, we can see that the transmission starts at **Layer 3** because of the ICMP & IP protocol usage. When the packet reaches **Router3**, the source and destination IP addresses flip. This behavior is a hallmark of unicast traffic.

<p align="center">
  <img src="https://github.com/user-attachments/assets/51bb3743-e5ba-4af0-8ad9-e75002a7e691" height="100%" width="100%" alt=""/>
</p>

Once I’m done analyzing, I hit the **Reset Simulation** button to clear the traffic.

---

## Part 2: Generate Broadcast Traffic

Next, we’ll generate some broadcast traffic. To do this, I use the **Add Complex PDU** tool. This tool is handy for creating custom test packets. 

I click **Add Complex PDU** and move the cursor over to **PC1** to set it as the source. The **Create Complex PDU** dialog pops up, where I enter `255.255.255.255` as the destination IP address—this is the broadcast address. I keep the **Select Application** option on **PING** and then hit **Create PDU**.

<p align="center">
  <img src="https://github.com/user-attachments/assets/7233028c-4d1e-4c8a-8e92-85f78c06a24e" height="100%" width="100%" alt=""/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/ccea1ef6-b794-4b12-aae2-fca7d00aa798" height="100%" width="100%" alt=""/>
</p>

This action generates a broadcast packet that I can now see in the **Simulation Panel Event List**. I click **Capture/Forward** a couple of times, and I observe the packet being sent to the switch and then broadcasted to **PC2**, **PC3**, and **Router1**. 

<p align="center">
  <img src="https://github.com/user-attachments/assets/c6a5e2c9-f36b-4d6b-8630-300c97ca8c74" height="100%" width="100%" alt=""/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/e4f915eb-3a6b-4db4-9eef-66e8e5f34b55" height="100%" width="100%" alt=""/>
</p>

Diving into the Layer 3 details, I notice that the broadcast packet transforms into a unicast response as it reaches the other devices. Interestingly, this broadcast doesn't extend beyond **Router1** to **Router2** or **Router3**. The reason? A limited broadcast packet is designed to stay within the LAN unless the router is explicitly configured to forward it.


<p align="center">
  <img src="https://github.com/user-attachments/assets/c0f9d21d-adce-4f7d-8800-b8e518232c94" height="100%" width="100%" alt=""/>
</p>

Once I’ve observed the broadcast behavior, I delete the test packet by clicking **Delete** under Scenario 0.

---

## Part 3: Investigate Multicast Traffic

Now, let's examine multicast traffic. Multicast is a bit different from unicast and broadcast. To explore this, I use the traffic generated by the EIGRP routing protocol.

I click **Capture/Forward** to see EIGRP packets at **Router1** waiting to be multicast out to each interface. By opening up the PDU information, I can observe that these packets are sent to **224.0.0.10**, the multicast address for EIGRP routers.

<p align="center">
  <img src="https://github.com/user-attachments/assets/5688843a-93e4-4eb8-bdd1-9906bbdd35ab" height="100%" width="100%" alt=""/>
</p>

As the packets move through the network, the routers process them since they belong to the multicast group, but the PCs simply drop them—no further processing is needed. The destination address of these packets is always `224.0.0.10`, which is specifically reserved for EIGRP.

When I click on one of the packets delivered to a PC, it's evident that the PC discards it without any additional processing, as expected.

<p align="center">
  <img src="https://github.com/user-attachments/assets/b9eaf686-94b0-4fb5-8b8b-9e9c57781352" height="100%" width="100%" alt=""/>
</p>

### Conclusion

By exploring unicast, broadcast, and multicast traffic, we've seen firsthand how data is transmitted across a network:
- **Unicast traffic** is a direct transmission from one device to another.
- **Broadcast traffic** is sent to all devices within a network, but only those that need to respond do so.
- **Multicast traffic** is sent to a specific group, and only devices within that group process it.

Understanding these differences is key to mastering network traffic behavior!
