Key assumptions:
- **VM Sizing**: Web front-end uses D2s v5 (2 vCPU, 8 GB RAM, Linux); API back-end and payment use D4s v5 (4 vCPU, 16 GB RAM, Linux); ML uses NC6s_v3 (6 vCPU, 112 GB RAM, 1x V100 GPU, intermittent usage at 200 hours/month).
- **Database Scaling**: SQL Hyperscale (8 vCores for 5 TB); Cosmos DB (10,000 RU/s for 10 TB); Synapse Analytics Dedicated SQL Pool (DW1500c for 15 TB).
- **Traffic/Usage**: CDN at 10 TB/month per region (30 TB total); data transfer for migration: 30 TB outbound.
- **Hours/Month**: 730 for continuous resources.
- **Free Tiers**: Applied where applicable (e.g., first 100 GB outbound free; DMS Standard tier free).
- Prices exclude taxes/VAT and potential discounts unless specified.

Total estimated **migration cost**: $4,457 (one-time).  
Total estimated **monthly operational cost (PAYG)**: $28,892.  
Total estimated **monthly management cost**: $300.  
Annual operational run rate (PAYG): ~$346,704.

## 1. Migration Cost (One-Time)
This covers data transfer, database replication via Azure Database Migration Service (DMS), and temporary resources for lift-and-shift (e.g., replication VMs for 1 week).

| Component                  | Estimated Cost | Notes |
|----------------------------|----------------|-------|
| Outbound Data Transfer (30 TB) | $2,457 | Tiered: First 100 GB free; next 9.9 TB at $0.087/GB ($861); remaining 20 TB at $0.08/GB ($1,600). Based on Zone 1 rates. |
| Database Migration Service (DMS) | $0 | Standard tier (up to 4 vCores) is free for online migrations. |
| Temporary Migration Resources | $2,000 | 5x D2s v5 VMs for 168 hours (~$80) + minor storage/replication overhead. |
| **Total**                  | **$4,457**    | Conservative estimate; actual may vary with tool usage. |

## 2. Operational Cost (Monthly, PAYG)
This includes compute, storage, networking, and scaling across regions. Auto-scaling assumed for API (20% average utilization savings applied post-optimization). Resources are multi-region where noted.

| Component                  | Quantity/Details                  | Unit Cost (East US) | Monthly Cost | Notes |
|----------------------------|-----------------------------------|---------------------|--------------|-------|
| **Web Front-End VMs**     | 30x D2s v5 (10/region)           | $70.08/VM          | $2,102      | High availability via Availability Zones (no extra cost). |
| **API Back-End VMs**      | 20x D4s v5 (global)              | $140.16/VM         | $2,803      | Auto-scaling groups; assumes 80% utilization. |
| **Payment Processing VMs**| 5x D4s v5 (global, PCI-compliant)| $140.16/VM         | $701        | Includes encryption (no extra VM cost). |
| **ML Processing VMs**     | 2x NC6s_v3 (200 hrs/month)       | $3.06/hr           | $1,226      | GPU for batch jobs; intermittent usage ($3.06/hr x 200 hrs x 2 VMs). |
| **Primary Database (SQL Hyperscale)** | 8 vCores + 5 TB storage | Compute: $5.40/hr; Storage: $0.25/GB | $5,222      | Compute ($3,942) + storage ($1,280); NA primary, read replicas in other regions (+10% cost included). |
| **Analytics Database (Cosmos DB)** | 10,000 RU/s + 10 TB storage | RU/s: $0.008/100/hr; Storage: $0.25/GB | $3,144      | Compute ($584) + storage ($2,560); multi-region writes. |
| **Data Warehouse (Synapse)** | DW1500c + 15 TB storage | Compute: $13,140; Storage: $0.07/GB | $14,215     | Compute full + storage ($1,075); on-demand scaling for reports/ML. |
| **Backup/DR Storage**     | 30 TB GRS (geo-redundant)        | $0.04/GB           | $1,200      | Hot tier Blob storage; replicated across regions. |
| **Load Balancers**        | 3x Standard (1/region)           | $10/LB             | $30         | Fixed + minor data processing. |
| **CDN**                   | 30 TB egress (10 TB/region)      | $0.081/GB          | $2,430      | Standard Microsoft tier, Zone 1. |
| **Networking/Misc**       | VNets, IPs, bandwidth            | N/A                | $100        | Intra-region free; minor inter-region. |
| **Subtotal (Pre-Scaling Adjustment)** | - | - | $33,153 | - |
| **Auto-Scaling Savings**  | API/ML (20% reduction)           | -                  | -$4,261     | Peak/off-peak adjustment. |
| **Total**                 | -                                | -                  | **$28,892** | Multi-region multiplier applied; excludes inbound transfer (free). |

## 3. Management Cost (Monthly)
Covers monitoring, logging, and cost tracking tools.

| Component                  | Details                           | Monthly Cost | Notes |
|----------------------------|-----------------------------------|--------------|-------|
| **Azure Monitor**         | 100 GB logs ingested              | $230         | $2.30/GB for Analytics Logs; includes metrics/alerts. |
| **Cost Management + Billing** | Budgets, forecasts, tagging      | $0           | Free core features. |
| **Security/Compliance Tools** | Basic Sentinel alerts            | $70          | Entry-level; PCI scanning included. |
| **Total**                  | -                                | **$300**    | Scalable; free tier for basic metrics. |

## Cost Optimization Strategy
To achieve cost-effectiveness, it is recommended a mix of Azure-native features and commitments. Current PAYG baseline: $29,192/month ($350,304/year). Optimizations can yield 40-60% savings.

### 1. Reserved Instances (RI) vs. PAYG Comparison
RIs offer up to 72% savings for 1-3 year commitments (no upfront for monthly payments).

| Resource Group             | PAYG Monthly | 1-Year RI (30% Savings) | 3-Year RI (60% Savings) | Annual Savings (3-Year) |
|----------------------------|--------------|-------------------------|-------------------------|-------------------------|
| VMs (Web + API + Payment) | $5,606      | $3,924                 | $2,242                 | $39,792                |
| Databases (All)           | $22,581     | $15,807                | $9,032                 | $162,588               |
| **Total Impact**          | **$28,187** | **$19,731**            | **$11,274**            | **$202,380**           |

- **Recommendation**: Commit to 3-year RIs for steady workloads (e.g., databases, web VMs) for ~60% savings ($17,000/month reduction). Use Spot VMs for ML batch jobs (up to 90% off).

### 2. Other Savings Opportunities
- **AutoShutdown & Right Sizing**: schedule non-peak shutdowns (e.g., ML VMs) and use Azure Advisor for resizing; estimated 15% savings ($4,300/month).
- **Azure Hybrid Benefit**: If using existing Windows licenses, save up to 40% on Windows VMs (not applied here; Linux assumed—reassess for $500+/month savings).
- **Tagging & Cost Allocation**: Implement tags for resource groups (e.g., by region/app) to track usage; enables budgets/alerts for 5-10% waste reduction.
- **Serverless Shift**: Migrate batch jobs to Azure Functions ($0.20/million executions) vs. VMs; potential $1,000/month savings on ML.
- **Free Tiers/Discounts**: Leverage 400 RU/s + 25 GB Cosmos free tier ($50/month savings); multi-year commitments for CDN (10% off).

**Net Optimized Monthly Cost**: $14,000 (50% reduction via RI + scaling).

## Future Growth and Budget Plan (3-Year Projection)
Assuming 20% annual growth in users/traffic (e.g., +20% VMs, +2 TB storage/year), baseline PAYG escalates without optimizations. We project optimized costs incorporating annual reviews.

| Year | Projected Growth Factor | PAYG Annual Cost | Optimized Annual Cost (w/ RI + 10% Efficiency Gains/Year) | Key Adjustments |
|------|-------------------------|------------------|------------------------------------------------------------|-----------------|
| **1 (2026)** | 1.0x (Baseline) | $350,304 | $201,600 (42% savings) | Initial RI commitments; auto-scaling implementation. |
| **2 (2027)** | 1.2x | $420,365 | $221,760 (47% savings) | Migrate ML to serverless; right-size databases via Advisor. |
| **3 (2028)** | 1.44x | $504,438 | $244,000 (52% savings) | Upgrade to newer VM series (e.g., Dsv6 for 20% better price/perf); explore Azure Savings Plans. |
| **Total (3 Years)** | - | **$1,275,107** | **$667,360** | Cumulative savings: $607,747. |

### Strategic Recommendations for Futur
- **Annual Audits**: Use Azure Cost Management for quarterly forecasts; set 10% buffer for growth
- **Scalability Features**: Adopt Azure Kubernetes Service (AKS) for microservices (potential 25% savings vs. VMs)
- **Risk Mitigation**: Monitor for 15% YoY inflation in pricing; hedge with 3-year RIs
- **ROI Projection**: Break-even on migration within 6 months; 3-year TCO savings of 50% vs. on-premises

