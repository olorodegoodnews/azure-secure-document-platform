# Secure Document Platform — Network Configuration

# DAY-2

## Overview

This project documents the network configuration completed for the Secure Document Platform in Microsoft Azure.

The network configuration follows a private-access architecture using an Azure Virtual Network, dedicated subnets, Network Security Groups, and Private Endpoints.

This documentation covers the network engineering tasks completed for the project, with emphasis on the Storage Account Private Endpoint.

---

## Network Resources

| Resource                   | Name                           |
| -------------------------- | ------------------------------ |
| Resource Group             | `rg-secure-document-platform`  |
| Virtual Network            | `vn-secure-document-platform`  |
| Private Endpoint Subnet    | `Private-Endpoint-Subnet`      |
| Storage Account            | `securedoceast01`              |
| Storage Private Endpoint   | `storage-private-endpoint`     |
| Private Endpoint NIC       | `storage-private-endpoint-nic` |
| Application Security Group | `App-Security-group`           |
| Azure Region               | `East US`                      |

---

# Storage Account Private Endpoint

## Objective

Create a Private Endpoint for the Storage Account after receiving confirmation from the Storage Engineers.

The Private Endpoint was configured to:

* Connect to the `securedoceast01` Storage Account
* Use the `blob` Private Link resource
* Be placed in the `Private-Endpoint-Subnet`
* Use the `vn-secure-document-platform` Virtual Network
* Verify that the Private Link connection was successfully approved

---

## Private Endpoint Configuration

### Private Endpoint Name

`storage-private-endpoint`

### Virtual Network

`vn-secure-document-platform`

### Subnet

`Private-Endpoint-Subnet`

### Target Storage Account

`securedoceast01`

### Private Link Resource

`securedoceast01`

### Group ID

`blob`

### Connection Status

`Approved`

### Provisioning State

`Succeeded`

### Approval

`Auto-Approved`

### Actions Required

`None`

---

## Network Interface

The Private Endpoint created the following Network Interface:

`storage-private-endpoint-nic`

The NIC is associated with the Storage Account Private Endpoint and provides the network connectivity required for the Private Link connection.

---

## Application Security Group

The Private Endpoint is associated with:

`App-Security-group`

This provides integration with the project's network security design.

---

# Verification

The Private Endpoint configuration was verified using the Azure resource information.

The verification confirmed:

```text
Provisioning State: Succeeded
Connection Status: Approved
Description: Auto-Approved
Actions Required: None
Group ID: blob
Subnet: Private-Endpoint-Subnet
Virtual Network: vn-secure-document-platform
Storage Account: securedoceast01
```

These results confirm that the Storage Account Private Endpoint was successfully provisioned and connected to the Storage Account.

---

# Network Engineer 1 Deliverable

The completed Network Engineer 1 deliverable is:

**Working Storage Account Private Endpoint**

The Private Endpoint is:

* Successfully provisioned
* Connected to the Storage Account
* Located in the designated Private Endpoint subnet
* Connected using the Blob Private Link resource
* Approved and ready for use

---

# Evidence

Screenshots documenting the completed configuration should be added to the project documentation.

1. Private Endpoint Overview

![Private Endpoint Overview](../Screenshots/Private-Endpoint-overview.png)

2. Private Endpoint Networking/Subnet configuration

![Private Endpoint Networking-Subnet configuration](../Screenshots/Private-Endpoint-vn..subnet.png)

3. Storage Account Private Endpoint connection

![Storage Account Private Endpoint connection](../Screenshots/Private-Endpoint-status.png)

---

## Status

**Storage Account Private Endpoint: COMPLETED ✅**
