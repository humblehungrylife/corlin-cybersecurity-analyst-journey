# Week 05 — Network Like an Operative

**Mad Hat Labs | Chapter 5: Virtual Networking**

**Status:** Five-hop training network trace completed; published as a redacted ticket reply.  
**Scenario role:** Junior analyst responding to an application-team connectivity question.  
**Access:** Azure Reader — configuration inspection only.  
**Environment:** Live multi-user Azure training tenant. **Not production experience.**

## The ticket

> Our new service reaches its customer-data storage dependency, but we cannot explain the network path. Can someone trace the connection and identify the network controls involved?

I investigated the configuration from the workload subnet to the storage service without changing resources. The five sections below describe what I inspected, what the evidence supports, and what each control means for connectivity.

## The trace

### 1. THE VNET — Where does the traffic originate?

**Checked:** Virtual Network (VNet) address space and workload-subnet configuration. **Observed:** The workload subnet uses a /24 slice within a larger /16 VNet. A Network Security Group (NSG) and route table are associated with this subnet. **Meaning:** The subnet identifies the origin and the two controls to inspect next; the /16 and /24 have different roles and must not be confused.

### 2. THE NSG — What does the filter permit?

**Checked:** Inbound and outbound rules attached to the workload subnet. **Observed:** A narrowly scoped inbound HTTPS exception precedes a custom Internet deny rule; default VNet outbound traffic remains permitted. **Meaning:** Inbound management/application access is constrained by specific rules. The separate outbound connection to the private storage dependency is compatible with the NSG's default internal-traffic rules. An inbound allow rule is not proof of the storage connection's source.

### 3. THE ROUTE — Why can traffic bypass the appliance?

**Checked:** The subnet-associated route table and its next-hop type. **Observed:** A default User-Defined Route (UDR) for `0.0.0.0/0` points toward a virtual appliance. **Meaning:** That route is useful for destinations that do not match a more-specific route. The Private Endpoint introduces a destination-specific `/32` route in the lab's routing model. Azure selects the longest matching prefix: `/32` outranks `/0` for this private destination, so the default appliance path is not selected.

### 4. THE PRIVATE ENDPOINT — How does storage remain privately reachable?

**Checked:** The Private Endpoint network interface and DNS configuration. **Observed:** The endpoint has a private address drawn from the workload network. The exercise specifies that public network access on the storage resource is disabled. **Meaning:** The workload has a private connection to its storage dependency through Azure Private Link. I did not separately capture the storage service's public-network-access setting, so that assertion is attributed to the exercise rather than claimed as an independent portal verification.

### 5. THE NAME — How does the application find the endpoint?

**Checked:** The private Domain Name System (DNS) zone and its address record. **Observed:** The record points the private-link storage name toward the same private endpoint address shown in the endpoint configuration. **Meaning:** With the correct private DNS zone linked and resolution in place, the application continues using the storage hostname, not a hardcoded private address. DNS resolves the destination first; it does not transport the storage data packets.

## The answer — Reply to the application team

The application requests storage by hostname. Private DNS resolves the storage name to the Private Endpoint's private address in the workload network. Outbound internal traffic is compatible with the NSG rules. During route selection, the endpoint's more-specific `/32` path takes precedence over the `0.0.0.0/0` default route to the appliance. The configured private connection therefore explains how storage remains reachable without the default firewall path being selected.

This was a **Reader-access configuration trace**, not a live packet capture or a verification of firewall log ingestion. I made no changes.

## What I would flag

Confirm with the network owner whether Private Endpoint traffic bypassing centralized appliance inspection is **intended**. If inspection is required, review the supported Private Endpoint network policies, effective routes, and security design; do not assume that making a firewall route `/32` automatically fixes the architecture.

## Evidence and interactive trace

**[Interactive five-hop network trace — open the GitHub Pages walkthrough](https://humblehungrylife.github.io/corlin-cybersecurity-analyst-journey/week-05/interactive-trace/)**

The walkthrough animates a **conceptual reconstruction** of this case and offers one sanitized evidence illustration per hop. It is not connected to the training tenant, and it cannot display live packets.

- [Hop 1 — VNet and subnet evidence](evidence/hop-01-vnet.svg)
- [Hop 2 — NSG rule review](evidence/hop-02-nsg.svg)
- [Hop 3 — Route-table review](evidence/hop-03-routing.svg)
- [Hop 4 — Private Endpoint review](evidence/hop-04-endpoint.svg)
- [Hop 5 — Private DNS record review](evidence/hop-05-dns.svg)

**Evidence limits:** The five visuals are redacted *reconstructions of portal configuration views*, not pixel-exact screenshots. Original screenshots remain private because they contain mission answers, resource identifiers, and tenant details. The lab provided the Private Endpoint `/32` route-selection explanation; no independent effective-route or packet-capture output is published. Screenshots and the page omit real source IPs, next-hop addresses, endpoint IPs, service names, subscription identifiers, and answer strings.

## What I learned

- Network Security Groups decide whether matching traffic is allowed; route tables decide the next hop; DNS identifies the destination address.
- A broad `/0` UDR is **not useless**. More-specific matching routes can take precedence for selected destinations.
- A Private Endpoint provides private connectivity but does not itself disable public network access on the service.
- An inbound HTTPS exception is not evidence of an outbound storage connection.
- Reader-level cloud access can support a useful, clearly qualified troubleshooting explanation without making changes.

## Interview-ready summary

I traced a storage connection through subnet associations, NSG rules, routing, Private Endpoint configuration, and private DNS using Azure Reader permissions. I explained why Private Endpoint routing can take precedence over a forced-tunnel default route and delivered a concise answer to the requesting application team.

## Training acknowledgment and disclosure

Thank you to **Mad Hat Labs** for the practical learning environment. This is an independently written learning case study, not an official lesson or an answer key.

I do not publish flags, precise mission values, target resource identifiers, portal answer sequences, or unredacted screenshots. See the repository [publication policy](../../PUBLICATION-POLICY.md).

**Experience classification: Read-only live Azure training investigation, not production administration.**