# Resizing Azure Virtual Machines

## Project Summary
* **Lab Name:** Resizing of Virtual Machine
* **Platform:** Microsoft Azure
* **Status:** Completed successfully
* **Validation Methods:** Azure Portal & Azure CLI (Cloud Shell)

---

## Lab Execution and Verification

### 1. Deployed Infrastructure Verification
Prior to executing the resizing procedures, all required supporting resources were verified in the `East US` region[cite: 1, 5]:
* **Virtual Machine:** `MyVM` (Windows Server 2016 Datacenter)[cite: 1, 5]
* **Virtual Network:** `vnet-eastus-1` (`172.16.0.0/16`)[cite: 1, 5]
* **Subnet:** `snet-eastus-1` (`172.16.0.0/24`)[cite: 1]
* **Network Interface:** `myvm669` (Private IP: `172.16.0.4`)[cite: 1, 5]
* **Public IP:** `MyVM-ip` (`20.25.13.212`)[cite: 1, 5]
* **Network Security Group:** `MyVM-nsg` (RDP Port 3389 inbound open)[cite: 1, 5]
* **Storage Account:** `teststorage13`[cite: 1, 5]

![resources.png](resources.png)

### 2. Initial Configuration Review
Inspected the VM size configurations under **MyVM > Settings > Size** to verify the baseline hardware allocation and review available SKUs within the current cluster[cite: 2]:
* **Initial VM SKU:** `Standard_B2s` (2 vCPUs, 4 GiB memory)[cite: 2]

![B2s-size.png](B2s-size.png)

### 3. Scale-Down Execution (Azure Portal)
Executed vertical downscaling via the Azure Portal:
1. Selected the `Standard_B1s` SKU from the sizing blade[cite: 2, 3].
2. Initiated the resize operation[cite: 2].
3. Verified that the VM updated successfully to `Standard B1s` (1 vCPU, 1 GiB memory) in the **Overview** blade[cite: 3].

![change-B1s-size.png](change-B1s-size.png)

### 4. Hardware Availability Check (Azure CLI)
Opened Azure Cloud Shell to query valid resize candidates available on the active compute cluster:

```bash
az vm list-vm-resize-options \
  --resource-group "<Lab-Resource-Group>" \
  --name "MyVM" \
  --output table
