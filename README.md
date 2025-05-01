# US_elections_analysis
TFG Matemáticas 2024/25

This is an academic project licensed under the GNU General Public License v3 with additional terms for commercial use. See the [LICENSE](./LICENSE) file for details.

The data extracted and initial data is not included in this repository for privacy reasons.

The repository is organized in folders with the files in them. Here we provide general descriptions of each file but more specific descriptions can often be found in the files themselves in the first markdown cells or in the comments.

**PART1_WEBSCRAPING:**
<ul>
<li>twitter_scraping: the scraping code. The scraper iterates over the target users, visits their profile pages, and continuously scrolls down to load and extract their tweets within the desired date range. If it encounters a problem it logs into another account and continues the scraping where it left off. It uses MongoDB for parallel computation.</li>

<li>preprocessing: combines all the scraped tweets into a single master table.</li>

<li>download_data_mongo: automatically downloads to a local folder the usernames uploaded to mongo and deletes them from the cloud to optimize space.</li>
</ul>

**PART2_PROCESSDATA:**
<ul>
<li>processing1: filters tweets that fit certain criteria (language, dates...) and only keeps political tweets.</li>

<li>processing2: filters tweets and only keeps the ones that mention one political candidate.</li>
</ul>

**PART3_VECTORGRAPH:**
<ul>
<li>user_tweets: checks all the tweets of each user and helps to manually decide if the user is republican or democrat.</li>

<li>flair_textblob: sentiment generation code with the flair sentiment (vflair), flair confidence (cflair), and analogous for blob. It can be used to extend the sample of manual labeled users under certain conditions (more about it in the project article), as we did in opiniones.xlsx.</li>

<li>generate_vectors: aggregates all tweets of a single user and encodes it as a vector. It generates the file encoding_user_768.csv (it assumes there is a folder ../data/ where the data is stored, in other words, if you create a folder "code" to put this code you must create another one "data" with the same path).</li>

<li>similarity_edge_generation: from the previous file encoding_user_768.csv it generates the edges based on cosine similarity that saves to the file aristas_p_y.csv that will later be used to create the graph.</li>

<li>common_edge_generation: generates the other kind of edges: the ones that join users with a common tweet. Since all users needed to be crossed for this one it is very slow, that is why we use MongoDB to make it faster.</li>

<li>node_generation: generates the nodes of the graph, deciding if someone is republican or democrat based on the manual labels and the ones obtained with flair both in the file "opiniones.xlsx". The output is a file opinion_y.csv.</li>

<li>neo4j_cypher_queries.txt: queries needed to input in neo4jDesktop once created the databases with the tweets of each year. These queries load the nodes, the edges and the labels of the manually classified nodes and apply the three different contagion mechanisms that spread the democrat and republican labels throughout the graph.</li>

<li>exportneo4j: exports the graph. The year parameter must be the same as the one in the active database in neo4jDesktop.</li>
</ul>

**PART4_METHODCHECKING:**

<ul>
<li> generate_sample_tweets: takes a sample of 50 users from the graph colored by each type of propagation, in total 150 user nodes.</li> 
<li> evaluate_sample_tweets: it is used to classify the 150 user,year pairs manually. For each user and year it displays the tweets of that year and the person must enter as an input if the user appears to be a republican (rep) or a democrat (dem). Then when pressing enter the program saves it in a column of the dataframe and moves on to the next. At the end of the notebook the evaluated sample is stored as a csv file.</li>
</ul>
