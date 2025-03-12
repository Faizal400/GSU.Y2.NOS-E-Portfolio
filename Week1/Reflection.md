# 📌 Week 1 - Reflection

## 🌍 Key Takeaways  

This week introduced **computer networks and operating systems**, focusing on their role in modern computing.  
Here are my main takeaways:  

### 1. Understanding Networks  
- A **network** is a system of "links" that interconnect "nodes" to exchange information.  
- Networks exist in many forms:  
  - **The Internet** - The largest global network.  
  - **Telephone networks** - Traditional communication systems.  
  - **Transportation networks** - Roads, railways, and air routes.  
  - **Sensor networks** - Used in IoT and real-time monitoring.  

### 2. How the Internet Transformed Society  
- The Internet has **reshaped industries** by enabling:  
  - **E-commerce** (e.g., Amazon, eBay, Alibaba)  
  - **Social media** (e.g., Facebook, Twitter, TikTok)  
  - **Cloud computing** (e.g., Google Drive, AWS, Azure)  
- Digital transformation is **impacting businesses, education, and governance**.  

### 3. Key Features of the Internet  
- **Global Connectivity** - Instant worldwide communication.  
- **Decentralization** - No single controlling authority.  
- **Interoperability** - Devices and applications use common standards (TCP/IP, HTTP).  
- **Scalability** - The Internet can grow to accommodate new users.

 ### 4. Packet Switching (vs Circuit Switching)
 - Forward packets (chunks of data)
 - Alternative to packet switching: Circuit Switching 

### 5. Internet Infrastructure  
- The **Internet is a federated system** that connects different **ISP networks** (>20,000 ISPs globally).  
- The Internet relies on the **IP protocol** to establish a universal addressing system.  
- There are **different network layers**, each responsible for specific tasks.

## ❓ Challenges & Solutions I faced
Luckily enough for me, I did do A Level Computer science (and physics - waves & interference) so I am familiar with some, (not all) of the content. However, despite this I still faced some challenges.

- **Frequency Division Multiplexing:** Different channels transmitted in different frequency bands
  - I feel like the slides don't do a good job about this.
  - I did further research about it. I came across a good analogy, where you imagine you're in a big concert hall where multiple musicians are playing at the same time. Instead of playing over each other, each musician plays at a different sound frequency so that you can hear them seperately.
  - Using this analogy and further reading, I came to understand what FDM actually does. It  divides the total bandwith of a communcation channel into multiple smaller frequency bands. Eahc frequency band is used to transmit a seperate data signal at the same time. Since each signal is sent on a different  frequency, they don't interfere with one another.
- **Time Division Muliplexing:** Each device gets an allocated time slot to send their data.
  - After watching similar  videos to FDM, I also found an analogy for this one where you imagine you and your friends want to talk on a single phone line. Instead of all talking at once (which causes a messs) you each get a small time slot to speak. Once one person finishes their slot, the next person gets their turn and the cycle ocntinues
  - TDM works very similar to this analogy. In the context of data transmission, TDM divides the total transmission time nito small fixed time slots
  - Each device (or user) gets a time slot to send their data
  - The system cycles through users in a fixed order, repeating continously preventing devices sending data all at once which causes a mess
  - A good example of this is CPU Scheduling - where the CPU switches between multiple running programs, giving each one a time slice.
  - Furthermore, I further read into TDM and found out there where two types:
  - Synchronous TDM: Every user gets a fixed time slot, even if they have no data to send
  - Asynchronous TDM (Satistical TDM): Only active users get time slots, avoiding bandwidth
- **Binomial Calculations - Finding the chance that every user is active at once**
  - This will just come along with practice - it's just binomal that I'm very rusty with and need to practice a lot on
  - ![image](https://github.com/user-attachments/assets/98c26f3e-7224-409b-9bd5-b2bc4501398b)
  - 
