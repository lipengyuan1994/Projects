# This folder contains csv files of this project



-------


### 1.Wayfair-Babson\_HackathonData 2019.csv: 
1. the row data provided by wayfair.

-------

### 2. data_with_null.csv: 
1. the row data after feature engineering(One-hot encoding on categorical features); 
2. without removing any null values; 
3. **without** feature engineering on feature **States**; 
4. the shape of this file is (992977, 35)

-------

### 3.data_with_null_v2.csv: 
1. Handled feature State, new feature generated based on State.
2. Feature : state_unknown, state_nonusa, state_territory, state_usa added.
```
data2['state_unknown'] = data2.State.isnull()
data2 = data2.applymap(lambda x: 1 if x == True else x)
data2 = data2.applymap(lambda x: 0 if x == False else x)
data2['state_territory'] = data2.State.apply(lambda x:1 if x in usa_territory else 0)
data2['state_nonusa'] = data2.State.apply(lambda x:1 if x in non_usa else 0)
data2['state_usa'] = data2.State.apply(lambda x:1 if x in state_usa else 0)
```
3. The shape of this file is (992977, 38)


-------

### 4.USAstate_by_area.csv: 
1. Feature State handled by US area. Only keep visit records by USA visitors.
2. New features generated as the codes behind:
```
Eastern =  ['AL','CT','DC','DE','FL','GA','IL','KY','MA','MD','ME',
        'MI','MS','NC','NH','NH','NY','OH','PA','RI','SC','TN','VA','VT','WV']
West_Coast = ['WA','OR','CA']
Mid_West = ['AR','CO','IA','ID','IN','KS','LA','MN','MO','MT','MD','ME','MV','OK','SD','UT','WI','WY']
South_West = ['AZ','NM','TX']
Other =  ['AK','HI','PR','VI']
wf['Eastern'] = wf.State.apply(lambda x:1 if x in Eastern else 0)
wf['West_Coast'] = wf.State.apply(lambda x:1 if x in West_Coast else 0)
wf['Mid_West']  = wf.State.apply(lambda x:1 if x in Mid_West else 0)
wf['South_West'] = wf.State.apply(lambda x:1 if x in South_West else 0)
wf['Other'] = wf.State.apply(lambda x:1 if x in Other else 0)
wf = wf.drop('State', axis=1)
wf.shape
```
1. The shape of this file is (406942, 39)

-------

### 5.dataforkmeans_v1.csv
1. Remove missing values in Gender
2. State segmented by US area, remove non_usa visit records, the method used is the same as in the file USAstate_by_area.csv
3. The shape of this files is (369393, 38)
4. This file is for k-means cluster analysis.

-------
### 6.dataforkmeans_v2.csv
1. Generate new feature 'b_effective' (banner effective)and droped features ClickedBanner, AddedtoBasket, (Purchased)?
```
%%time
data = data.reset_index()
data['b_effective'] = 0
for i in range(len(data.index)):
    if data['ClickedBanner'][i] == 1 and (data['AddedToBasket'][i] ==1 or data['Purchased'][i]==1):
        data['b_effective'][i] =1
data=data.drop(['AddedToBasket','ClickedBanner','Purchased'],axis =1)
```
3. Here is my thought: the goal of this project is to analyze the effect of the banner, and since each uniquevisitid is unique. Therefore I made an assumption that :if someone clicked the banner and then added this item to basket   or    after clicking this banner the consumer purchased this item, the banner is effective to this consumer.
4. The shape of this file is (369393, 34)

-------

### 7.dataforkmeans_v3.csv
1. Generate new feature 'b_effective' (banner effective)and droped features ClickedBanner, AddedtoBasket, Purchased with code below
```
data['b_effective'] = 0
for i in range(len(data.index)):
    if data['ClickedBanner'][i] == 1 and data['AddedToBasket'][i]==0 and data['Purchased'][i]==0 :
        data['b_effective'][i]=30
    elif data['ClickedBanner'][i] == 1 and data['AddedToBasket'][i]==1 and data['Purchased'][i]==0:
        data['b_effective'][i]=60
    elif data['ClickedBanner'][i] == 1 and data['Purchased'][i]==1:
        data['b_effective'][i]=100
# data=data.drop(['AddedToBasket','ClickedBanner','Purchased'],axis =1)
```
1. Shape (369393, 34)

### 8.data_categorical.csv (Updated on 27th Feb)
1. The feature State was reassigned by state area, but still categorical feature;
2. Feature b_effective generated by 
```
data['b_effective'] = 0
for i in range(len(data.index)):
    
    if data['ClickedBanner'][i] == 1 and (data['AddedToBasket'][i]==1 or data['Purchased'][i]==1):
        data['b_effective'][i]=1
```
2. More info, please see  ***2.1 Data preprocessing.ipynb***
3. Shape of this file (992977, 17)


### 9.n_cluster2.csv (Updated on 27th Feb)
1. labeled data from data_categorical.csv
2. K-means Clustering technique: kmodes.KPrototypes
3. The number of cluster is **2**. 
4. Shape (381111, 16)
5. More info, please see ***3.5 K modes.ipynb_v2***.

### 10.n_cluster3.csv (Updated on 27th Feb)
1. labeled data from data_categorical.csv
2. K-means Clustering technique: kmodes.KPrototypes
3. The number of cluster is **3**. 
4. Shape (381111, 16)
5. More info, please see ***3.5 K modes.ipynb_v2***

### 11.n_cluster4.csv (Updated on 27th Feb)
1. labeled data from data_categorical.csv
2. K-means Clustering technique: kmodes.KPrototypes
3. The number of cluster is **4**. 
4. Shape (381111, 16)
5. More info, please see ***3.5 K modes.ipynb_v2***
