import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output, State
import pandas as pd
import plotly.graph_objs as go

import numpy as np
import plotly.express as px

df = pd.read_csv('https://raw.githubusercontent.com/jboshkoski/Title-I/master/MAP%20and%20Independent%20Reading%20-%20Sheet1.csv?token=AO4OQIDTOU6NQJIZ6LT3DAC6TSNF4')
external_stylesheets = ['https://codepen.io/amyoshino/pen/jzXypZ.css']
available_indicators = df['Grade'].unique()
fig = px.scatter(df, x="Books Read", y="Map Growth", color="Grade", title="MAP Scores and Independent Reading",
                color_continuous_scale=px.colors.sequential.Viridis, render_mode="webgl")


app = dash.Dash(__name__, external_stylesheets=external_stylesheets)
    


app = dash.Dash(
    __name__,
    meta_tags=[
        {"name": "viewport", "content": "width=device-width, initial-scale=1.0"}
    ],
)
server = app.server
app.config["suppress_callback_exceptions"] = True

app
colors = {
    'background': '#DAD9DB',
    'text': '#2C0039'
 
}

app.layout = html.Div(
    style={'backgroundColor': colors['background']}, children=[
 html.H1(
        children='Title I Average MAP Growth',
        style={
            'textAlign': 'center',
            'color': colors['text']
        }
    ),
   
 
     
                     
                     html.P('Filter by Grade'),
                       html.Div([
            dcc.Dropdown(
                id='xaxis-column',
                options=[{'label': i, 'value': i} for i in available_indicators],
                value='Grade'
            ), 
            dcc.Dropdown(
                id='dropdown_Year',
                    options=[
                        {'label': '2019-2020', 'value': '19-20'},
                        {'label': '2020-2021', 'value': '20-21'},
                        {'label': '2021-2022', 'value': '21-22'},
                        ],
                    value=['19-20'],
                    multi=True
                        ),
            
        ],
        style={'width': '48%', 'display': 'inline-block'}),
        


                  
html.Div(children='RIT Score Increase by Grade', style={
            'textAlign': 'center',
            'color': colors['text']
        }),
dcc.Graph(
        id='example-graph',
        figure={
            'data': [
                {'x': [1, 2, 3, 4, 5], 'y': [6, 12, 12, 40, 2], 'type': 'bar', 'name': '2019-2020'},
                {'x': [1, 2, 3, 4, 5], 'y': [6, 10, 20, 5, 10], 'type': 'bar', 'name': u'2020-2021'},
                {'x': [1, 2, 3, 4, 5], 'y': [2, 10, 3, 14, 6], 'type': 'bar', 'name': u'2021-2022'},
            ],
            'layout': {  
                'plot_bgcolor': colors['background'],
                'paper_bgcolor': colors['background'],
                'font': {
                    'color': colors['text']                 
                
                }
            }
        }
    ),
html.Div([ 
     
            dcc.Graph(figure=fig)
            ]),

])


if __name__ == '__main__':
    app.run_server(debug=True)
