# multi-area-ospf-eigrp-route-redistribution-architecture
Advanced Multi-Area Enterprise Routing Network integrating OSPFv2 Area 0 (Backbone), OSPF Area 1 (Non-backbone), an external EIGRP AS 100 routing domain, and an isolated ISP Edge Transit Gateway (8.8.8.8).

🌐 Quick Overview & Network Specs
🟢 Architecture & Topology: Advanced Multi-Area Enterprise Routing Network integrating OSPFv2 Area 0 (Backbone), OSPF Area 1 (Non-backbone), an external EIGRP AS 100 routing domain, and an isolated ISP Edge Transit Gateway (8.8.8.8).

🔵 Router ID & Area Allocations:

Router R2 (Backbone Router): Router ID 2.2.2.2 (Area 0)

Router R3 (Area Border Router / ABR): Router ID 3.3.3.3 (Staddling Area 0 & Area 1)

Router R1 (Autonomous System Boundary Router / ASBR): Router ID 1.1.1.1 (Backbone edge connected to ISP via 203.0.113.0/30)

Router R4 (Internal Router): Router ID 4.4.4.4 (Area 1)

Router R5 (ASBR / EIGRP-OSPF Boundary): Router ID 5.5.5.5 (Staddling Area 1 & EIGRP AS 100)

Router R6 (EIGRP External Router): Router ID 6.6.6.6 (Managing subnets 10.0.1.0/24 through 10.0.5.0/24)

🟣 Transit & Inter-Router Subnets:

Backbone Switch Segment: 123.0.0.0/24 (Interconnecting R1, R2, and R3 via Switch0)

R2–R3 Direct Link: 23.0.0.0/24

R3–R4 Inter-Area Link: 34.0.0.0/24

R4–R5 Area Link: 45.0.0.0/24

R5–R6 EIGRP Domain Link: 56.0.0.0/24

ISP WAN Link: 203.0.113.0/30

💡 Key Technical Insights & What This Lab Demonstrates
🔍 Multi-Protocol Redistribution (ASBR Operations): Proves full mastery of bidirectional route injection between interior OSPF domains and exterior EIGRP Autonomous Systems (EIGRP 100), ensuring complete domain-wide reachability.

⚡ ABR Summarization & Type 3 LSA Propagation: Demonstrates how Router R3 summarizes and translates intra-area topology changes across area boundaries using Type 3 Summary LSAs (O IA).

🛠️ External Type-5 / Type-7 LSAs (O E2): Audits how static default routes and redistributed EIGRP paths are injected into OSPF as external link states, maintaining metric tracking (O E2 / O E1).

🛡️ Routing Hygiene & Loop Prevention: Outlines proper seed metrics, administrative distance handling, and filtering at redistribution boundaries to prevent routing feedback loops between OSPF and EIGRP.

🛠️ Essential Cisco CLI Configuration Snippets
Configuring OSPF-to-EIGRP Redistribution on Router R5
Cisco CLI
router ospf 1
 redistribute eigrp 100 subnets metric-type 1
!
router eigrp 100
 redistribute ospf 1 metric 10000 100 255 1 1500
Configuring Default Route Injection on ASBR Router R1
Cisco CLI
router ospf 1
 default-information originate always metric 20 metric-type 2
🧪 Verification & Audit Commands
Bash
# Verify OSPF and EIGRP neighbor adjacencies across all boundaries
show ip ospf neighbor
show ip eigrp neighbors

# Inspect the routing table to verify OSPF intra-area (O), inter-area (O IA), and external (O E2) entries
show ip route ospf

# Verify redistributed routes in the EIGRP topology table
show ip eigrp topology
