# Final Practicum Project: Multi-pronged Approach to Detect Cheaters in Storj DCS (Decentralized Cloud Storage System)
Final Practicum of Georgia Tech M.Sc. in Analytics, in collaboration with Storj DCS

# Description

This is the Storj practicum report package from Priscilla Mariani and Lisa Marshall.
Final report has been submitted in August 2021 to Storj DCS for further investigation.
No NDA was signed, however data are kept hashed and detailed report is not uploaded for the company's benefit.

# Data
Data were drawn from a combination of fetched blockchain transactions (using Etherscan and Zksync APIs) and supplied hashed data from the company.

# Methodology

Our proposed approach includes a 2-pronged approach on each side:
1. For the user side:
  a. Clustering on the user data to detect anomalous accounts
  b. User bandwidth usage pattern analysis to detect potential cheaters
2. For the node operator side:
  a. Clustering on the wallet data to detect anomalous accounts
  b. Wallet transaction analysis to detect anomalies in operator handling of Storj tokens

# Submission Files

Our supplemental files are included in 2 folders:

1. Scripts 
This is a set of jupyter notebooks, written in Python 3, that we used to collect and manipulate the data and perform our analysis.
It contains the following notebooks and subfolders:

map_project_to_node.ipynb -- This notebook takes in the project-level output file of user_usage_pattern_analysis.ipynb and performs a number of merges
to connect the user accounts we studied in that file with operator accounts sharing a common email address. It then identifies cases where both the user
and the operator behavior scored >.8.

user_exceed_limit.ipynb -- This notebook takes the user-level data file output from the GetUserLevelMasterSet-freeOnly.ipynb notebook and explores instances
where free-tier users exceed their project bandwidth and/or storage limits.

user_usage_pattern_analysis.ipynb -- This notebook analyzes the difference between cheating suspects and non-suspects based on timely download and upload 
cancellations, and then performs a download bandwidth usage analysis using Dynamic Time Warping (DTW). The result is a user-level probability combining 
probabilities from download cancellation counts, upload cancellation counts, and download bandwidth usage pattern. It is exported to user_combinedProb.csv.

wallet_clustering_analysis.ipynb -- This notebook performs the clustering analysis. It takes as input the operator-level data output but mergeTXdata3.ipynb,
creates a few final features, performs the clustering and scoring, and outputs 3 files: one containing the clustering results (probabilities) for all
wallets, called LSCPoutputFinal.csv; one containing the results and full dataset for the 65 highlighted candidates in pickle format, called candidateDF2.pkl;
and a version of that previous file saved to csv, called clustering_candidate65.csv.

wallet_midas_analysis.ipynb -- This notebook pre-processes fetched Etherscan Storj token transactions data for MIDAS input and analyze the MIDAS output. 
The result is a wallet-level anomalousness probability.

wallet_results_combination.ipynb -- This notebook combines the cheating probabilities from clustering and MIDAS approaches using a weighting algorithm. 
The results are wallet-level and node-level cheating probabilities. The wallet-level result includes probabilities from the individual approaches. The two 
sets of results are exported to wallet_combinedProb.csv and node_combinedProb.csv.

etherscan_fetch_wallet_txns.ipynb -- This notebook uses Etherscan API to fetch the token transactions data for specified list of wallets, starting from the
wallets of Storj node operators. The results are used in wallet clustering and MIDAS approaches.

Subfolder 'Clustering pre-processing' contains all of the notebooks that create the master datasets for user-level data and operator-level data. It contains:
*  operatorData.ipynb: Takes in raw csv data sets from across three regions, performs manipulation and aggregation on the node and payments data; 
fetches wallet IDs for zksync wallets using zkscan API; merges the node and payments data by wallet ID. Produces the operatorDatax.pkl and missingWallets.csv files.
*  LimitTokenTXtoUsedWallets2.ipynb: Takes the tokenTx_ori.csv file and reduces it to include only the wallets contained in the operatorDatax.pkl file. 
Produces the tokenTXS2.pkl file.
* mergeTXdata3.ipynb: Takes the tokenTXS2.pkl, dfNormal.csv, operatorDatax.pkl, and missingWallets.csv files and merges the transaction data with the node and 
payments data to provide a full overview for each wallet. Produces the operatorDataTx.pkl file.
* GetUserLevelMasterSet-freeOnly.ipynb: Takes in all of the original csv files provided by Storj and outputs a master set of the free-tier user data in pickle 
format, called userDataFree.pkl.

Subfolder 'Etherscan fetching' contains notebooks used to pull transaction data related to node operators from Etherscan:
* etherscan_fetch_wallet_txns.ipynb -- collects token transactions for all node operator wallets listed in the Storj payment files and outputs a file called
tokenTx_ori.csv.
* etherscanTransactionsByWallet.ipynb -- collects normal transactions for the node operator wallets that have received some payment from Storj in the last 6
months (the group of wallets used for clustering analysis). It outputs the dfNormal.csv file.

2. Results
This is a set of csv files containing results from our various approaches. They include:

clustering_candidate65.csv -- containing data on the 65 node operator candidates identified by the clustering approach.

node_combinedProb.csv -- containing each node ID's probability of cheating (obtained by taking the max cheating probability from among its associated wallets).

user_combinedProb.csv -- containing cheating probabilities for each user.

wallet_combinedProb.csv -- containing the probabilities for each node operator wallet resulting from the weighted combination of the clustering and MIDAS approaches.

# Team Members
- Priscilla Mariani
- Lisa Marshall
