<h1>SQL FUN WITH WINE DATA SET</h1>

<h1>Purpose</h1>
<p>
As a wine enthusiast, choosing the right wine for the right mood along with the struggle of getting the right quality of wine can get frustrating. There are apps such as Vivino that can help me, but doing research and make informed decision based on my analysis before choosing wine at dinner is always a good practice...and fun. 
</p>

<h1>Process</h1>
<p>
I start the process by downloading dataset from the internet. The dataset has three csv files. Wine.csv, Grapes.csv, and Appellations.csv. To extract data from each table to reach my desired result, I performed aggregation function and multiple join table operations. 
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


