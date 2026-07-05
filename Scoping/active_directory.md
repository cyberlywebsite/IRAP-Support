# Windows Endpoint Management & Join State Assessment Guide

An IRAP assessor should first determine how the endpoint is managed. Whether a system is **Standalone (Workgroup)**, **Active Directory (AD) domain-joined**, or **Microsoft Entra ID joined** dictates which Information Security Manual (ISM) controls apply for authentication, policy enforcement, and centralised management.

---

## 1. Checking Active Directory & Workgroup Status

### Check if the Device is Domain-Joined
Run the following PowerShell command to check the domain membership status:

```powershell
(Get-CimInstance -ClassName Win32_ComputerSystem).PartOfDomain
```

#### Expected Results

| Result | Meaning |
| :--- | :--- |
| `False` | The device is **Standalone (Workgroup)**. |
| `True` | The device is **Active Directory Domain-Joined**. |

---

### Display the Domain or Workgroup Name
To identify the specific domain or workgroup name the device belongs to, run:

```powershell
(Get-CimInstance -ClassName Win32_ComputerSystem).Domain
```

#### Example Output

| Output | Meaning |
| :--- | :--- |
| `WORKGROUP` | Standalone device |
| `CORP.LOCAL` | Active Directory domain |

---

## 2. Checking Microsoft Entra ID (Azure AD) Join Status

For modern Windows Standard Operating Environments (SOEs), verify whether the device is joined to Microsoft Entra ID or operating in a Hybrid configuration.

Run the following command in a command prompt or PowerShell:

```cmd
dsregcmd /status
```

Review the following fields under the **Device State** section of the output:

| Field | Meaning |
| :--- | :--- |
| `AzureAdJoined : YES` | Device is Microsoft Entra ID joined |
| `DomainJoined : YES` | Device is Active Directory domain joined |
| `Both YES` | Hybrid Active Directory + Entra ID joined |
| `Both NO` | Standalone device (not centrally joined) |

---

> [!TIP]
> **IRAP Assessment Tip:** Determining the device join state should be one of the first checks performed during an SOE assessment, as it influences how many ISM controls (such as Group Policy, identity management, authentication, and configuration enforcement) are expected to be implemented.