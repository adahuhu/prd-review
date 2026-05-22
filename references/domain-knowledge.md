# Cross-Border E-Commerce SaaS Domain Knowledge

> This document provides domain-specific knowledge for reviewing PRDs in cross-border e-commerce SaaS products (ERP/WMS/SCM).

---

## Industry Overview

Cross-border e-commerce involves selling products from one country to customers in another, requiring coordination across:
- **Multiple sales platforms** (Amazon, eBay, Shopify, Walmart, 天猫国际, 京东全球购, etc.)
- **Complex logistics** (international shipping, customs clearance, last-mile delivery)
- **Multi-currency & tax compliance** (exchange rates, VAT, tariffs)
- **Cross-border regulations** (import/export restrictions, product certifications)

---

## 1. ERP (Enterprise Resource Planning) Domain

### 1.1 Core Modules

#### Order Management (订单管理)
**Key concepts:**
- **Order capture**: Automatically pulling orders from multiple platforms
- **Order consolidation**: Merging multiple orders from same customer
- **Order splitting**: Splitting orders for partial shipment or multi-warehouse fulfillment
- **Order status synchronization**: Keeping order status consistent across platforms
- **Exception handling**: Address validation, fraud detection, payment failure

**Common requirements to check:**
- How are multi-platform orders aggregated?
- How are order status updates synced back to platforms?
- How are partial shipments handled?
- What happens when platform API is unavailable?

#### Inventory Management (库存管理)
**Key concepts:**
- **Multi-warehouse inventory**: Tracking stock across domestic warehouse, overseas warehouses, FBA
- **Safety stock**: Buffer inventory to prevent stockouts
- **Inventory reservation**: Allocating stock to pending orders
- **Inventory周转率 (turnover rate)**: How fast inventory moves
- **FBA inventory**: Amazon-specific inventory management (inbound shipment, storage fees)

**Common requirements to check:**
- How is multi-warehouse inventory visualized?
- How are safety stock levels calculated?
- How are inventory discrepancies reconciled?
- How is FBA inventory synced?

#### Procurement Management (采购管理)
**Key concepts:**
- **Procurement planning**: Calculating reorder points based on sales velocity
- **Supplier management**: Managing relationships, lead times, quality ratings
- **Purchase orders (PO)**: Creating, approving, tracking POs
- **Landed cost**: Total cost including product cost, shipping, tariffs, handling

**Common requirements to check:**
- How are reorder points calculated? (considering lead time, safety stock)
- How are supplier lead times factored into procurement plans?
- How is landed cost calculated and tracked?

#### Financial Management (财务管理)
**Key concepts:**
- **Multi-currency**: Handling transactions in USD, EUR, GBP, CNY, etc.
- **Exchange rates**: Real-time vs. daily rates, rate sources (OANDA, central bank)
- **Cost accounting**: FIFO, LIFO, weighted average methods
- **Reconciliation**: Matching platform settlements with orders
- **Profit calculation**: Revenue - (product cost + shipping + platform fees + tariffs)

**Common requirements to check:**
- Which currencies are supported?
- How are exchange rate fluctuations handled?
- How is COGS (Cost of Goods Sold) calculated?
- How are platform fees (referral fees, FBA fees) accounted for?

---

## 2. WMS (Warehouse Management System) Domain

### 2.1 Core Modules

#### Inbound Management (入库管理)
**Key concepts:**
- **ASN (Advanced Shipping Notice)**: Notification of incoming goods
- **Quality inspection (QC)**: Checking received goods for damage, quantity, specs
- **Putaway**: Moving goods from receiving to storage locations
- **Labeling**: Applying SKU labels, FBA labels, shipping marks

**Common requirements to check:**
- How are damaged goods handled during QC?
- How are putaway locations determined? (random vs. fixed locations)
- How are FBA shipment labels generated?

#### Outbound Management (出库管理)
**Key concepts:**
- **Order wave**: Grouping orders for efficient picking
- **Picking**: Retrieving items from storage (single-order vs. batch picking)
- **Packing**: Choosing appropriate packaging, calculating dimensional weight
- **Shipping mark**: Labels required for international shipping (FBA labels, carrier labels)
- **Manifest**: Shipping document listing all packages in a shipment

**Common requirements to check:**
- How are pick paths optimized?
- How is packaging determined? (dimensional weight pricing)
- How are shipping labels generated for different carriers?
- How are fragile/oversized items handled?

#### Inventory Control (库存控制)
**Key concepts:**
- **Cycle counting**: Regular partial inventory counts to maintain accuracy
- **Stock adjustment**: Correcting discrepancies (damage, loss, found)
- **Stock transfer**: Moving inventory between warehouses
- **FIFO/FEFO**: First-In-First-Out or First-Expired-First-Out for perishables

**Common requirements to check:**
- How are cycle count schedules determined?
- How are stock adjustments audited?
- How is FIFO enforced for expiration-date-sensitive goods?

#### Returns Management (退换货管理)
**Key concepts:**
- **RMA (Return Merchandise Authorization)**: Authorizing returns
- **Inspection**: Assessing condition of returned goods
- **Restocking**: Returning sellable items to inventory
- **Disposition**: Discarding, returning to supplier, or liquidating unsellable items

**Common requirements to check:**
- How are return reasons categorized?
- How are restocking decisions made?
- How are return shipping costs handled?

---

## 3. SCM (Supply Chain Management) Domain

### 3.1 Core Modules

#### Supplier Management (供应商管理)
**Key concepts:**
- **Supplier onboarding**: Vetting, contracting, setting up in system
- **Performance rating**: Quality, on-time delivery, responsiveness
- **Supplier portal**: Allowing suppliers to view POs, update lead times
- **Multi-sourcing**: Using multiple suppliers for same product to mitigate risk

**Common requirements to check:**
- How are supplier performance metrics calculated?
- How are supplier portals secured?
- How are supplier lead time changes propagated to procurement plans?

#### Procurement Planning (采购计划)
**Key concepts:**
- **Demand forecasting**: Predicting future sales based on historical data
- **Reorder point**: Inventory level that triggers replenishment
- **EOQ (Economic Order Quantity)**: Optimal order quantity to minimize total costs
- **Lead time**: Time from placing PO to receiving goods

**Common requirements to check:**
- What forecasting algorithm is used? (moving average, exponential smoothing)
- How are seasonality and trends accounted for?
- How are lead time variability and supply chain disruptions handled?

#### Logistics Management (物流管理)
**Key concepts:**
- **Shipping methods**: Ocean freight, air freight, express courier (DHL, FedEx, UPS)
- **Freight forwarding**: Coordinating international shipping
- **Customs clearance**: Documentation, duties, taxes
- **Tracking**: Real-time visibility into shipment status

**Common requirements to check:**
- How are shipping methods compared? (cost vs. speed)
- How are customs documents generated?
- How are duties and taxes estimated?

---

## 4. Cross-Cutting Concerns

### 4.1 Multi-Platform Integration
**Key platforms:**
- **Amazon**: SP-API (Selling Partner API), reports API, notifications
- **eBay**: Trading API, Inventory API
- **Shopify**: Admin API, webhooks
- **Walmart**: Marketplace API
- **天猫国际/京东全球购**: Open platform APIs

**Common integration patterns:**
- **Polling**: Periodically fetching orders (every 15 minutes)
- **Webhooks**: Receiving real-time notifications (preferred)
- **Rate limiting**: Respecting API call limits (e.g., Amazon SP-API: 0.0167 requests/sec for some operations)
- **Error handling**: Retry logic, dead-letter queues

**Requirements to check:**
- How are API rate limits handled?
- How are webhook failures handled?
- How is API versioning managed?

### 4.2 Multi-Currency & Exchange Rates
**Key considerations:**
- **Exchange rate sources**: OANDA, XE, central banks, platform-provided rates
- **Rate update frequency**: Real-time vs. daily vs. hourly
- **Rate types**: Spot rate, average rate, historical rate
- **Currency conversion**: Which currency is the base currency? How are reports in different currencies handled?

**Requirements to check:**
- Which exchange rate source is used?
- How often are rates updated?
- How are historical transactions handled when rates change?

### 4.3 Cross-Border Compliance
**Key regulations:**
- **GDPR (EU)**: Data privacy for EU customers
- **CCPA (California)**: Data privacy for California residents
- **Product compliance**: CE marking (EU), FCC (US), RoHS (hazardous substances)
- **Customs regulations**: HS codes (Harmonized System), certificates of origin
- **Tax compliance**: VAT (Value Added Tax), GST (Goods and Services Tax)

**Requirements to check:**
- How is customer data protected?
- How are HS codes managed for products?
- How are taxes calculated for different destinations?

### 4.4 Data Consistency & Integrity
**Key challenges:**
- **Eventual consistency**: Cross-system updates may have delays
- **Idempotency**: Preventing duplicate processing (e.g., duplicate orders from webhook retries)
- **Data reconciliation**: Matching data across systems (ERP vs. WMS vs. platform)

**Requirements to check:**
- How are duplicate orders prevented?
- How are inventory discrepancies detected and resolved?
- How are financial reconciliations automated?

---

## 5. Common Business Scenarios

### 5.1 Typical Order Fulfillment Flow
1. Customer places order on Amazon
2. Order syncs to ERP via SP-API
3. ERP checks inventory availability
4. If in stock: ERP sends order to WMS for picking
5. WMS picks, packs, generates shipping label
6. Carrier picks up package, tracking number syncs back to WMS → ERP → Amazon
7. Customer receives order, feedback syncs back to ERP

**PRD review questions:**
- How are orders routed to the optimal warehouse?
- How are partial shipments handled?
- How are shipping delays communicated to the customer?

### 5.2 Procurement Flow
1. ERP detects low inventory (reorder point reached)
2. System generates procurement suggestion
3. User reviews and creates PO
4. PO sent to supplier (email, EDI, or supplier portal)
5. Supplier confirms, provides estimated delivery date
6. Goods arrive at warehouse, WMS receives and QCs
7. Inventory updated in WMS → ERP
8. Supplier invoice received, ERP matches with PO and receipt (3-way matching)
9. Payment scheduled

**PRD review questions:**
- How are reorder points dynamically adjusted based on sales trends?
- How are supplier delays communicated?
- How are quality issues with suppliers handled?

### 5.3 Returns Flow
1. Customer requests return via Amazon
2. Return authorization syncs to ERP
3. ERP instructs WMS to expect return
4. Customer ships return, tracking number provided
5. WMS receives return, inspects condition
6. If sellable: restock to inventory
7. If unsellable: dispose or return to supplier
8. Refund processed via Amazon

**PRD review questions:**
- How are return reasons categorized for analysis?
- How are refund amounts calculated (including shipping, restocking fees)?
- How are return shipping costs handled?

---

## 6. Key Metrics & KPIs

### 6.1 ERP Metrics
- **Order processing time**: From order placement to shipment
- **Inventory accuracy**: (Physical count - System count) / Physical count
- **Perfect order rate**: Orders with no errors (on time, complete, undamaged)
- **Gross margin**: (Revenue - COGS) / Revenue

### 6.2 WMS Metrics
- **Picking accuracy**: Correct items picked / Total items picked
- **Packing efficiency**: Orders packed per hour
- **Space utilization**: Used storage space / Total storage space
- **Cycle count accuracy**: Discrepancies found during cycle counts

### 6.3 SCM Metrics
- **Perfect order fulfillment**: Complete and on-time orders / Total orders
- **Inventory turnover**: COGS / Average inventory
- **Days of inventory on hand**: Average inventory / (COGS / 365)
- **Supplier on-time delivery**: On-time deliveries / Total deliveries

**PRD review questions:**
- Are these metrics defined in the PRD?
- How will the system calculate and display these metrics?
- What are the target values for each metric?

---

## 7. Common Pitfalls in PRDs

### 7.1 Vague Requirements
❌ **Bad**: "Support multi-currency"  
✅ **Good**: "Support USD, EUR, GBP, and CNY. Exchange rates fetched daily from OANDA at 00:00 UTC. Support viewing reports in any supported currency."

### 7.2 Missing Edge Cases
❌ **Bad**: "Sync orders from Amazon" (no error handling specified)  
✅ **Good**: "Sync orders from Amazon every 15 minutes via SP-API. If API is unavailable, retry 3 times with exponential backoff. Log failures and alert operations team."

### 7.3 Unrealistic Expectations
❌ **Bad**: "Real-time inventory sync across all platforms" (impossible due to API rate limits)  
✅ **Good**: "Sync inventory to platforms every 15 minutes. For Amazon, use SP-API feeds API for bulk updates to respect rate limits."

### 7.4 Ignoring Cross-Border Complexities
❌ **Bad**: "Calculate shipping cost" (ignores dimensional weight, customs duties)  
✅ **Good**: "Calculate shipping cost based on weight, dimensions, destination, and shipping method. For international shipments, estimate duties and taxes using built-in customs duty tables."

---

## 8. Domain-Specific Review Angles

> 注意：以下为领域知识维度的补充视角，**不替代** v2.3 检查清单（见 checklist.md）。
> 审核时仅在此内容与当前 PRD 业务领域相关时参考。

**For ERP features:**
- [ ] Does it handle multi-platform order aggregation correctly?
- [ ] Does it account for multi-currency and exchange rate fluctuations?
- [ ] Does it integrate with accounting for cost tracking?
- [ ] Does it handle cross-border tax compliance?

**For WMS features:**
- [ ] Does it support multi-warehouse operations?
- [ ] Does it handle FBA-specific workflows (inbound shipment plans)?
- [ ] Does it optimize picking paths for efficiency?
- [ ] Does it handle international shipping documentation?

**For SCM features:**
- [ ] Does it factor in lead time variability?
- [ ] Does it handle supplier performance tracking?
- [ ] Does it optimize shipping methods (cost vs. speed)?
- [ ] Does it handle customs and compliance documentation?

---

## 9. References

**Industry standards:**
- GS1 standards for barcode/labeling
- INCOTERMS for international shipping terms
- HS (Harmonized System) codes for product classification

**Platform documentation:**
- Amazon SP-API: https://developer-docs.amazon.com/sp-api/
- Shopify Admin API: https://shopify.dev/docs/admin-api
- eBay Trading API: https://developer.ebay.com/devzone/trading/trading-call-ref/

**Recommended reading:**
- "Cross-Border E-Commerce Operations" (跨境电商运营实战)
- "Warehouse Management Best Practices" (仓储管理最佳实践)
- "Supply Chain Management: Strategy, Planning, and Operation"
