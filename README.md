# VLAN Segmentation and Trunking Lab (Cisco Packet Tracer)

<h2>Description</h2>
This lab demonstrates VLAN segmentation, inter switch trunking, unused port hardening, and inter VLAN routing using a router on a stick configuration. Two Cisco 2960 switches are trunked together, carrying three VLANs, with a 1941 router providing routing between them. Six PCs (two per VLAN) confirm that devices in the same VLAN can communicate while devices in different VLANs require the router to reach each other.
<br />

<h2>Technologies Used 🧪</h2>

<table>
  <tr>
    <td><b>Cisco Packet Tracer</b></td>
    <td><b>1 x Cisco 1941 Router</b></td>
  </tr>
  <tr>
    <td><b>2 x Cisco 2960 Switches</b></td>
    <td><b>4 x End-Device PCs</b></td>
  </tr>
</table>

<h2>Topology</h2>

<img src="https://i.imgur.com/2iWgGdz.png" alt="Topology"/>

<table>
  <tr>
    <td><b>Device</b></td>
    <td><b>Model</b></td>
    <td><b>Role</b></td>
  </tr>
  <tr>
    <td><b>CopySwitch0</b></td>
    <td><b>2960 | 24TT</b></td>
    <td><b>Access switch, VLANs 10/20/30</b></td>
  </tr>
    <tr>
    <td><b>CopySwitch1</b></td>
    <td><b>2960 24TT</b></td>
    <td><b>Access switch, VLANs 10/20/30, trunk to Router1</b></td>
  </tr>
    <tr>
    <td><b>Router1</b></td>
    <td><b>1941</b></td>
    <td><b>Router on a stick, subinterfaces for each VLAN</b></td>
  </tr>
  <tr>
    <td><b>PC-PT x6</b></td>
    <td><b>Generic PC</b></td>
    <td><b>Two per VLAN, endpoints for testing</b></td>
  </tr>
</table>

<h2>Objective</h2>
<ul>
<li><strong>Create VLANs 10, 20, and 30 on both switches</li>
<li><strong>Assign access ports to the correct VLAN</li>
<li><strong>Configure a trunk link between the two switches carrying all three VLANs</li>
<li><strong>Configure router subinterfaces for inter VLAN routing</li>
<li><strong>Harden the switch by placing unused ports into a dedicated blackhole VLAN</li>
<li><strong>Assign static IP addressing to each PC and verify connectivity</li>
</ul>

<h2>IP Addressing Scheme</h2>

<table>
  <tr>
    <td><b>VLAN</b></td>
    <td><b>Purpose</b></td>
    <td><b>Subnet</b></td>
    <td><b>Gateway</b></td>
    <td><b>Hosts</b></td>
  </tr>
  <tr>
    <td><b>10</b></td>
    <td><b>HR</b></td>
    <td><b>172.16.1.0/24</b></td>
    <td><b>172.16.1.1</b></td>
    <td><b>.10, .20</b></td>
  </tr>
    <tr>
    <td><b>20</b></td>
    <td><b>Staff</b></td>
    <td><b>172.16.2.0/24</b></td>
    <td><b>172.16.2.1</b></td>
    <td><b>.10, .20</b></td>
  </tr>
    <tr>
    <td><b>30</b></td>
    <td><b>Sales</b></td>
    <td><b>172.16.3.0/24</b></td>
    <td><b>172.16.3.1</b></td>
    <td><b>.10, .20</b></td>
  </tr>
</table>

<h2>Logical Addressing Table</h2>

<p>All IP addresses are subnetted into Class C blocks using a 24 bit subnet mask (255.255.255.0) to define logical VLAN boundaries.</p>

<table>
  <tr>
    <td><b>Host / Interface</b></td>
    <td><b>Role</b></td>
    <td><b>VLAN</b></td>
    <td><b>IP Address</b></td>
    <td><b>Subnet Mask</b></td>
    <td><b>Default Gateway</b></td>
  </tr>
  <tr>
    <td>PC-A</td>
    <td>HR</td>
    <td>10</td>
    <td><code>172.16.1.10</code></td>
    <td><code>255.255.255.0</code></td>
    <td><code>172.16.1.1</code></td>
  </tr>
  <tr>
    <td>PC-B</td>
    <td>Staff</td>
    <td>20</td>
    <td><code>172.16.2.10</code></td>
    <td><code>255.255.255.0</code></td>
    <td><code>172.16.2.1</code></td>
  </tr>
  <tr>
    <td>PC-C</td>
    <td>Sales</td>
    <td>30</td>
    <td><code>172.16.3.10</code></td>
    <td><code>255.255.255.0</code></td>
    <td><code>172.16.3.1</code></td>
  </tr>
  <tr>
    <td>PC-D</td>
    <td>HR</td>
    <td>10</td>
    <td><code>172.16.1.20</code></td>
    <td><code>255.255.255.0</code></td>
    <td><code>172.16.1.1</code></td>
  </tr>
  <tr>
    <td>R1 Fa0/1.10</td>
    <td>HR Gateway subinterface</td>
    <td>10</td>
    <td><code>172.16.1.1</code></td>
    <td><code>255.255.255.0</code></td>
    <td>N/A</td>
  </tr>
  <tr>
    <td>R1 Fa0/1.20</td>
    <td>Staff Gateway subinterface</td>
    <td>20</td>
    <td><code>172.16.2.1</code></td>
    <td><code>255.255.255.0</code></td>
    <td>N/A</td>
  </tr>
  <tr>
    <td>R1 Fa0/1.30</td>
    <td>Sales Gateway subinterface</td>
    <td>30</td>
    <td><code>172.16.3.1</code></td>
    <td><code>255.255.255.0</code></td>
    <td>N/A</td>
  </tr>
</table>





<h2>1. Configuration Steps</h2>

<h4>PC-A (VLAN 10, HR)</h4>
<ol>
  <li>Open PC-A in Packet Tracer</li>
  <li>Go to Desktop &rarr; IP Configuration</li>
  <li>Select Static and enter:
    <ul>
      <li>IP Address: <code>172.16.1.10</code></li>
      <li>Subnet Mask: <code>255.255.255.0</code></li>
      <li>Default Gateway: <code>172.16.1.1</code></li>
    </ul>
  </li>
</ol>

<h4>PC-B (VLAN 20, Staff)</h4>
<ul>
  <li>IP Address: <code>172.16.2.10</code></li>
  <li>Subnet Mask: <code>255.255.255.0</code></li>
  <li>Default Gateway: <code>172.16.2.1</code></li>
</ul>

<h4>PC-C (VLAN 30, Sales)</h4>
<ul>
  <li>IP Address: <code>172.16.3.10</code></li>
  <li>Subnet Mask: <code>255.255.255.0</code></li>
  <li>Default Gateway: <code>172.16.3.1</code></li>
</ul>

<h4>PC-D (VLAN 10, HR, connected to Switch 2)</h4>
<ul>
  <li>IP Address: <code>172.16.1.20</code></li>
  <li>Subnet Mask: <code>255.255.255.0</code></li>
  <li>Default Gateway: <code>172.16.1.1</code></li>
</ul>


Device	Model	Role
CopySwitch0	2960 24TT	Access switch, VLANs 10/20/30
CopySwtich1	2960 24TT	Access switch, VLANs 10/20/30, trunk to Router1
Router1	1941	Router on a stick, subinterfaces for each VLAN
PC-PT x6	Generic PC	Two per VLAN, endpoints for testing
<p>Logical Addressing Table
All IP addresses are subnetted into Class C blocks utilizing a 24-bit subnet mask to define logical boundaries [26].

Host / Interface	Intended Department/Role	VLAN ID	IP Address	Subnet Mask	Default Gateway
PC-A	Human Resources (HR) [28]	10	172.16.1.10 [26]	255.255.255.0 [26]	172.16.1.1 [26]
PC-B	Staff Support [28]	20	172.16.2.10 [27]	255.255.255.0 [27]	172.16.2.1 [27]
PC-C	Sales Department [28]	30	172.16.3.10 [27]	255.255.255.0 [27]	172.16.3.1 [27]
PC-D	Human Resources (HR) [28]	10	172.16.1.20 [27]	255.255.255.0 [27]	172.16.1.1 [27]
R1 - FA 0/1.10	HR Gateway sub-interface [37]	10	172.16.1.1 [37]	255.255.255.0 [37]	N/A
R1 - FA 0/1.20	Staff Gateway sub-interface [37]	20	172.16.2.1 [37]	255.255.255.0 [37]	N/A
R1 - FA 0/1.30	Sales Gateway sub-interface [27]	30	172.16.3.1 [27]	255.255.255.0 [27]	N/A</p>

<table>
  <tr>
    <td><b>VLAN</b></td>
    <td><b>Purpose</b></td>
    <td><b>Subnet</b></td>
    <td><b>Gateway</b></td>
    <td><b>Hosts</b></td>
  </tr>
  <tr>
    <td><b>10</b></td>
    <td><b>HR</b></td>
    <td><b>172.16.1.0/24</b></td>
    <td><b>172.16.1.1</b></td>
    <td><b>.10, .20</b></td>
  </tr>
    <tr>
    <td><b>20</b></td>
    <td><b>Staff</b></td>
    <td><b>172.16.2.0/24</b></td>
    <td><b>172.16.2.1</b></td>
    <td><b>.10, .20</b></td>
  </tr>
    <tr>
    <td><b>30</b></td>
    <td><b>Sales</b></td>
    <td><b>172.16.3.0/24</b></td>
    <td><b>172.16.3.1</b></td>
    <td><b>.10, .20</b></td>
  </tr>
</table>





</p>Each PC was configured with the addressing pattern below, adjusted per VLAN:
```
- IP Address: 172.16.1.10
- Subnet Mask: 255.255.255.0
- Default Gateway: /172.16.1.1
```
<h2>3Configuration Step-by-Step</h2>
<p>Configure the static IP parameters on each end-user PC.
Step 1.1: PC-A Configuration [26]
Open PC-A in Packet Tracer.
Navigate to Desktop -> IP Configuration.
Select Static and input:
IP Address: 172.16.1.10
Subnet Mask: 255.255.255.0
Default Gateway: 172.16.1.1
Step 1.2: PC-B Configuration [27]
Open PC-B and input:
IP Address: 172.16.2.10
Subnet Mask: 255.255.255.0
Default Gateway: 172.16.2.1
Step 1.3: PC-C Configuration [27]
Open PC-C and input:
IP Address: 172.16.3.10
Subnet Mask: 255.255.255.0
Default Gateway: 172.16.3.1
Step 1.4: PC-D Configuration [27]
Open PC-D (connected to Switch 2) and input:
IP Address: 172.16.1.20
Subnet Mask: 
  
Default Gateway: 172.16.1.1
Configuration Steps
1. VLAN Creation and Access Port Assignment

VLANs 10 (HR), 20 (Staff), and 30 (Sales) were created on both switches, and access ports were assigned so each PC lands in the correct VLAN.

2. Trunk Configuration

The link between CopySwitch0 and CopySwtich1 was configured as a trunk, carrying VLANs 10, 20, and 30 between the switches.

3. Unused Port Hardening (Blackhole VLAN)

All unused switchports were placed into a dedicated VLAN 100, named BLACKHOLE, so an unauthorized device plugged into an idle port has no path onto the network.<h3>The following settings were configured during account creation:</h3>
<ul>
  <li>Display name and username following the defined convention</li>
  <li>"Force password change on first login" enabled as a security baseline</li>
  <li>License assignment applied at creation</li>
</ul>
4. Router on a Stick

Router1 was configured with subinterfaces for each VLAN (Gig0/1.10, Gig0/1.20, Gig0/1.30 style addressing) to route traffic between VLANs.
5. Save Configuration

Running configuration was saved to startup configuration on the switches to persist the setup.Verification
Trunk and VLAN StateConnectivity Testing

Pings confirmed reachability within a VLAN, to the default gateway, and across VLANs through the router.Mistakes and Troubleshooting
Mistake 1: Native VLAN Mismatch

While configuring the trunk, the switches logged a %CDP-4-NATIVE_VLAN_MISMATCH warning. CopySwitch0 had the trunk native VLAN set to 40, while CopySwtich1 was still on the default native VLAN of 1. A native VLAN mismatch means untagged traffic on the trunk is interpreted as belonging to different VLANs on each end, which breaks Spanning Tree and can leak traffic between VLANs. This was resolved by setting the native VLAN to 40 on both ends of the trunk so they agreed.

Mistake 2: Port to VLAN Mix Up

While assigning access ports on the second switch, VLAN 40 was mistakenly applied to Fa0/1 instead of Fa0/2, which is where VLAN 10 needed to live. A follow up typo (inf f0/2 instead of int f0/2) also returned an invalid input error before the correct interface was reached. The result was that the PC at 172.16.1.20 could not be reached, showing 100 percent packet loss.After correcting the port assignment so Fa0/2 carried VLAN 10 and Fa0/1 carried the native VLAN, the ping succeeded with 0 percent loss.


<h2>3. Switch 1 (S1) Configuration</h2>

<img src="https://i.imgur.com/Cuhp9ea.png" alt="Okta dashboard showing successfully integrated"/>

Key Takeaways
The native VLAN must match on both ends of a trunk link, or Packet Tracer (and real switches) will flag a mismatch and traffic can be misclassified.
A dedicated blackhole VLAN for unused ports is a simple and effective hardening step, since it denies unauthorized devices any VLAN membership by default.
Careful attention to interface numbering matters. A single digit mix up between Fa0/1 and Fa0/2 was enough to take a whole VLAN's connectivity down.
Router on a stick is an efficient way to route between VLANs without a Layer 3 switch, using subinterfaces tied to VLAN tags on a single physical link.
<h1>Key Takeaways</h1>
<ul>
<li><strong>Provisioned a Microsoft 365 tenant from scratch using publicly available trials, layering in Azure, Entra ID, and Okta as lab scope expanded</li>
<li><strong>Established enterprise standard user provisioning practices including naming conventions, forced password changes, and bulk onboarding</li>
<li><strong>Configured RBAC using the principle of least privilege, assigning scoped roles rather than broad Global Admin access</li>
<li><strong>Security via Isolation (Black-Hole VLANs): By default, unused switch ports auto-negotiate trunks (via dynamic trunking) and sit in VLAN 1 [3, 14]. Disabling dynamic negotiation, forcing access mode, redirecting unused ports to a dead-end VLAN (VLAN 100), and shutting them down ensures unauthorized users cannot plug in and easily access resources</li>
<li><strong>Integrated Okta as an external IdP via SAML 2.0, simulating a federated enterprise identity architecture</li>
</ul>
