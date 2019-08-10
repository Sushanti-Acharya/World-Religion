# World-Religion

import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
import plotly.offline as py
py.init_notebook_mode(connected=True)
import plotly.graph_objs as go
import plotly.tools as tls
from wordcloud import WordCloud, STOPWORDS
from scipy.misc import imread
import base64

wrp_reg = pd.read_csv('../input/wrp-reg/regional.csv')

wrp_reg

print(wrp_reg['region'].unique())

#fig = plt.figure(figsize=(10, 8))
fig, axes = plt.subplots(nrows=1, ncols=3)
colormap = plt.cm.viridis_r
# fig = plt.figure(figsize=(20, 10))
# plt.subplot(1)
christianity_year = wrp_reg.groupby(['year','region']).christianity_all.sum()
christianity_year.unstack().plot(kind='area',stacked=True,  colormap= colormap, grid=False,ax= axes[0],figsize=(20,10) , legend=False)
axes[0].set_title('Christianity Followers',y=1.08,size=12)
axes[0].set_ylabel('Billions', color='gray')

# plt.subplot(2)
islam_year = wrp_reg.groupby(['year','region']).islam_all.sum()
islam_year.unstack().plot(kind='area',stacked=True,  colormap= colormap, grid=False, ax= axes[1], legend= False)
axes[1].set_title('Islam Followers',y=1.08,size=12)
axes[1].set_ylabel('Billions', color='gray')

# plt.subplot(3)
judaism_year = wrp_reg.groupby(['year','region']).judaism_all.sum()
judaism_year.unstack().plot(kind='area',stacked=True,  colormap= colormap , grid=False, ax= axes[2])
axes[2].legend(bbox_to_anchor=(-1.7, -0.3, 2, 0.1), loc=10,prop={'size':12},
           ncol=5, mode="expand", borderaxespad=0.)
axes[2].set_title('Judaism Adherents',y=1.08,size=12)
axes[2].set_ylabel('Billions', color='gray')

plt.tight_layout()
plt.show()



#fig = plt.figure(figsize=(10, 8))
fig, axes = plt.subplots(nrows=1, ncols=3)
colormap = plt.cm.autumn_r
# fig = plt.figure(figsize=(20, 10))
# plt.subplot(1)
hinduism_year = wrp_reg[wrp_reg['region'] != 'Asia'].groupby(['year','region']).hinduism_all.sum()
hinduism_year.unstack().plot(kind='area',stacked=True,  colormap= colormap, grid=False,ax= axes[0],figsize=(20,10) , legend=False)
axes[0].set_title('Hindusim Followers',y=1.08,size=12)

# plt.subplot(2)
sikhism_year = wrp_reg[wrp_reg['region'] != 'Asia'].groupby(['year','region']).sikhism_all.sum()
sikhism_year.unstack().plot(kind='area',stacked=True,  colormap= colormap, grid=False, ax= axes[1], legend= False)
axes[1].set_title('Sikhism Followers',y=1.08,size=12)

# plt.subplot(3)
jainism_year = wrp_reg[wrp_reg['region'] != 'Asia'].groupby(['year','region']).jainism_all.sum()
jainism_year.unstack().plot(kind='area',stacked=True,  colormap= colormap, grid=False, ax= axes[2])
axes[2].legend(bbox_to_anchor=(-1.7, -0.3, 2, 0.1), loc=10,prop={'size':12},
           ncol=5, mode="expand", borderaxespad=0.)
axes[2].set_title('Jainism Followers',y=1.08,size=12)

plt.tight_layout()
plt.show()



#fig = plt.figure(figsize=(8, 5))
fig, axes = plt.subplots(nrows=1, ncols=3)
colormap = plt.cm.tab20_r
# fig = plt.figure(figsize=(20, 10))
# plt.subplot(1)
buddhist_year = wrp_reg[wrp_reg['region'] != 'Asia'].groupby(['year','region']).buddhism_all.sum()
buddhist_year.unstack().plot(kind='area',stacked=True,  colormap= colormap, grid=False,ax= axes[0],figsize=(20,10) , legend=False)
axes[0].set_title('Buddhist Followers',y=1.08,size=12)

# plt.subplot(2)
taoism_year = wrp_reg[wrp_reg['region'] != 'Asia'].groupby(['year','region']).taoism_all.sum()
taoism_year.unstack().plot(kind='area',stacked=True,  colormap= colormap, grid=False, ax= axes[1], legend= False)
axes[1].set_title('Taoist Followers',y=1.08,size=12)

# plt.subplot(3)
shinto_year = wrp_reg[wrp_reg['region'] != 'Asia'].groupby(['year','region']).shinto_all.sum()
shinto_year.unstack().plot(kind='area',stacked=True,  colormap= colormap, grid=False, ax= axes[2])
axes[2].legend(bbox_to_anchor=(-1.7, -0.3, 2, 0.1), loc=10,prop={'size':12},
           ncol=5, mode="expand", borderaxespad=0.)
axes[2].set_title('Shinto Followers',y=1.08,size=12)

plt.tight_layout()
plt.show()



mport altair as alt
alt.renderers.enable('notebook')
interval = alt.selection_interval()

points = alt.Chart(wp_reg).mark_point().encode(
  x='total muslims:O',
  y='year:O', 
  color=alt.condition(interval, 'region', alt.value('lightgray'))
).properties(
  selection=interval
)

histogram = alt.Chart(wp_reg).mark_bar().encode(
  x='count()',
  y='region',
  color='region'
).transform_filter(interval)

points & histogram


source = wp_reg

brush = alt.selection(type='interval', resolve='global')

base = alt.Chart(source).mark_point().encode(
    y='year:O',
    color=alt.condition(brush, 'region', alt.ColorValue('gray'))
).add_selection(
    brush
).properties(
    width=300,
    height=300
)

base.encode(x='total christians:O') | base.encode(x='total muslims:O')


national_2010 = wp_nat[wp_nat['year'] == 2010]
religion_list = []
for col in national_2010.columns:
    if 'total' in col:
        religion_list.append(col)
        
        

metricscale1=[[0,"rgb(5, 10, 172)"],[0.35,"rgb(40, 60, 190)"],[0.5,"rgb(70, 100, 245)"],[0.6,"rgb(90, 120, 245)"],[0.7,"rgb(106, 137, 247)"],[1,"rgb(220, 220, 220)"]]

# Mercator plots for the Buddhism
data = [ dict(
        type = 'choropleth',
        locations = national_2010['name'],
        z = national_2010['total buddhists'],
        text = national_2010['name'].unique(),
        colorscale = metricscale1,
        autocolorscale = False,
        reversescale = True,
        marker = dict(
            line = dict (
                color = 'rgb(200,200,200)',
                width = 0.5
            ) ),
        colorbar = dict(
            autotick = False,
            title = 'Number of Buddhist Adherents'),
      ) ]

layout = dict(
    title = 'Spread of Buddhist adherents in 2010',
    geo = dict(
        scope = 'Africa' 'Asia' 'Europe' 'Mideast' 'West. Hem',
        showframe = False,
        showocean = True,
        oceancolor = 'rgb(0,0,50)',
       #oceancolor = 'rgb(232,243,246)',
        #oceancolor = ' rgb(28,107,160)',
        showcoastlines = True,
        projection = dict(
            type = 'Mercator'
        )
    )
)

fig = dict( data=data, layout=layout )
py.iplot( fig, validate=False, filename='d3-world-map' )



# Mercator plots for Hinduism
data1 = [ dict(
        type = 'choropleth',
        locations = national_2010['name'],
        z = national_2010['total hindus'],
        text = national_2010['name'].unique(),
        colorscale = metricscale1,
        autocolorscale = False,
        reversescale = True,
        marker = dict(
            line = dict (
                color = 'rgb(200,200,200)',
                width = 0.5
            ) ),
        colorbar = dict(
            autotick = False,
            title = 'Number of Hinduism Adherents'),
      ) ]

layout1 = dict(
    title = 'Spread of Hinduism adherents in 2010',
    geo = dict(
        scope = 'Africa' 'Asia' 'Europe' 'Mideast' 'West. Hem',
        showframe = False,
        showocean = True,
        oceancolor = 'rgb(0,0,50)',
#         oceancolor = 'rgb(232,243,246)',
        #oceancolor = ' rgb(28,107,160)',
        showcoastlines = True,
        projection = dict(
            type = 'Mercator'
        )
    )
)

fig = dict( data=data1, layout=layout1 )
py.iplot( fig, validate=False, filename='world-map' )


# Mercator plots for christians
data2 = [ dict(
        type = 'choropleth',
        locations = national_2010['name'],
        z = national_2010['total christians'],
        text = national_2010['name'].unique(),
        colorscale = metricscale1,
        autocolorscale = False,
        reversescale = True,
        marker = dict(
            line = dict (
                color = 'rgb(200,200,200)',
                width = 0.5
            ) ),
        colorbar = dict(
            autotick = False,
            title = 'Number of christians Adherents'),
      ) ]

layout2 = dict(
    title = 'Spread of christians adherents in 2010',
    geo = dict(
        scope = 'Africa' 'Asia' 'Europe' 'Mideast' 'West. Hem',
        showframe = False,
        showocean = True,
        oceancolor = 'rgb(0,0,50)',
#         oceancolor = 'rgb(232,243,246)',
        #oceancolor = ' rgb(28,107,160)',
        showcoastlines = True,
        projection = dict(
            type = 'Mercator'
        )
    )
)

fig = dict( data=data2, layout=layout2 )
py.iplot( fig, validate=False, filename='d3-world-map' )



#no religion,screntasim
# Mercator plots for non religious
data4 = [ dict(
        type = 'choropleth',
        locations = national_2010['name'],
        z = national_2010['non religious'],
        text = national_2010['name'].unique(),
        colorscale = metricscale1,
        autocolorscale = False,
        reversescale = True,
        marker = dict(
            line = dict (
                color = 'rgb(200,200,200)',
                width = 0.5
            ) ),
        colorbar = dict(
            autotick = False,
            title = 'Number of  Non religious Adherents'),
      ) ]

layout4 = dict(
    title = 'Spread of Non religious adherents in 2010',
    geo = dict(
        scope = 'Africa' 'Asia' 'Europe' 'Mideast' 'West. Hem',
        showframe = False,
        showocean = True,
        oceancolor = 'rgb(0,0,50)',
#         oceancolor = 'rgb(232,243,246)',
        #oceancolor = ' rgb(28,107,160)',
        showcoastlines = True,
        projection = dict(
            type = 'Mercator'
        )
    )
)

fig = dict( data=data4, layout=layout4 )
py.iplot( fig, validate=False, filename='d3-world-map' )

#total muslims

# Mercator plots for Islam
data5 = [ dict(
        type = 'choropleth',
        locations = national_2010['name'],
        z = national_2010['total muslims'],
        text = national_2010['name'].unique(),
        colorscale = metricscale1,
        autocolorscale = False,
        reversescale = True,
        marker = dict(
            line = dict (
                color = 'rgb(200,200,200)',
                width = 0.5
            ) ),
        colorbar = dict(
            autotick = False,
            title = 'Number of Islam Adherents'),
      ) ]

layout5 = dict(
    title = 'Spread of Islam adherents in 2010',
    geo = dict(
        scope = 'Africa' 'Asia' 'Europe' 'Mideast' 'West. Hem',
        showframe = False,
        showocean = True,
        oceancolor = 'rgb(0,0,50)',
        showcoastlines = True,
        projection = dict(
            type = 'Mercator'
        )
    )
)

fig = dict( data=data5, layout=layout5 )
py.iplot( fig, validate=False, filename='d3-world-map' )

# Mercator plots for 
data6 = [ dict(
        type = 'choropleth',
        locations = national_2010['name'],
        z = national_2010['total syncretics'],
        text = national_2010['name'].unique(),
        colorscale = metricscale1,
        autocolorscale = False,
        reversescale = True,
        marker = dict(
            line = dict (
                color = 'rgb(200,200,200)',
                width = 0.5
            ) ),
        colorbar = dict(
            autotick = False,
            title = 'Number of syncretics Adherents'),
      ) ]

layout6 = dict(
    title = 'Spread of syncretics adherents in 2010',
    geo = dict(
        scope = 'Africa' 'Asia' 'Europe' 'Mideast' 'West. Hem',
        showframe = False,
        showocean = True,
        oceancolor = 'rgb(0,0,50)',
        showcoastlines = True,
        projection = dict(
            type = 'Mercator'
        )
    )
)

fig = dict( data=data6, layout=layout6 )
py.iplot( fig, validate=False, filename='d3-world-map' )



metricscale1=[[0.0,"rgb(20, 40, 190)"],[0.05,"rgb(40, 60, 190)"],[0.25,"rgb(70, 100, 245)"],[0.6,"rgb(90, 120, 245)"],[0.7,"rgb(106, 137, 247)"],[1,"rgb(220, 220, 220)"]]
data = [ dict(
        type = 'choropleth',
        autocolorscale = False,
        colorscale = 'Viridis',
        reversescale = True,
        showscale = True,
        locations = national_2010['name'].values,
        z = national_2010['total christians'].values,
        locationmode = 'name',
        text = national_2010['name'].values,
        marker = dict(
            line = dict(color = 'rgb(200,200,200)', width = 0.5)),
            colorbar = dict(autotick = True, tickprefix = '', 
            title = 'Number of Christian Adherents')
            )
       ]

layout = dict(
    title = 'Christian Adherents in 2010',
    geo = dict(
        showframe = True,
        showocean = True,
        oceancolor = 'rgb(0,0,0)',
        #oceancolor = 'rgb(222,243,246)',
        projection = dict(
        type = 'orthographic',
            rotation = dict(
                    lon = 60,
                    lat = 10),
        ),
        lonaxis =  dict(
                showgrid = False,
                gridcolor = 'rgb(102, 102, 102)'
            ),
        lataxis = dict(
                showgrid = False,
                gridcolor = 'rgb(102, 102, 102)'
                )
            ),
        )
fig = dict(data=data, layout=layout)
py.iplot(fig, validate=False, filename='worldmap2010')
#Map_ISLAM
data_1 = [ dict(
        type = 'choropleth',
        autocolorscale = False,
        colorscale = 'Viridis',
        reversescale = True,
        showscale = True,
        locations = national_2010['name'].values,
        z = national_2010['total muslims'].values,
        locationmode = 'name',
        text = national_2010['name'].values,
        marker = dict(
            line = dict(color = 'rgb(200,200,200)', width = 0.5)),
            colorbar = dict(autotick = True, tickprefix = '', 
            title = 'Number of Islam Adherents')
            )
       ]

layout_1 = dict(
    title = 'Islam Adherents in 2010',
    geo = dict(
        showframe = True,
        showocean = True,
        oceancolor = 'rgb(0,0,0)',
        #oceancolor = 'rgb(222,243,246)',
        projection = dict(
        type = 'orthographic',
            rotation = dict(
                    lon = 60,
                    lat = 10),
        ),
        lonaxis =  dict(
                showgrid = False,
                gridcolor = 'rgb(102, 102, 102)'
            ),
        lataxis = dict(
                showgrid = False,
                gridcolor = 'rgb(102, 102, 102)'
                )
            ),
        )
fig = dict(data=data_1, layout=layout_1)
py.iplot(fig, validate=False, filename='worldmap2010')


data_2 = [ dict(
        type = 'choropleth',
        autocolorscale = False,
        colorscale = 'Viridis',
        reversescale = True,
        showscale = True,
        locations = national_2010['name'].values,
        z = national_2010['total hindus'].values,
        locationmode = 'name',
        text = national_2010['name'].values,
        marker = dict(
            line = dict(color = 'rgb(200,200,200)', width = 0.5)),
            colorbar = dict(autotick = True, tickprefix = '', 
            title = 'Number of Hindu Adherents')
            )
       ]

layout_2 = dict(
    title = 'Hindus Adherents in 2010',
    geo = dict(
        showframe = True,
        showocean = True,
        oceancolor = 'rgb(0,0,0)',
        #oceancolor = 'rgb(222,243,246)',
        projection = dict(
        type = 'orthographic',
            rotation = dict(
                    lon = 60,
                    lat = 10),
        ),
        lonaxis =  dict(
                showgrid = False,
                gridcolor = 'rgb(102, 102, 102)'
            ),
        lataxis = dict(
                showgrid = False,
                gridcolor = 'rgb(102, 102, 102)'
                )
            ),
        )
fig = dict(data=data_2, layout=layout_2)
py.iplot(fig, validate=False, filename='worldmap2010')



