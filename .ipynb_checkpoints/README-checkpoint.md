# Starfire
This repo contains the code and logs for the Starfire project.


## Logs
## Aug 19:
- Create `notebooks/refinitiv_mna_deals_2.ipynb`. Gets stock market responses from refinitiv via API for firms in deals set.
- Loops too low over deals. Any other solution? Also, I got some error that could not figure out and code stopped running. 
- Wasted my time figuring out what indices to benchmark stock responses. found this data https://github.com/lukes/ISO-3166-Countries-with-Regional-Codes

### Aug 16:
- Goal: Create data set with as many deals with both price and stock market reaction, and linked to PB data. The idea is to get all deals and then a subset of startups that are present in PB.
- 1. Get all deals from SDC/refinitiv and the stock market response for the acquirer, also from refinitiv
- 2. Use Ewens et al. to get gvkeys --> then match to compustat on gvkeys and get CIKs. then match on PB via CIKs. For both acquirer and target
- 3. There will be lots of missing data, need to think about it
- Created `notebooks/refinitiv_mna_deals.ipynb`. Gets refinitiv data on M&A on a more thorough list of variables than v1.


### Aug 15:
- Updated `notebooks/01_motivation` with industry mapping
- Created `notebooks/refinitiv_ewens_deals`. Complements ewens matching of scd_deal_id with gvkeys with other data on the mna from refinitiv.
- Create derived dataset from `refinitiv_ewens_deals`. ewens_extended.csv. Uploaded on BQ/starfire/derived. Manually changed the names of the variables (TR. not compatible with BQ naming).


  
### Aug 14:
- Updated `notebooks/pitchbook_wrds_to_bq.ipynb`. Update PB data on BigQuery.
- Created `notebooks/01_motivation`. Analyzes startup activity (founding, investment) over time.

## Thoughts:
-  Run unsupervised analysis with the stock market responses, see how the data gets partioned.
- Create science orientation measure. As per converstaion w/ Jay, there's no paper that discusses this isseue thoroughly.
- Consider using Marx's founding startup data
- Review Ramana's Net Zero paper. Section on causes why startups get lower risk-adjusted returns. Likewise, there numbers on patenting by VCs are a good motivation. Higher quality patents by all means
- Astebro's paper on SEJ is also a good motivation shows that the number of startups founded by PhDs strongly declined over time (also check some cites on that paper).
- I see that most of the startups on PB are crap. Share of startups with patents, which I plot, is very low? 6% at its peak and then goes down (probably bc of truncation).
- What is thus a good way to measure what I want to measure. What is it that I want to measure? Scienceness? Deep tech? Tech risk? Market risk?
- I need to rigorously show a decline over time of funding towards the startups I want to study. The question is why. The rest I can do with the litearture. For example, I know only a small fraction of startups patent, but I now from Nanda, Lerner, Marx, Ewens, etc. that these startups are so relevant. So I can rely on these cites. What I need to show is the trend over time. And some relationship.
- The question is should I think first on scienceness or re-run stock market response? Is there any other way to compute value capture?

## Data flow
1. Download all deals from refinitiv: 
    - 1.1 `code.py`
    - 1.2 `dataset.data`
    
2. Get stock market data for acquirors from refintiv:
    - 2.1 `code.py`
    - 2.2 `dataset.data`
    
3. Compute private value of acquisition for acquiror with different time windows:
    - 4.1 `code.py`
    - 4.2 `dataset.data`
    
4. Merge private value data to deals dataset:
    - 4.1 `code.py`
    - 4.2 `dataset.data`
    
5. Merge refinitiv data to PB
