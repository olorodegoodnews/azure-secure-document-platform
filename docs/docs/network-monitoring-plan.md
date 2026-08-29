# Network Monitoring Plan: Storage Account Private Endpoint Network Security

**Author:** Kehinde Oyewumi
**Resource Group:** rg-secure-document-platform
**Region:** East US
**Date:** August 29, 2026
**Status:** Network security and DNS configuration complete. Private Endpoint intentionally not yet connected (see Scope).

---

## Executive Summary

This document records the network security and private DNS configuration prepared for a Storage Account Private Endpoint deployment. The work covers creation and association of a Network Security Group (NSG) on the subnet designated for private endpoints, and provisioning of the Private DNS zone and virtual network link required for private endpoint name resolution.

As specified in the task scope, the Private Endpoint itself has not been connected to the Blob Storage account at this stage. This document covers only the network security and DNS groundwork that must be in place before that connection is made.

---

## Environment Details

| Setting | Value |
|---|---|
| Subscription ID | a9409c2e-e78f-42f4-aebb-ed513af6a5c4 |
| Resource Group | rg-secure-document-platform |
| Region | East US |
| Virtual Network | vn-secure-document-platform |
| Subnet Name | Private-Endpoint-Subnet |
| Subnet Address Range | 10.0.1.0/24 |
| DNS Zone Name | privatelink.blob.core.windows.net |
| NSG Name | nsg_storage_subnet |
| VNet Link Name | link-vnet |

---

## 1. Network Security Group (NSG)

An NSG was created and applied to the subnet that will host the Storage Account Private Endpoint, to control traffic at the subnet boundary.

**Configuration:**
- Name: `nsg_storage_subnet`
- Resource Group: `rg-secure-document-platform`
- Location: East US
- Custom rules: None added. Default rules only.

### Inbound Rules

| Priority | Name | Purpose |
|---|---|---|
| 65000 | AllowVnetInBound | Allows traffic between resources within the same virtual network |
| 65001 | AllowAzureLoadBalancing | Allows Azure's health probe and load balancer traffic |
| 65500 | DenyAllInbound | Denies all other inbound traffic not explicitly allowed above |

### Outbound Rules

| Priority | Name | Purpose |
|---|---|---|
| 65000 | AllowVnetOutBound | Allows outbound traffic between resources within the same virtual network |

**Screenshot reference:** `images/1.png` (NSG deployment completion), `images/2.png` (NSG rules overview)

---

## 2. NSG to Subnet Association

The NSG was associated with the subnet reserved for the private endpoint.

| Setting | Value |
|---|---|
| Associated Subnet | Private-Endpoint-Subnet |
| Address Range | 10.0.1.0/24 |
| Virtual Network | vn-secure-document-platform |
| Association Status | Connected |

**Screenshot reference:** `images/3.png` (NSG subnet association)

---

## 3. Private DNS Zone

A Private DNS zone was provisioned to support name resolution for the private endpoint once it is connected. The zone name follows Azure's required naming convention for Storage Account blob private endpoints.

| Setting | Value |
|---|---|
| Zone Name | privatelink.blob.core.windows.net |
| Resource Group | rg-secure-document-platform |
| Location | Global |
| Deployment Status | Succeeded |
| Deployment Time | August 29, 2026, 8:04:27 PM |

**Screenshot reference:** `images/4.png` (Private DNS zones list), `images/5.png` (DNS zone deployment confirmation)

---

## 4. Virtual Network Link

A virtual network link was created to connect the Private DNS zone to the virtual network, so that resources inside the VNet can resolve the private endpoint's DNS name once it exists.

| Setting | Value |
|---|---|
| Link Name | link-vnet |
| Linked Virtual Network | vn-secure-document-platform |
| Status | Completed |
| Auto-Registration | Disabled |
| Fallback to Internet | Disabled |

Auto-registration was deliberately left disabled to keep the DNS zone limited to explicitly created private endpoint records rather than automatically registering every VM in the VNet. Fallback to internet resolution was disabled so that lookups for this zone never fall back to public DNS, which is the correct setting for a private-only resolution path.

**Screenshot reference:** `images/6.png` (Virtual Network Links page)

---

## 5. Private Endpoint Status

**The Private Endpoint has intentionally not been connected to the Blob Storage account at this stage.** This is a task requirement, not an oversight. The network security and DNS groundwork above is in place so that the private endpoint connection can be added in a later step without additional network configuration.

---

## Security Considerations

- The NSG's default `DenyAllInbound` rule ensures no inbound traffic reaches the subnet unless explicitly permitted, following the principle of least privilege at the network boundary.
- No custom inbound rules were added, since the private endpoint has not yet been connected and there is no service on this subnet yet requiring inbound access beyond the default VNet and load balancer rules.
- Disabling auto-registration on the VNet link prevents unintended DNS records from being created automatically, keeping the private DNS zone limited to intentional entries.
- Disabling fallback to internet resolution ensures that if a lookup against this private zone fails, it does not silently resolve through public DNS, which would defeat the purpose of using a private endpoint.
- The subscription ID and resource identifiers in this document are internal identifiers, not credentials. No secrets, keys, or passwords are included in this documentation.

---

## Troubleshooting Guide

| Issue | Cause | Resolution |
|---|---|---|
| Virtual Network Links option not visible | Insufficient role assignment on the Private DNS zone or VNet | Request Network Contributor or equivalent role from an administrator |
| VNet link shows red/failed status | Link name conflict or propagation delay | Use a unique link name (this deployment used `link-vnet` after an earlier attempt failed) and allow a few minutes for status to update |
| NSG association not taking effect | Subnet already associated with a different NSG | Confirm only one NSG is associated per subnet before reattempting |
| DNS zone deployment fails | Zone name does not exactly match the required private link namespace | Confirm the zone name is exactly `privatelink.blob.core.windows.net` with no typos |

**Issue encountered during this deployment:** Initial permission restrictions prevented access to the Virtual Network Links option. This was resolved after the team lead granted the necessary role assignment.

---

## Validation Checklist

- [x] NSG `nsg_storage_subnet` created in `rg-secure-document-platform`
- [x] NSG associated with `Private-Endpoint-Subnet` (10.0.1.0/24)
- [x] Default inbound rules confirmed, including `DenyAllInbound`
- [x] Private DNS zone `privatelink.blob.core.windows.net` deployed successfully
- [x] Virtual network link `link-vnet` created and linked to `vn-secure-document-platform`
- [x] Auto-registration disabled on the VNet link
- [x] Fallback to internet disabled on the VNet link
- [x] Private Endpoint confirmed NOT connected, per task scope
- [x] All 6 supporting screenshots captured and referenced

---

## Lessons Learned

- NSG default rules alone provide a reasonable baseline of subnet-level security before any custom rules are needed.
- Private DNS zone names must match Azure's required naming convention exactly; there is no flexibility in the zone name for a given service.
- Virtual network links require specific role permissions that are not always included in a standard contributor role, and may need to be requested separately.
- Disabling auto-registration is generally the better default for private endpoint scenarios, since it keeps the DNS zone limited to intentional, explicitly created records.

---

## Screenshot Index

| File | Description |
|---|---|
| `images/1.png` | NSG deployment completion confirmation |
| `images/2.png` | NSG rules overview (inbound and outbound) |
| `images/3.png` | NSG to subnet association confirmation |
| `images/4.png` | Private DNS zones list view |
| `images/5.png` | Private DNS zone deployment confirmation |
| `images/6.png` | Virtual Network Links page showing connected status |

---

*This document reflects the network security and DNS configuration state as of August 29, 2026. The Private Endpoint connection step is intentionally out of scope for this document and will be covered in a subsequent update once completed.*
