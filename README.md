# Title-I
Data Dash Plotly

import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output, State
import pandas as pd
import plotly.graph_objs as go

import numpy as np


df = pd.read_csv('https://raw.githubusercontent.com/jboshkoski/Title-I/master/MAP%20and%20Independent%20Reading%20-%20Sheet1.csv?token=AO4OQIDTOU6NQJIZ6LT3DAC6TSNF4')
external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']
available_indicators = df['Grade'].unique()

def build_banner():
    return html.Div(
        id="banner",
        className="banner",
        children=[
            html.Img(src=app.get_asset_url("dash-logo.png")),
            html.H6("Oil and gas ternary map"),
        ],
    )

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
            dcc.RadioItems(
                id='xaxis-type',
                options=[{'label': i, 'value': i} for i in ['2019-2020', '2020-2021', '2021-2022']],
                value='2019-2020',
                labelStyle={'display': 'inline-block'}
            )
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
    )
])


if __name__ == '__main__':
    app.run_server(debug=True)
