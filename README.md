# Sneaker Resale Sourcing Analysis

A data-driven look at which sneakers are actually worth buying to resell, built on 99,956 real StockX transactions.

Why I built this

[EDIT THIS PARAGRAPH WITH YOUR OWN STORY. Suggested outline: I ran a sneaker reselling business as the owner and operator, handling sourcing, pricing, and selling myself. (Add: roughly when, how many pairs, what platforms you sold on, and one real decision you made on instinct, like which pairs to buy or when to sell.) This project tests those instincts against real market data and treats resale as a supply chain problem: what to source, how much margin it carries, how easy it is to move, and how long to hold it. I'm studying Supply Chain Management at Michigan State University's Broad College of Business and wanted a project that applies that thinking to something I know firsthand.]

Questions
Which models deliver the best return after fees, and how reliable is that return?
Do resale premiums fade after release, and does the answer depend on the shoe?
Do shoe size or buyer region meaningfully change the premium?
Can the answers be combined into a simple restock priority ranking?
Data

The StockX 2019 Data Contest dataset (Kaggle): 99,956 sales of Adidas Yeezy 350 (72,162 sales) and Nike x Off-White (27,794 sales) sneakers, 2017 to 2019. Each row has order date, release date, sale price, retail price, shoe size, and buyer state.

Limits of the data: only two brands, only 2017 to 2019, and a sample of StockX sales. Treat the findings as directional, not as a description of today's market.

Method
Cleaned prices and dates, stripped a whitespace bug in the Brand column, and merged model names that differed only by capitalization.
Created Price Premium (sale price / retail price), Profit, and Days Since Release.
Compared models using medians, not means, because a few extreme sales (up to 21x retail) skew averages.
Built a restock priority score from percentile ranks of net profit (50%), sales volume as a liquidity proxy (25%), and loss rate (25%). The weights are a judgment call, not a fitted model.
Ran a sensitivity test on the fee assumption (9%, 12%, 15%).

Assumptions (not from the data): 12% seller fees and $5 shipping per pair, and that every pair is bought at retail. Change FEE_RATE and SHIPPING in the notebook to match your own costs.

Key findings
1. High margin and high volume rarely overlap

Off-White Jordan 1s carry the biggest margins (median net profit of roughly $1,300 for the Chicago and White colorways) but only 300 to 755 sales each. The mass-release Yeezy 350 V2 colorways trade 5,000 to 11,000 times each, but their median premium is only about 1.1x to 1.3x. That is the core sourcing tradeoff: scarce products pay well and are hard to get, while easy-to-get products barely pay.

The Off-White Jordan 1 University Blue is the exception. It has about 4,700 sales and a median net profit near $520, with no unprofitable sales in the data.

2. Thin-margin shoes are sensitive to fees
Model	Median net, 9% fee	12% fee	15% fee	Loss rate, 9% / 12% / 15%
Jordan 1 University Blue	$546	$521	$497	0% / 0% / 0%
Yeezy 350 V2 Static	$27	$19	$10	1% / 3% / 12%
Yeezy 350 V2 Butter	$19	$11	$3	16% / 26% / 43%
Yeezy 350 V2 Sesame	$12	$4	-$4	14% / 35% / 58%
Yeezy 350 V2 Cream White	$3	-$5	-$13	46% / 55% / 60%

Fees barely change the picture for high-margin shoes. For the mass-release V2s, whether a pair is worth buying depends on the fee structure, and Cream White is break-even at best.

3. Time-based price decay needs to be measured within each model

A simple chart of premium by time since release shows Off-White premiums jumping to about 6.8x for sales a year or more after release. That is a mix effect, not appreciation. None of the Off-White models in the high-volume table has sales in that bucket, so the older, higher-premium shoes dominate it. Comparing each model to itself (median premium after 90 days vs. within the first 30 days) gives a different picture:

Hyped Yeezy colorways fade hard. Semi-Frozen Yellow retains only about 0.38x of its early premium.
Most Off-White models stay roughly flat (about 0.85x to 1.15x).
A few Off-White models (Air Force 1 Low, Zoom Fly, Air Max 97, React Hyperdunk) show 1.5x or more, which I did not verify further and do not claim as appreciation.

Only 20 models had 50 or more sales in both windows, so this is a subset of the catalog.

4. Model choice matters far more than size or region
Regional median premiums across the top 10 states ranged only from about 1.63x to 1.85x. California and New York dominate volume.
For Off-White, whole sizes had higher median premiums than the neighboring half sizes. For Yeezy, sizes 8.5 to 10 had the highest premiums (about 1.55x to 1.59x). These are observations from medians without significance testing, and size could be confounded with model mix.
Restock priority ranking

Top of the list: Off-White Jordan 1 (Chicago, University Blue, White), Off-White Air Presto, and the older Yeezy 350 V2 Core Black Red and 350 Low Pirate Black. Bottom of the list: Yeezy 350 V2 Cream White, Sesame, Static, Butter, and the Off-White Zoom Fly variants.

Because the score uses percentile ranks, it compresses large dollar differences, so read it alongside the profit columns.

Limitations
Assumes every pair is bought at exactly retail. For scarce shoes like the Off-White Jordans, real acquisition cost (raffles, bots, lines, secondary-market buys) was likely much higher, so their true profit is lower than shown.
Fees and shipping are assumed, not observed.
The data ends in 2019 and covers two brands, so the findings may not hold for other brands or today's market.
Sale price is the price the buyer paid. It does not capture time to sell, unsold inventory, or returns.
Priority score weights are subjective.
What I'd do next
Add acquisition cost assumptions for scarce models and recompute the ranking.
Model days-to-sell and holding costs to account for capital tied up in inventory.
Test whether size premiums hold within a single model.
Build an interactive Streamlit dashboard with a fee slider.
Tools

Python, pandas, matplotlib, Google Colab.

Files
stockx_analysis.ipynb: full analysis notebook
README.md: this file
