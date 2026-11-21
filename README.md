# China direct route VPS: what CN2 GIA, AS9929 and CMIN2 actually buy you, which plan is enough, and what a year really costs

AkkoCloud's CN2 GIA line starts at **CN¥50/month in San Jose** (about $7) and **CN¥299/year in Frankfurt** (about $42), the cheapest entry into premium China routing I could verify while putting this together. DMIT charges $29.90/month for its Los Angeles premium tier; BandwagonHost charges $169.99/year, roughly $14/month, for triple-carrier CN2 GIA in Los Angeles. That spread needs an explanation before it needs a recommendation.

The differences that decide the purchase are these: how the five China-facing networks differ and why a $5 "163 direct" plan is a different product from CN2 GIA; what peak-hour latency looks like on each; how much annual billing actually saves against monthly; and which after-sales terms (refund window, IP replacement, SLA credits) decide whether a cheap plan stays cheap.

Two things about AkkoCloud before the table. Its locations are San Jose, Frankfurt and London. There is no Hong Kong or Tokyo node, so if you need single-digit latency from southern China, this is the wrong shop. Support runs on Chinese-language tickets and a Telegram group, checkout takes Alipay, and prices are quoted in CNY.

> **Quick pick:** Most people searching "china direct route vps" need one stable return path into the mainland for a site or a small team's proxy node. AkkoCloud's Frankfurt FRA.CN2.A1 does that for **CN¥299/year** with 0.9GB RAM, 500GB monthly traffic and a 1Gbps port; third-party measurement puts its return-route latency between 126ms and 184ms across seven provinces. 👉 [Check FRA.CN2.A1 price and current stock](https://bit.ly/akkocloud)

## AkkoCloud plans: what is actually on the order page

| Plan | CPU · RAM · Disk | Traffic · Port | Price | Order |
| --- | --- | --- | --- | --- |
| San Jose CN2 GIA | 1 core · 512MB · 10GB SSD | 500GB/mo · 300Mbps | CN¥50/month | [San Jose plan](https://bit.ly/akkocloud) |
| Frankfurt CN2 GIA (FRA.CN2.A1) | 1× AMD EPYC · 0.9GB · 9.7GB | 500GB/mo · 1Gbps | CN¥299/year | [Frankfurt plan](https://bit.ly/akkocloud) |
| London CN2 GIA (annual special) | 1 core · 768MB · 15GB SSD | traffic not listed · 300Mbps | CN¥449/year | [London annual](https://bit.ly/akkocloud) |
| London dedicated server | dual EPYC 7552 · 512GB RAM · 10×1TB SSD | not listed | CN¥33,000/month | [London dedicated](https://bit.ly/akkocloud) |

Frankfurt is sold as several configurations on the order page; FRA.CN2.A1 is the one with published specs and price. Prices and stock move, so confirm at checkout.

The San Jose box is the smallest and the only monthly-billed VPS in the list. 512MB runs a proxy stack or a static site; it will not run Docker plus a database plus a build pipeline. Frankfurt gives you double the RAM, a faster port and a third of the per-month cost, in exchange for an annual commitment. London's annual special sits between them on price with 768MB and 15GB of disk. The London dedicated server is a different conversation: 512GB of RAM and ten 1TB SSDs for CN¥33,000/month, aimed at hosting providers and heavy multi-tenant workloads rather than anyone reading a VPS buying guide.

One structural note: all three VPS regions sit outside mainland China, so none of them require an ICP license. Content hosted there is not subject to the filing regime that applies to servers inside the mainland.

## Five networks, three carriers, and why the cheap plan is not the same product

China Telecom's public backbone, **AS4134** (ChinaNet, the "163" network), is where most international VPS traffic lands by default. One route guide reports its packet loss reaching **30%+ between 8pm and 11pm China time**, which is exactly when streaming, remote work and video calls happen. A US host selling San Jose VPS advertises direct peering to that same 163 backbone as its China-optimized feature and says it reduces latency and packet loss. Both claims come from parties with something to sell. What isn't in dispute is the price: a 1GB/20GB/1TB plan on a 163-peered network costs **$5/month**, and premium-routed plans start several times higher. They are different products, and the right one depends on which carrier your users are on.

**AS4809** splits into two tiers. CN2 GT (Global Transit) was the mid-tier option, and by around 2019 its peak-hour congestion had drifted close to ChinaNet levels, which is why it stopped being a selling point. CN2 GIA (Global Internet Access) is the tier that still holds: capacity is limited, transit costs providers as much as **$120 per megabit**, and the return is **120–140ms while ordinary routes spike past 300ms**.

**AS23764 CTGNet** is China Telecom's newer product. One route guide puts its performance and pricing roughly level with CN2 GIA, and a Hong Kong host describes it as an upgraded version of CN2 GIA. Fewer providers carry it, so it shows up as a premium line item more often than a budget one.

On the other two carriers: Unicom's premium path appears as **AS9929** in one source and as **AS10099** in another host's documentation of its Los Angeles facility. Those are different labels and I am not merging them into one number. China Mobile international is **AS58807**, sold as **CMIN2**.

That is what "triple-carrier optimization" means: telecom CN2 GIA, the Unicom premium path and CMIN2 all routed properly. Many providers optimize one carrier. The telecom user gets a clean path; the colleague on Mobile gets whatever generic transit delivers, which in practice is the 250ms+ evening.

So: **a $5 direct-163 plan and a CN2 GIA plan are not the same tier**, even when both pages say "China optimized". The 163 plan is bulk bandwidth at a budget price with a backbone that saturates in the evening. CN2 GIA is limited-capacity transit with a price to match. If your users are on mixed carriers and the application is latency-sensitive, neither a single-carrier optimization nor a 163 peering deal answers the question.

## Peak hour is where the money goes

Numbers below come from third-party roundups and forum reports, not from any vendor's official test.

| Setup | Reported mainland latency |
| --- | --- |
| DMIT LAX Pro (CN2 GIA + AS9929 routing) | 140–180ms through peak hours |
| BandwagonHost USCA_9 CN2 GIA | ~158ms average, no peak-hour loss reported |
| Los Angeles CN2 GIA to Shanghai, general | 160–200ms |
| Hong Kong CN2 GIA | single-digit to low-double-digit ms |
| Ordinary Tier-1 US VPS | 250ms+ with visible packet loss |
| AS4134 / 163 during 8–11pm | 30%+ packet loss reported |

The pattern matters more than any single figure. A properly routed premium line lands in the 140–200ms band from North America and stays there in the evening. A generic Tier-1 box is fine at 3pm and unusable at 9pm, with the CPU idle the whole time. Users describe this as "the server is slow", which sends people shopping for more RAM they don't need.

### AkkoCloud's measured return routes

A third-party measurement published in October 2026 recorded these median return-route latencies for FRA.CN2.A1:

| Province | China Telecom | China Unicom | China Mobile |
| --- | --- | --- | --- |
| Beijing | 159ms | 131ms | 126ms |
| Shanghai | 158ms | 151ms | 148ms |
| Zhejiang | 164ms | 158ms | 150ms |
| Jiangsu | 164ms | 156ms | 150ms |
| Guangdong | 180ms | 166ms | 175ms |
| Hubei | 171ms | 158ms | 145 |
| Sichuan | 184ms | 171ms | 159ms |

A European node landing inside the same band as Los Angeles CN2 GIA is the interesting part. Frankfurt is not inherently worse to China than California once the return path is engineered, and the spread across provinces (126ms to 184ms) is wider than the spread across carriers. The carrier labels are reproduced as the measurement reports them; the Unicom column is tagged CN2 GIA there, which is unusual for a Unicom path, so ask the host directly if Unicom users are your priority.

## Monthly versus annual, and what leaving costs

San Jose at CN¥50/month runs **CN¥600 if you pay monthly for a year**. Frankfurt at CN¥299/year costs less than six months of that, with more RAM and a faster port. London at CN¥449/year works out around CN¥37/month. The annual specials are AkkoCloud's discount mechanism; there is no separate coupon to hunt for, and the plan pages are the place to check what is currently in stock.

Cheap annual plans are only cheap if you can leave. Compare the exit terms across the market:

- **DMIT:** full refund within 3 days and under 30GB of transfer used, partial refund within 30 days, nothing after. Uptime SLA 99%, with half a month credited below 99%, a full month below 95%, two months below 90%. Ticket SLA 72 hours.
- **BandwagonHost:** 30-day refund. Non-urgent tickets are slow.
- **AkkoCloud:** the pages summarized here publish a support commitment (tickets answered within 24h, resolved within 48h) rather than a refund window or an uptime credit schedule I could verify. Before paying for a year, ask support in writing what the refund terms are. Support is in Chinese, which is fine if someone on your team reads it and a real obstacle if nobody does.

The term that decides whether a China-facing VPS is usable long-term is **IP replacement**. Addresses get blocked; when that happens, a plan with no swap policy is a plan you replace entirely.

- **DMIT:** free IP replacement every 15 days on Pro and Eyeball profiles, or every 7 days with the IP Care+ add-on, with first-connection reachability guaranteed across countries.
- **BandwagonHost:** no routine IP swap policy in the documentation reviewed here.
- **AkkoCloud:** not documented on the plan pages reviewed here, so ask.

Data-center migration is the other flexibility lever. BandwagonHost lets you move a server between data centers after purchase at no extra charge, consuming traffic equal to your disk usage. DMIT's Pro, Eyeball and Tier-1 profiles are separate product lines and don't migrate across tiers. AkkoCloud spans three continents, which makes a documented migration policy valuable and its absence worth clarifying.

👉 [Open the AkkoCloud order page to check current plan pricing and stock](https://bit.ly/akkocloud)

## How it stacks up against the usual names

Pure spec-and-price comparison, no endorsements implied:

| Provider | Route focus | Verified entry price | Terms worth knowing |
| --- | --- | --- | --- |
| AkkoCloud | CN2 GIA, San Jose / Frankfurt / London | CN¥50/mo; CN¥299/yr | CNY billing, Alipay, Chinese-language tickets |
| DMIT | CN2 GIA + AS9929 + CMIN2, LA / HK / Tokyo | $29.90/mo (LAX Pro) | 3-day full refund under 30GB, 15-day IP swap, 99% SLA |
| BandwagonHost | CN2 GIA + CMIN2 + Unicom premium (AS10099) | $169.99/yr CN2 GIA LA; $49.99/yr Basic | 30-day refund, free one-click DC migration |
| SoftShellWeb | Direct 163 (AS4134), San Jose | $5/mo (1GB) | Shared 1Gbps port, 4 vCPU/8GB at $40/mo |
| TemasekTech | CTGNet, Hong Kong | $25.95/mo | Plans list 2–5Mbps dedicated alongside a 1Gbps port; clarify before buying |

AkkoCloud's position in that table is price. It is the only entry under CN¥300/year with CN2 GIA routing on all three carriers in the measurement, and the trade is a smaller spec sheet, no Asian location and support that runs in Chinese. DMIT and BandwagonHost charge 4–8× more per month and document their refund and IP policies in detail, which is what the extra money buys alongside the bigger boxes. SoftShellWeb is cheaper per gigabyte of RAM but sells 163 peering, a different tier. TemasekTech sells Hong Kong proximity with bandwidth caps that need reading carefully.

## Who should not buy China-optimized routing

Skip the premium route entirely if your site's audience isn't in mainland China. A blog read from the US and Europe gets nothing from CN2 GIA, and a $5 box elsewhere does the job.

Skip AkkoCloud if your users are concentrated in Guangdong or Hong Kong. Hong Kong nodes deliver single-digit to low-double-digit milliseconds, and no amount of return-path engineering in Frankfurt closes that gap. If you need that, pay for Hong Kong.

Skip the 512MB plan if you're running anything beyond a proxy or a static page. A 4 vCPU / 8GB / 240GB plan with 16TB of traffic costs $40/month elsewhere, and buying three of AkkoCloud's small boxes to approximate it is not a saving.

And skip any annual China-route plan if you haven't confirmed the IP replacement policy first.

## The risk that isn't on any sales page

A June 2026 thread on r/VPS from a developer planning a business trip captures the real variance. In the same discussion:

- One person ran **GreenCloud Tokyo + Hysteria2 + Shadowrocket** on a China Unicom connection in Guangdong and reported stable routing throughout.
- Another had Hysteria2 packets dropped after the setup had been working.
- A third ran **Vultr Japan + Xray Reality**; testing TCP reachability on itdog.cn, every region timed out except one.
- One commenter argued that any proxy hosted on a VPS gets detected and blocked quickly, and recommended an international roaming eSIM with hotspot instead.

The useful conclusion for a purchase decision is not "VPS proxies work" or "they don't". It's that results depend on the carrier, the province, the time of day and the current state of the IP you were assigned. Standard WireGuard, OpenVPN and basic IPSec get fingerprinted quickly by DPI; the current default stack is Xray with VLESS + XTLS Reality or Hysteria2/TUIC, managed through a panel like 3X-UI. None of that is a guarantee.

What that means commercially: **budget for more than one node**, and prefer a provider that will swap your IP without a fight. The comment repeated most often in that thread was that what works on Monday may not work on Tuesday. A CN¥299/year plan is a reasonable amount to lose if a node goes bad; a $169.99/year plan plus a second provider is a more realistic setup for a team that needs GitHub and Slack every working day.

## FAQ

**Do I need an ICP license?** No. The servers are physically in the US, Germany and the UK, outside mainland jurisdiction, so the filing requirement doesn't apply.

**Hong Kong or the US/Europe?** Hong Kong wins on physics, with single-digit to low-double-digit milliseconds. Los Angeles CN2 GIA typically lands at 160–200ms to Shanghai, and the Frankfurt measurement above sat at 126–184ms. For websites, APIs and most applications, 150ms is fine. For real-time gaming or checkout flows, it isn't.

**Is 512MB enough?** For a proxy node or a static site, yes. For a database, a build pipeline or anything containerized with a JVM in it, no. Take the Frankfurt or London plan.

**What happens if the IP gets blocked?** Ask before you pay. DMIT documents 15-day swaps, or 7 with an add-on; BandwagonHost and AkkoCloud don't publish a routine swap policy on the pages reviewed here. This is the single most important pre-purchase question for proxy use.

**Is the $5 direct-163 plan the same thing?** No. It peers with AS4134, a public backbone one source reports dropping 30%+ of packets between 8pm and 11pm, and it is priced accordingly.

## Before you order

Match the plan to the job:

- One site or a small team's proxy node, budget first → **Frankfurt FRA.CN2.A1 at CN¥299/year**
- US West location and monthly billing flexibility → **San Jose at CN¥50/month**
- Slightly more RAM, annual billing, European location → **London annual special at CN¥449/year**
- Real workloads with a database → none of the above; go to a larger plan tier
- Lowest latency from southern China → a Hong Kong node, not this provider

Two things to confirm on the live site before paying: the current price and stock (annual specials change), and the refund and IP replacement terms in writing from support. Then place the order.

👉 [Compare AkkoCloud's current plans and start the order](https://bit.ly/akkocloud)
