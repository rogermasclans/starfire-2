# Starfire
Code and logs for the Starfire project.


## Logs

## Sep 9:
- Update code `2.1_retrieve_compu_stock_prc_for_pb_deals` to create main set with firm stock and stock index prices. 
    - Create final dataset for analysis, uploaded to big query under table `pb_deals_compu_stock_prices_v1`
    - Added index reactions. For 522 (0.4%), index reaction was missing. Used S&P for these cases. Now only 7 are missing. 
    - Created lags at the loop_counter-iid level. That is, for each stock retrieved and stock issued type.

## Sep 5:
- Update, via refinitiv, stock index data with prices before 2000. The notebook `2.3_retrieve_refi_stock_index_price` contains the details on when index data starts, query to merge files.
    - Now the dataset on BQ is named `equity_indices_all_times`    

### Sep 4:
- v2: Stock price retrieval is now working. Clean up notebooks:
    - 1.1_create_refi_deals: retrieves all deals from refinitiv
    - 2.1_retrieve_compu_stock_prc_for_pb_deals: retrieves stock prices via cik from compustat for PB deals 
    - 2.2_retrieve_compu_stock_prc_for_refi_deals: retrieves stock prices via cik from compustat for refinitiv deals, using ewens et al. linking data
    - 2.3_retrieve_refi_stock_index_price: retrieves stock index prices for all relevant, international indeces. Uses refinitiv as it works better than compustat
    - the rest of notebooks are relegated to 99.

### Sep 3:
- Retrieving stock price data for all deals via refinitiv is too slow. Let's try to match data to PB first, then start by retrieving only deals on PB. 
- Matching PB deals with Refinitiv:
    - 1. Match refi-SCD deals to gvkey via Ewens et al. 
    
### Aug 26:
- Get data for all periods. Should I filter on deal type or some other stuff to make it faster? 

### Aug 22:
- Clean up code, split into data_1, data_2, etc. Miscelania left on `refinitiv_api.ipynb`
- Added stock index prices to stock market reactions dataset
- Computed response variables to then merge to main deals data

### Aug 19:
- Create `notebooks/refinitiv_mna_deals_2.ipynb`. Gets stock market responses from refinitiv via API for firms in deals set.
- Loops too low over deals. Any other solution? Also, I got some error that could not figure out and code stopped running. 
- Wasted my time figuring out what indices to benchmark stock responses. found these data https://github.com/lukes/ISO-3166-Countries-with-Regional-Codes
- Download index price data from wrds compustat, upload to bigquery. Code: `data2.1_compu_stock_index.ipynb`

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
