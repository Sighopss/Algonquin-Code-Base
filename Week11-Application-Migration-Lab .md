## Task 1 Setting  Up Tooling for Discovery

**1. Agent-based, agentless, or a mix? which one**  
**Answer:** Tailwind should use a **mix** of agentless and agent-based discovery 
- **Agentless** for the majority of VMs since there is faster rollout
- **Agent-based** only for the small subset of servers where agentless dependency mapping is insufficient this is like legacy OSes or when deeper process level visibility is required

**2. How many appliances do you need and why?**  
**Answer:** **One Azure Migrate appliance** is sufficient for the entire on premises environment 
Reasoning:  
- A single appliance can discover up to 10,000 VMware VMs or 5,000 Hyper VVMs. Tailwind is well below these limits.  
- It supports continuous discovery and replication traffic for Azure Migrate Server Migration

**3. Credentials to be prepared**

| Purpose                     | Required Account Type                                                                 |
|-----------------------------|----------------------------------------------------------------------------------------|
| Software inventory          | local Administrator (Windows) or root for linux                                 |
| SQL discovery               | SQL sysadmin or account with **VIEW SERVER STATE + VIEW ANY DEFINITION**                  |
| Dependency mapping (agentless) | Same as software inventory (local admin/root)                                       |
| Dependency mapping (agent-based) | Local Administrator + Log on as a service right (for MGSi service)                |

**4. Three best practices Tailwind should follow while running discovery**  
1. to validate that the appliance can reach all subnets and has outbound internet access port 443
2. Run discovery during normal business hours for at least 30 days so as to capture accurate performance and dependency data 
3. Regularly monitor the azure project portal keenly to confirm discovery status is Ongoing and the correct data is flowing

---

## Task 2 – Perform Assessment Planning

| Question                                    | Recommendation & Justification                                                                                         |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Production vs non-production assessment     | Create **two separate assessments** one for production workloads and one for non-production (dev/test)                 |
| Target region & performance-history duration| **Primary Azure region:** East US 2 or any close region  <br>**Performance history:** **1 month** (captures monthly cycles) |
| Performance-based vs on-premises (as-is) sizing | **Performance-based sizing**  Tailwind wants rightsizing and cost optimization, not lift and shift of oversized VMs   |
| Comfort factor, pricing, licensing          | **Comfort factor:** 1.3 (30 % headroom)  <br>**Pricing model:** Pay as you go with Azure Hybrid Benefit  <br>**Licensing:** Azure Hybrid Benefit (use existing Windows/SQL licenses) |

---

## Task 3 – The Dependency Analysis

**Application components involved**  
- Web tier servers (WEB01–WEB04)  
- Application tier servers (APP01–APP30)  
- Database tier (SQL01–SQL03 clustered)  
- Hardware load balancers (F5 BIG-IP)  
- Active Directory domain controllers (on-premises)

**dependencies**  
1. WEB ->APP: HTTPS (443 TCP)  
2. APP -> SQL: MS-SQL (1433 TCP)  
3. WEB/APP -> AD: Kerberos/LDAP (88, 389 TCP)  
4. APP -> File Share (SMB 445 TCP)  
5. WEB -> External payment gateway (HTTPS 443 outbound)  
6. SQL -> Backup server (SMB 445)  
7. All servers -> DNS (53 UDP/TCP)  
8. Management -> RDP/WinRM (3389, 5985–5986)

**Items to filter out as noise**  
- Known system processes like svchost.exe and lsass.exe
- RFC-1918 broadcast/multicast traffic  
- Public internet endpoints such as windowsupdate.microsoft.com, *.akamaihd.net, etc
- Azure Migrate appliance traffic itself

**Business requirements collected from application owner**  
| Category                  | Details                                                                 |
|---------------------------|-------------------------------------------------------------------------|
| Business criticality      | LOB Application 1 = Tier 1 (revenue generating)                         |
| Uptime & downtime         | 99.9 percent  SLA allowed 4-hour planned downtime window once per quarter    |
| Data classification       | Contains PCI and PII data                                               |
| Licensing dependencies    | SQL Enterprise tied to hardware; Windows Server licenses with SA       |
| Patching requirements     | Monthly patching during maintenance window                              |
| Firewall & IP considerations | Hard coded IP addresses in several legacy components; cannot change IP |

---

## Task 4 – Validating Assessment Results with Application Owner

**Recommended VM sizes (example from assessment)**  
- Web tier: **Standard_D4s_v5** (4 vCPU, 16 GiB RAM)  
- App tier: **Standard_D8s_v5** (8 vCPU, 32 GiB RAM)  
- SQL tier: **Standard_E16s_v5** + Premium SSD (or move to SQL Managed Instance)

**Components needing replacement/optimization in Azure**  
- On prem F5 load balancers -> Replace with **Azure Application Gateway** or **Azure Load Balancer**  
- SQL Server failover cluster -> Migrate to **Azure SQL Managed Instance**  or keep IaaS with Availability Zones  
- Legacy file shares -> Migrate to **Azure Files** or **NetApp Files**

**Dependency validation**  
All inbound/outbound ports identified in Task 3 are present in the dependency map 

**SLA & downtime**  
Target Azure configuration (Availability Zones + 99.95 % VM SLA) exceeds current 99.9 % requirement

**SQL migration options summary**  
| Option                  | Description                              | Recommended for Tailwind? |
|-------------------------|------------------------------------------|---------------------------|
| Rehost (lift-and-shift) | SQL on Azure VM                          | No (higher management)    |
| Azure SQL Managed Instance | Near 100 % compatibility, built-in HA   | Yes (preferred)           |
| Azure SQL Database      | PaaS single DB / Elastic Pool            | Only for modernized apps  |

---

## Task 5 – Create the Migration Plan 

### Pre-migration tasks
- Freeze application changes 2 weeks prior  
- Deploy Azure Application Gateway (new public IPs)  
- Replicate VMs using Azure Migrate (test failover first)  
- Update DNS TTL to 5 minutes 24 h before cutover  

### Migration steps (per wave)
1. Failover Wave -> Verify in Azure  
2. Update internal DNS records to point to new Azure private IPs  
3. Update connection strings in app config to new SQL MI endpoint  
4. Re-point load balancer health probes and backend pools  
5. Run smoke tests  

### DNS & connection string changes
- CNAME www.tailwind.com -> Azure Application Gateway public IP  
- Update web.config / appsettings.json: `Server=tailwind-mi.public.dns.zone.database.windows.net`

### Load balancer considerations
- Replace F5 with Azure Application Gateway (WAF enabled)  
- Session affinity configured (cookie-based)

### SQL migration considerations
- Use Azure DMS (online) to SQL Managed Instance  
- Minimal downtime (log shipping + final cutover)

### Post-migration validation checklist
- Application login & core transactions working  
- Monitoring (Azure Monitor + Log Analytics) receiving data  
- Backup jobs running in Azure Backup  
- Performance baseline matches or exceeds on-prem

### Back-out plan
- Keep on-premises VMs powered on (paused replication) for 72 h  
- Revert DNS to on-prem IPs if critical failures occur  
- Documented rollback window: 4 hours

---

## Task 6 – Plan the Migration Waves

| Wave | Servers / Components                              | Reason / Justification                                                                                 |
|------|---------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| 1    | WEB01–WEB04 (4 web servers)                       | Stateless, low risk, quick rollback, validates networking & load balancer                           |
| 2    | APP01–APP30 (30 application servers)             | Depends on Wave 1; medium risk; can scale horizontally if issues occur                               |
| 3    | SQL01–SQL03 + File shares + AD connectors        | Highest risk; contains customer data; longest cutover time; requires extended testing & validation  |

**Final wave justification**  
- Follows natural application tiers (web -> app _>  data)  
- Minimizes business impact (revenue-facing web tier proven first)  
- Allows full regression testing after each wave  
- Meets the 4 hour quarterly maintenance window constraint
