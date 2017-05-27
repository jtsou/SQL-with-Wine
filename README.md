<h1>SQL FUN WITH WINE DATA SET</h1>

<h1>Purpose</h1>
<p>
As a wine enthusiast, choosing the right wine for the right mood along with the struggle of getting the right quality of wine can get frustrating. There are apps such as Vivino that can help me, but doing research and make informed decision based on my analysis before choosing wine at dinner is always a good practice...and fun. 
</p>

<h1>Process</h1>
<p>
I start the process by downloading dataset from the internet. The dataset has three csv files. Wine.csv, Grapes.csv, and Appellations.csv. To extract data from each table to reach my desired result, I performed aggregation function and multiple join table operations. I plan to use SQLITE3 because it is built-in with Mac OS system. Although, the function is limited, it does the trick.
I will show my process as below later.
</p>

<h1>Design</h1>
<p>
I started wanting to explore the size of data set. But to do that first, I need to know the variables in the data. After I typed in .schema wine (I imported the csv file under the name wine), list of variables came out. 
</p>

<ul>
sqlite> .schema wine
<ul>
CREATE TABLE wine(</ul>
  <ul>"No" TEXT,</ul>
  <ul>"Grape" TEXT,</ul>
  <ul>"Winery" TEXT,</ul>
  <ul>"Appelation" TEXT,</ul>
  <ul>"State" TEXT,</ul>
  <ul>"Name" TEXT,</ul>
  <ul>"Year" TEXT,</ul>
  <ul>"Price" TEXT,</ul>
  <ul>"Score" TEXT,</ul>
  <ul>"Cases" TEXT,</ul>
  <ul>"Drink" TEXT</ul>
<ul>);
</ul>
</ul>

I then performed count function to find out the data size:

sqlite> SELECT count(*) FROM wine;
<ul>
500
</ul>

With that information in mind, I have an idea of the size of data. 
I want to find out first what is the highest score that is associated with the Winery, and the Grape as well:

sqlite> SELECT max(score), Winery, Grape FROM wine;
<ul>
98,"'Sine Qua Non’","'Syrah'"
</ul>

To combine the tables, I need to know what the data are like for other csv files and the variables: 
I performed .schema on each of the csv files - Appellations and Grapes.

The result is the following: 


sqlite> .schema GRAPES
<ul>
CREATE TABLE GRAPES(</ul>
  <ul>"ID" TEXT,</ul>
  <ul>"Grape" TEXT,</ul>
  <ul>"Color" TEXT
);
</ul>

sqlite> .schema APPELLATIONS
<ul>
CREATE TABLE APPELLATIONS(</ul>
 <ul> "No" TEXT,</ul>
 <ul> "Appelation" TEXT,</ul>
 <ul>"County" TEXT,</ul>
 <ul>"State" TEXT,</ul>
 <ul>"Area" TEXT,</ul>
 <ul>"isAVA" TEXT
);
</ul>

After I discovered this, I want to combine this data to Grapes.csv in JOIN table. I imported Grapes.csv under the name GRAPES. I want to find out the top 10 quality of Grapes with Winery that are associated with scores. 

sqlite> SELECT wine.Winery, wine.Score, wine.Grape, GRAPES.Grape FROM wine LEFT JOIN GRAPES on wine.Grape=GRAPES.Grape GROUP BY Winery ORDER BY Score DESC LIMIT 10;
<ul>
"'Hourglass'",97,"'Cabernet Sauvingnon'","'Cabernet Sauvingnon'"</ul>
<ul>"'Chappellet'",96,"'Cabernet Sauvingnon'","'Cabernet Sauvingnon'"</ul>
<ul>"'Dancing Hares'",95,"'Cabernet Sauvingnon'","'Cabernet Sauvingnon'"</ul>
<ul>"'Favia'",95,"'Syrah'","'Syrah'"</ul>
<ul>"'Kazme & Blaise'",95,"'Chardonnay'","'Chardonnay'"</ul>
<ul>"'Lewis'",95,"'Cabernet Sauvingnon'","'Cabernet Sauvingnon'"</ul>
<ul>"'Maybach'",95,"'Chardonnay'","'Chardonnay'"</ul>
<ul>"'Three Sticks'",95,"'Chardonnay'","'Chardonnay'"</ul>
<ul>"'Tor'",95,"'Chardonnay'","'Chardonnay'"</ul>
<ul>"'Caymus'",94,"'Cabernet Sauvingnon'","'Cabernet Sauvingnon’"
</ul>

I also would want to find out what Area produce the best quality wine, looks like North Coast is the winner with Cabernet Sauvingnon. This makes me think that North Coast area produce the best Cabernet Sauvingnon grapes.

sqlite> SELECT wine.Winery, wine.Score, wine.Grape, APPELLATIONS.Area FROM wine JOIN APPELLATIONS on wine.No = APPELLATIONS.No GROUP BY Score ORDER BY Score DESC LIMIT 10;

<ul>"'Chappellet'",96,"'Cabernet Sauvingnon'","'North Coast'"</ul>
<ul>"'Ramey'",95,"'Cabernet Sauvingnon'","'North Coast'"</ul>
<ul>"'Round Pond Estate'",94,"'Cabernet Sauvingnon'","'North Coast'"</ul>
<ul>"'Chiarello Family'",93,"'Zinfandel'","'Sierra Foothills'"</ul>
<ul>"'Janzen'",92,"'Cabernet Sauvingnon'","'North Coast'"</ul>
<ul>"'Peter Michael'",91,"'Sauvignon Blanc'","'Central Coast'"</ul>
<ul>"'Round Pond Estate'",90,"'Sauvignon Blanc'","'North Coast'"</ul>
<ul>"'Selene'",89,"'Sauvignon Blanc'","'North Coast'"</ul>
<ul>"'Sbragia Family'",88,"'Sauvignon Blanc'","'North Coast'"</ul>
<ul>"'White Oak'",87,"'Sauvignon Blanc'","'North Coast'"</ul>

<p>
North Coast is a huge area, so I want to narrow down the geographical location more.
</p>

sqlite> SELECT wine.Winery, wine.Score, wine.Grape, APPELLATIONS.Area, APPELLATIONS.County FROM wine JOIN APPELLATIONS on wine.No = APPELLATIONS.No GROUP BY Score ORDER BY Score DESC LIMIT 10;

<ul>"'Chappellet'",96,"'Cabernet Sauvingnon'","'North Coast'","'Sonoma'"</ul>
<ul>"'Ramey'",95,"'Cabernet Sauvingnon'","'North Coast'","'N/A'"</ul>
<ul>"'Round Pond Estate'",94,"'Cabernet Sauvingnon'","'North Coast'","'Napa'"</ul>
<ul>"'Chiarello Family'",93,"'Zinfandel'","'Sierra Foothills'","'Amador'"</ul>
<ul>"'Janzen'",92,"'Cabernet Sauvingnon'","'North Coast'","'Napa'"</ul>
<ul>"'Peter Michael'",91,"'Sauvignon Blanc'","'Central Coast'","'Monterey'"</ul>
<ul>"'Round Pond Estate'",90,"'Sauvignon Blanc'","'North Coast'","'Sonoma'"</ul>
<ul>"'Selene'",89,"'Sauvignon Blanc'","'North Coast'","'Mendocino'"</ul>
<ul>"'Sbragia Family'",88,"'Sauvignon Blanc'","'North Coast'","'Napa'"</ul>
<ul>"'White Oak'",87,"'Sauvignon Blanc'","'North Coast'","'Napa'"</ul>

Napa and Sonoma produce the top rated wine.

<h3>Reflection </h3>
This is a rather short project; however, I was able to practice SQL with it. From the data extraction, we can roughly see how Napa and Sonoma produce the top rated wine, regardless of year. So next time I know what to get if I feel adventureous. 
