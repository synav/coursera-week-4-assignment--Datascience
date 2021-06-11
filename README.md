# coursera-week-4-assignment--Datascience
import pandas as pd
import numpy as np
import scipy.stats as stats
import re
nhl_df=pd.read_csv("nhl.csv")
cities=pd.read_html("wikipedia_data.html")[1]
cities=cities.iloc[:-1,[0,3,5,6,7,8]]
#pattern='^[A-Z][a-z]*'
result=re.findall('\w[a-z]*',cities.iloc[0]['NHL'] )
result1=re.findall('\w[a-z]*',cities.iloc[1]['NHL'])
result[0:3]
citiesnhl=cities[['Metropolitan area','Population (2016 est.)[8]','NHL']]
citiesnhl=citiesnhl.set_index('Metropolitan area')
citiesnhl
new=pd.DataFrame(result[0:3], index=['New York City','New York City','New York City'])
new1=pd.DataFrame(result1,index=['Los Angeles','Los Angeles'])
#newdf=pd.DataFrame(new)
new['Population (2016 est.)[8]']=cities['Population (2016 est.)[8]'][0]
new=new.rename(columns={0:'NHL'})
new1['Population (2016 est.)[8]']=cities['Population (2016 est.)[8]'][1]
new1=new1.rename(columns={0:'NHL'})
frames=[citiesnhl, new, new1]
newdf=pd.concat(frames)
newdf=newdf.reset_index()
newdf=newdf.drop([0,1])
pattern='^[A-Z][a-z]*'
pattern1=(r'\[.*')
newdf['NHL1']=newdf.NHL.str.replace(pattern1,'')
newdf.drop('NHL',axis=1)
newdf['NHL']=newdf['NHL1']
finaldfcities=newdf.drop('NHL1',axis=1)
finaldfcities['NHL']=finaldfcities['NHL'][(finaldfcities['NHL']!='—') & (finaldfcities['NHL']!='')]
finaldfcities=finaldfcities.dropna().reset_index()
finaldfcities.columns
finaldfcities.pop('level_0')
finaldfcities=finaldfcities.sort_values('index')
finaldfcities=finaldfcities[['index','Population (2016 est.)[8]']]
finaldfcities.rename(columns={'index':'teamnew'}, inplace=True)
finaldfcities=finaldfcities.set_index('teamnew')
finaldfcities['Population (2016 est.)[8]']=finaldfcities['Population (2016 est.)[8]'].astype(float)
finaldfcities=finaldfcities.groupby('teamnew').sum()
finaldfcities.iloc[10]=np.divide(finaldfcities.iloc[10],2)
finaldfcities.iloc[15]=np.divide(finaldfcities.iloc[15],3)
finaldfcities





nhl_df=pd.read_csv("nhl.csv")
nhl_df=nhl_df[nhl_df['year']==2018]
nhl_df=nhl_df[['team','W','L']]
nhl_df=nhl_df.drop([0,9,18,26], axis=0)
nhl_df=nhl_df.reset_index().drop('index', axis=1)
nhl_df['W']=nhl_df['W'].astype(float)
nhl_df['L']=nhl_df['L'].astype(float)
#nhl_df['W/L ratio']=np.divide((nhl_df['W']),(nhl_df['W']+nhl_df['L']))
nhl_df['NHL']=nhl_df.team.str.findall('\w+\*$|\w+$')
nhl_df['teamnew']=nhl_df.team.str.replace('\w+\*$|\w+$','')
nhl_df['teamnew'].iloc[2]='Toronto'
nhl_df['teamnew'].iloc[4]='Detroit'
nhl_df['teamnew'].iloc[11]='Columbus'
nhl_df['teamnew'].iloc[23]='Las Vegas'
nhl_df.sort_values('teamnew')
nhl_df=nhl_df[['W','L','teamnew']]
nhl_df['teamnew'].iloc[24]='Los Angeles'
nhl_df['teamnew'].iloc[30]='Phoenix'
nhl_df['teamnew'].iloc[19]='Denver'
nhl_df['teamnew'].iloc[3]='Miami-Fort Lauderdale'
nhl_df['teamnew'].iloc[21]='Dallas-Fort Worth'
nhl_df['teamnew'].iloc[18]='Minneapolis-Saint Paul'
nhl_df['teamnew'].iloc[12]='New York City'
nhl_df['teamnew'].iloc[14:16]='New York City'
nhl_df['teamnew'].iloc[25]='San Francisco Bay Area'
nhl_df['teamnew'].iloc[0]='Tampa Bay Area'
nhl_df['teamnew'].iloc[8]='Washington, D.C.'
nhl_df['teamnew'].iloc[13]='Raleigh'
nhl_df=nhl_df.sort_values('teamnew')
nhl_df=nhl_df[['teamnew','W','L']]


nhl_df=nhl_df.set_index('teamnew')
nhl_df.index=['Boston', 'Buffalo', 'Calgary', 'Chicago', 'Columbus',
        'Dallas–Fort Worth', 'Denver', 'Detroit', 'Edmonton', 'Las Vegas',
        'Los Angeles', 'Los Angeles', 'Miami–Fort Lauderdale',
        'Minneapolis–Saint Paul', 'Montreal', 'Nashville', 'New York City',
        'New York City', 'New York City', 'Ottawa', 'Philadelphia', 'Phoenix',
        'Pittsburgh', 'Raleigh', 'San Francisco Bay Area', 'St. Louis',
        'Tampa Bay Area', 'Toronto', 'Vancouver', 'Washington, D.C.',
        'Winnipeg']
nhl_df.index.name='teamnew'
nhl_df['W/L ratio']=np.divide((nhl_df['W']),(nhl_df['W']+nhl_df['L']))
nhl_df=nhl_df[['W/L ratio']]
nhl_df=nhl_df.groupby('teamnew',as_index=True).mean()
nhl_df


finaldf=finaldfcities.merge(nhl_df, left_index=True,right_index=True)
finaldf.index.name='Metro'

finaldf=finaldf.groupby(['Metro','Population (2016 est.)[8]'], as_index=True).mean()
finaldf


number=stats.pearsonr(finaldfcities['Population (2016 est.)[8]'], nhl_df['W/L ratio'])
number


#WEEK 4 QUESTION 2
nba_df=pd.read_csv("nba.csv")
nba_df=nba_df[nba_df['year']==2018]
nba_df=nba_df[['team','W','L']]
nba_df['NBA']=nba_df.team.str.replace('\*\s\(\d\)','')
nba_df['NBA']=nba_df.NBA.str.replace('\(\d+\)','')
nba_df['team1']=nba_df.NBA.str.replace('\w+$','')
nba_df['team1']=nba_df.NBA.str.findall('\w+$|\s\w+\s$')
nba_df['NBA']=nba_df.NBA.str.replace('\w+$|\s\w+\s$','')
nba_df=nba_df[['NBA','W','L','team1']]
nba_df=nba_df.set_index('NBA')
nba_df=nba_df.sort_values('NBA')
#nba_df['NBA'][11]='New York City'
#nba_df['NBA'][9]='Raleigh'
nba_df=nba_df.reset_index()
nba_df['NBA'][2]='New York City'
nba_df['NBA'][6]='Dallas–Fort Worth'
nba_df['NBA'][9]='San Francisco Bay Area'
nba_df['NBA'][11]='Indianapolis'
nba_df['NBA'][15]='Miami–Fort Lauderdale'
nba_df['NBA'][17]='Minneapolis–Saint Paul'
nba_df['NBA'][19]='New York City'
nba_df['NBA'][24]='Portland'
nba_df['NBA'][28]='Salt Lake City'
nba_df['NBA'][29]='Washington, D.C.'
nba_df=nba_df.sort_values('NBA')
nba_df=nba_df.reset_index()
nba_df=nba_df[['NBA','W','L']]
nba_df['W']=nba_df['W'].astype(float)
nba_df['L']=nba_df['L'].astype(float)
nba_df=nba_df.groupby('NBA', as_index=True).sum()
nba_df.index=['Atlanta', 'Boston', 'Charlotte', 'Chicago', 'Cleveland',
       'Dallas–Fort Worth', 'Denver', 'Detroit', 'Houston', 'Indianapolis',
       'Los Angeles', 'Memphis', 'Miami–Fort Lauderdale', 'Milwaukee',
       'Minneapolis–Saint Paul', 'New Orleans', 'New York City',
       'Oklahoma City', 'Orlando', 'Philadelphia', 'Phoenix', 'Portland',
       'Sacramento', 'Salt Lake City', 'San Antonio', 'San Francisco Bay Area',
       'Toronto', 'Washington, D.C.']
nba_df['W/L ratio']=np.divide((nba_df['W']),(nba_df['W']+nba_df['L']))
nba_df


    cities=pd.read_html("wikipedia_data.html")[1]
    cities=cities.iloc[:-1,[0,3,5,6,7,8]]
    #pattern='^[A-Z][a-z]*'
    result=re.findall('\w[a-z]*',cities.iloc[0]['NHL'] )
    result1=re.findall('\w[a-z]*',cities.iloc[1]['NHL'])
    result[0:3]
    citiesnbl=cities[['Metropolitan area','Population (2016 est.)[8]','NHL','NBA']]
    citiesnbl=citiesnbl.set_index('Metropolitan area')
    citiesnbl=citiesnbl.sort_values('Metropolitan area')
    citiesnbl=citiesnbl.reset_index()
    citiesnbl=citiesnbl[['Metropolitan area', 'Population (2016 est.)[8]','NBA']]
    citiesnbl['NBA']=citiesnbl.NBA.str.replace('\[.+\]','—')
    citiesnbl['NBA']=citiesnbl['NBA'][(citiesnbl['NBA']!='—') & (citiesnbl['NBA']!='')]
    citiesnbl=citiesnbl.dropna()
    citiesnbl=citiesnbl.reset_index()
    citiesnbl=citiesnbl[['Metropolitan area','Population (2016 est.)[8]']]
    citiesnbl=citiesnbl.set_index('Metropolitan area')
    citiesnbl['Population (2016 est.)[8]']=citiesnbl['Population (2016 est.)[8]'].astype(float)
    citiesnbl
    
    
    finalnba=nba_df.merge(citiesnbl,left_index=True,right_index=True)
finalnba


numbernba=stats.pearsonr(finalnba['Population (2016 est.)[8]'], finalnba['W/L ratio'])
numbernba

#Q3 WEEK 4 assignment
import pandas as pd
import numpy as np
import scipy.stats as stats
import re

mlb_df=pd.read_csv("mlb.csv")
mlb_df=mlb_df[mlb_df['year']==2018]
mlb_df=mlb_df[['team','W','L']]
mlb_df['team1']=mlb_df.team.str.replace('\s\w+$','')
mlb_df['team1'][0]='Boston'
mlb_df['team1'][3]='Toronto'
mlb_df['team1'][8]='Chicago'

mlb_df.sort_values('team1')
mlb_df['team1'][27]='Phoenix'
mlb_df['team1'][14]='Dallas–Fort Worth'
mlb_df['team1'][26]='Denver'
mlb_df['team1'][19]='Miami–Fort Lauderdale'
mlb_df['team1'][6]='Minneapolis–Saint Paul'
mlb_df['team1'][1]='New York City'
mlb_df['team1'][18]='New York City'
mlb_df['team1'][11]='San Francisco Bay Area'
mlb_df['team1'][28]='San Francisco Bay Area'
mlb_df['team1'][2]='Tampa Bay Area'
mlb_df['team1'][16]='Washington, D.C.'
mlb_df['W']=mlb_df['W'].astype(float)
mlb_df['L']=mlb_df['L'].astype(float)
mlb_df=mlb_df.groupby('team1', as_index=True).sum()
mlb_df.index=['Atlanta', 'Baltimore', 'Boston', 'Chicago', 'Cincinnati', 'Cleveland',
       'Dallas–Fort Worth', 'Denver', 'Detroit', 'Houston', 'Kansas City',
       'Los Angeles', 'Miami–Fort Lauderdale', 'Milwaukee',
       'Minneapolis–Saint Paul', 'New York City', 'Philadelphia', 'Phoenix',
       'Pittsburgh', 'San Diego', 'San Francisco Bay Area', 'Seattle',
       'St. Louis', 'Tampa Bay Area', 'Toronto', 'Washington, D.C.']
mlb_df['W/L ratio']=np.divide((mlb_df['W']),(mlb_df['W']+mlb_df['L']))
mlb_df

    cities=pd.read_html("wikipedia_data.html")[1]
    cities=cities.iloc[:-1,[0,3,5,6,7,8]]
    citiesmlb=cities[['Metropolitan area','Population (2016 est.)[8]','MLB']]
    citiesmlb=citiesmlb.set_index('Metropolitan area')
    citiesmlb=citiesmlb.sort_values('Metropolitan area')
    citiesmlb['MLB']=citiesmlb.MLB.str.replace('\[.+\]','—')
    citiesmlb['MLB']=citiesmlb['MLB'][(citiesmlb['MLB']!='—') & (citiesmlb['MLB']!='')]
    citiesmlb=citiesmlb.dropna()
    citiesmlb=citiesmlb[['Population (2016 est.)[8]']].astype(float)
    citiesmlb
    
    
    numbermlb=stats.pearsonr(citiesmlb['Population (2016 est.)[8]'], mlb_df['W/L ratio'])
numbermlb


#Q4 WEEK 4 assignment

    cities=pd.read_html("wikipedia_data.html")[1]
    cities=cities.iloc[:-1,[0,3,5,6,7,8]]
    citiesnfl=cities[['Metropolitan area','Population (2016 est.)[8]','NFL']]
    citiesnfl=citiesnfl.set_index('Metropolitan area')
    citiesnfl=citiesnfl.sort_values('Metropolitan area')
    citiesnfl['NFL']=citiesnfl.NFL.str.replace('\[.+\]','—')
    citiesnfl['NFL']=citiesnfl['NFL'][(citiesnfl['NFL']!='—') & (citiesnfl['NFL']!='')]
    citiesnfl=citiesnfl.dropna()
    citiesnfl=citiesnfl[['Population (2016 est.)[8]','NFL']]
    citiesnfl=citiesnfl.drop(index=['Toronto'])
    citiesnfl['Population (2016 est.)[8]']=citiesnfl['Population (2016 est.)[8]'].astype(float)
    citiesnfl
    
    nfl_df=pd.read_csv("nfl.csv")
nfl_df=nfl_df[nfl_df['year']==2018]
nfl_df=nfl_df[['team','W','L']]
nfl_df=nfl_df.drop(index=[0,5,10,15,20,25,30,35])
nfl_df['team1']=nfl_df.team.str.findall('\w+\s')
nfl_df['team1'][1]='Boston'
nfl_df['team1'][2]='Miami–Fort Lauderdale'
nfl_df['team1'][4]='New York City'
nfl_df['team1'][24]='New York City'
nfl_df['team1'][13]='Nashville'
nfl_df['team1'][19]='San Francisco Bay Area' 
nfl_df['team1'][38]='San Francisco Bay Area'
nfl_df['team1'][21]='Dallas–Fort Worth'
nfl_df['team1'][23]='Washington, D.C.'
nfl_df['team1'][27]='Minneapolis–Saint Paul'
nfl_df['team1'][32]='Charlotte'
nfl_df['team1'][34]='Tampa Bay Area'
nfl_df['team1'][39]='Phoenix'
nfl_df['team1']=nfl_df['team1'].apply(','.join)
nfl_df['team1']=nfl_df['team1'].str.replace(',','')
nfl_df=nfl_df.sort_values('team1')
nfl_df=nfl_df[['team1','W','L']]
nfl_df['W']=nfl_df['W'].astype(float)
nfl_df['L']=nfl_df['L'].astype(float)
nfl_df=nfl_df.groupby('team1',as_index=True).sum()
nfl_df.index=['Atlanta', 'Baltimore', 'Boston', 'Buffalo', 'Charlotte', 'Chicago',
       'Cincinnati', 'Cleveland', 'Dallas–Fort Worth', 'Denver', 'Detroit',
       'Green Bay', 'Houston', 'Indianapolis', 'Jacksonville', 'Kansas City',
       'Los Angeles', 'Miami–Fort Lauderdale', 'Minneapolis–Saint Paul',
       'Nashville', 'New Orleans', 'New York City', 'Philadelphia', 'Phoenix',
       'Pittsburgh', 'San Francisco Bay Area', 'Seattle', 'Tampa Bay Area',
       'Washington, D.C.']
nfl_df['W/L ratio']=np.divide((nfl_df['W']),(nfl_df['W']+nfl_df['L']))
nfl_df

numbernfl=stats.pearsonr(citiesnfl['Population (2016 est.)[8]'], nfl_df['W/L ratio'])
numbernfl

numbernfl=stats.pearsonr(citiesnfl['Population (2016 est.)[8]'], nfl_df['W/L ratio'])
numbernfl

numbernfl=stats.pearsonr(citiesnfl['Population (2016 est.)[8]'], nfl_df['W/L ratio'])
numbernfl


nhl_df_nba_df=nhl_df.merge(nba_df,left_index=True, right_index=True)
nhl_df_nba_df
a=nhl_df_nba_df['W/L ratio_x']
b=nhl_df_nba_df['W/L ratio_y']

NBANHL=stats.ttest_rel(a, b, axis=0)[1]
NBANHL

[NBANHL,NHLMLB,NHLNFL,MLBNFL,NBAMLB,NBANFL]
pvaluedf=pd.DataFrame(columns=['NFL', 'NBA', 'NHL', 'MLB'],index=['NFL', 'NBA', 'NHL', 'MLB'])
pvaluedf
values={0.02229704964343875,
 0.0007114451156501551,
 0.030883173359891308,
 0.8023839276872875,
 0.951046117643026,
 0.941792047733962}
sports = ['NFL', 'NBA', 'NHL', 'MLB']
p_values = pd.DataFrame({k:np.nan for k in sports}, index=sports)
p_values['NFL']=[np.nan,0.941792047733962,0.030883173359891308,0.8023839276872875]
p_values['NBA']=[0.941792047733962,np.nan,0.02229704964343875,0.951046117643026]
p_values['NHL']=[0.030883173359891308,0.02229704964343875,np.nan,0.0007114451156501551]
p_values['MLB']=[ 0.8023839276872875,0.951046117643026,0.0007114451156501551,np.nan]
p_values


