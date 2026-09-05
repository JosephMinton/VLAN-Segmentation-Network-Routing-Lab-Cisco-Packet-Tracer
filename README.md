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
    <td>PC-E</td>
    <td>Staff</td>
    <td>20</td>
    <td><code>172.16.2.20</code></td>
    <td><code>255.255.255.0</code></td>
    <td><code>172.16.2.1</code></td>
  </tr>
  <tr>
    <td>PC-F</td>
    <td>Sales</td>
    <td>30</td>
    <td><code>172.16.3.20</code></td>
    <td><code>255.255.255.0</code></td>
    <td><code>172.16.3.1</code></td>
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

<h4>PC-E (VLAN 20, Staff)</h4>
<ul>
  <li>IP Address: <code>172.16.2.20</code></li>
  <li>Subnet Mask: <code>255.255.255.0</code></li>
  <li>Default Gateway: <code>172.16.2.1</code></li>
</ul>

<h4>PC-F (VLAN 30, Sales)</h4>
<ul>
  <li>IP Address: <code>172.16.3.20</code></li>
  <li>Subnet Mask: <code>255.255.255.0</code></li>
  <li>Default Gateway: <code>172.16.3.1</code></li>
</ul>

<h2>2. VLAN Creation and Access Port Assignment</h2>
<p>VLANs 10 (HR), 20 (Staff), and 30 (Sales) were created on both switches, and access ports were assigned so each PC lands in the correct VLAN.</p>
<p>Initial state check on CopySwtich1, before any port hardening. VLAN 1 still holds every unused port (Fa0/6 through Fa0/24, Gig0/1).</p>
<img src="https://i.imgur.com/r4ASGGE.png)" alt="br"/>

<h2>3. Trunk Configuration</h2>
<p>The link between CopySwitch0 and CopySwtich1 was configured as a trunk, carrying VLANs 10, 20, and 30 between the switches.</p>
<h2>4. Unused Port Hardening (Blackhole VLAN)</h2>
<p>While configuring the trunk on CopySwitch0, a %CDP-4-NATIVE_VLAN_MISMATCH warning appeared on Fa0/4, since CopySwitch0's native VLAN was set to 40 while CopySwtich1 was still on the default native VLAN of 1. VLAN 100 was also created here as a dedicated BLACKHOLE VLAN, and unused ports were moved into it so an unauthorized device plugged into an idle port has no path onto the network.</p>
<img src="https://i.imgur.com/7JA6iAu.png" alt="Switch0Blackhole"/>

<h3>5. Router on a Stick</h3>
<p>Router1 was configured with subinterfaces for each VLAN (Fa0/1.10, Fa0/1.20, Fa0/1.30) to route traffic between VLANs.</p>

<h2>Verification</h2>

<h3>Ping Test and Port to VLAN Fix</h3>
<p>A ping to 172.16.1.20 failed with 100 percent packet loss, tracing back to a port assignment mix up on CopySwtich1 (see Mistakes and Troubleshooting below).</p>

<img src="https://i.imgur.com/CLMp2la.png" alt="FailedPing"/>

<p>The fix, correcting which VLAN lived on Fa0/1 versus Fa0/2:</p>
<img src="https://i.imgur.com/5KU0BaT.png" alt="1to2p1"/>
<img src="https://i.imgur.com/HhzF843.png" alt="1to2p2"/>

<p>Retesting the same ping confirms the fix, shown here failing then succeeding back to back:</p> 
<img src="https://i.imgur.com/HNpsh0H.png" alt="fix"/>

<h2>Trunk and EtherChannel State</h2>
<p><code>show interfaces trunk</code> confirms the trunk is up and carrying all three VLANs plus the native VLAN:</p>
<img src="https://i.imgur.com/hWE9nxF.png" alt="Trunk Test"/>

<p><code>show etherchannel summary</code> confirms no accidental port channel formed on the trunk link:</p>
<img src="https://i.imgur.com/JePL1Au.png" alt="eth summary"/>

<h2>Router Subinterface State</h2>
<p><code>show ip interface brief</code> on Router1 confirms the subinterfaces are up and passing traffic:</p>
<img src="https://i.imgur.com/HbXuB72.png" alt="state up"/>

<h2>Final Connectivity Test</h2> 
<p>A final ping to 172.16.2.10 confirms cross VLAN reachability through the router, closing out verification.</p>
<img src="https://i.imgur.com/aITDSsy.png" alt="final test"/>

<h2>Mistakes and Troubleshooting</h2>
<h4>Mistake 1: Native VLAN Mismatch</h4>
<p>While configuring the trunk, the switches logged a <code>%CDP-4-NATIVE_VLAN_MISMATCH</code> warning. CopySwitch0 had the trunk native VLAN set to 40, while CopySwtich1 was still on the default native VLAN of 1. A native VLAN mismatch means untagged traffic on the trunk is interpreted as belonging to different VLANs on each end, which breaks Spanning Tree and can leak traffic between VLANs. This was resolved by setting the native VLAN to 40 on both ends of the trunk so they agreed.</p>

<h4>Mistake 2: Port to VLAN Mix Up</h4>
<p>While assigning access ports on the second switch, VLAN 40 was mistakenly applied to Fa0/1 instead of Fa0/2, which is where VLAN 10 needed to live. A follow up typo (<code>inf f0/2</code> instead of int <code>f0/2</code>) also returned an invalid input error before the correct interface was reached. The result was that the PC at 172.16.1.20 could not be reached, showing 100 percent packet loss. After correcting the port assignment so Fa0/2 carried VLAN 10 and Fa0/1 carried the native VLAN, the ping succeeded with 0 percent loss.</p>


<h1>Key Takeaways</h1>
<ul>
<li><strong>The native VLAN must match on both ends of a trunk link, or Packet Tracer (and real switches) will flag a mismatch and traffic can be misclassified.</li>
<li><strong>A dedicated blackhole VLAN for unused ports is a simple and effective hardening step, since it denies unauthorized devices any VLAN membership by default.</li>
<li><strong>Careful attention to interface numbering matters. A single digit mix up between Fa0/1 and Fa0/2 was enough to take a whole VLAN's connectivity down.</li>
<li><strong>Router on a stick is an efficient way to route between VLANs without a Layer 3 switch, using subinterfaces tied to VLAN tags on a single physical link.</li>
</ul>
