---
title: Add Snippets
tags: 
keywords: rouge, pygments, prettify, color coding,
last_updated: Sept 5, 2022
summary: "Copy the following javascript code and save as a file .jupyter/custom/custom.js. This will automatically add a snippet menu to the jupyter notebook once opened."
sidebar: mydoc_sidebar
permalink: addsnippets.html
folder: mydoc
---


```javascript
requirejs(["nbextensions/snippets_menu/main"], function (snippets_menu) {
    console.log('Loading `snippets_menu` customizations from `custom.js`');
    snippets_menu.options['menus'] = snippets_menu.default_menus;
    var horizontal_line = '---';
    snippets_menu.options['menus'][0]['sub-menu'].push(horizontal_line);

        snippets_menu.options['menus'].push({
            'sub-menu': [{
                'snippet': [
                        'import warnings',
                        'import pandas as pd',
                        'import matplotlib.pyplot as plt',
                        'plt.style.use(\'fivethirtyeight\') ',
                        'plt.rcParams[\'figure.figsize\'] = (12,6)',
                        'import numpy as np',
                        'import os',
                        '%load_ext autoreload',
                        '%autoreload 2',
                        'warnings.filterwarnings(\'ignore\')',
                        'from IPython.core.interactiveshell import InteractiveShell',
                        'InteractiveShell.ast_node_interactivity = \'all\'',
                        'pd.set_option(\'max_columns\',1000)',
                        'pd.set_option(\'max_rows\',1000)'
                ],
                'name':
                'Imports'
            }, 
                {
                'snippet': [
                    '!jupyter --config-dir',
                    '%load C:\\Users\\212615890\\.jupyter\\custom\\custom.js',
                    '#%%writefile C:\\Users\\212615890\\.jupyter\\custom\\custom.js'
                ],
                'name':
                'Update snippets'
            }, 
                         {
                'snippet': [
                    'from pandas_profiling import ProfileReport', 'def profile(file):',
                    "    prof = ProfileReport(file, title='Pandas Profiling Report', html={'style':{'full_width':True}})",
                    '    return prof.to_widgets()'
                ],
                'name':
                'Pandas_Profile'
            }, {
                'snippet': [
                    'import tableauserverclient as TSC',
                    'server_url ="https://tableau-fdl.corporate.ge.com/"',
                    'site = "HRSite"',
                    'mytoken_name = "mytoken"',
                    'mytoken_secret = "wwMdxOslRM2xvvL3NSItTg==:m1i9g0wAtSDpBXRkpFreln2DUCi4xCo4"',
                    'server = TSC.Server(server_url, use_server_version=True)',
                    'tableau_auth = TSC.PersonalAccessTokenAuth(token_name=mytoken_name, personal_access_token=mytoken_secret, site_id=site)',
                    'with server.auth.sign_in_with_personal_access_token(tableau_auth):',
                    '    print("[Logged in successfully to {}]".format(server_url))',
                    'with server.auth.sign_in_with_personal_access_token(tableau_auth):',
                    '    all_datasources, pagination_item = server.datasources.get()',
                    '    print("\nThere are {} datasources on site: ".format(pagination_item.total_available))',
                    '    kk={datasource.name:datasource.id for datasource in all_datasources}',
                    'with server.auth.sign_in_with_personal_access_token(tableau_auth):',
                    '    server.datasources.download(kk["hr_incident_datalake"],filepath="input/hr_incident_datalake.zip")',
                    'import pantab',
                    'result = pantab.frames_from_hyper(database="input/hr_incident_datalake.hyper")',
                    'df=result[list(result.keys())[0]]',
                    'import datetime',
                    'df1=df[pd.to_datetime(df["date_created"])>datetime.datetime(2021,1,1)]',
                    '#df2=df1[df1["request_origin"]!="Mass Case"]',
                    'df1.drop_duplicates().to_pickle("input/hr_incident_datalake.pkl")'
                ],
                'name':
                'Load data from Tableau Server'
            },{
                'snippet': [
                    "def view(file):",
                    "    if not os.path.exists(\'./delete\'):",
                    "        os.mkdir(\'./delete\')",
                    "    from datetime import datetime",
                    "    now = datetime.now()",
                    "    name = \'./delete/File_\' + str(now.strftime(\"%d/%m/%Y %H:%M:%S\")).replace(",
                    "        \' \', \'_\').replace(\'/\', \'-\').replace(\':\', \'-\') + \'.xlsx\'",
                    "    file.to_excel(name, index=None)",
                    "    os.system(\"start EXCEL.EXE {}\".format(name))"
                ],
                'name':
                'View'
            }, {
                'snippet': [
                    "def clip(name=\'data\'):",
                    "    df=pd.read_clipboard(sep=\'\\t\')", "    save(df,name)",
                    "    return df"
                ],
                'name':
                'Read_Clipboard'
            },{
                'snippet': [
                    'python.exe -m pip install pystan==2.17.1.0',
                    'python.exe -m pip install fbprophet==0.6',
                    'python.exe -m pip install --upgrade fbprophet'
                ],
                'name':
                'Install Prophet'
            }, {
                'snippet': [
                    '%%writefile README.md', '#### useful links:',
                    '1. [Packages](https://github.com/bharatb964/packages)',
                    '1. [Github Families Model](https://github.build.ge.com/aviation-analytics/Material_Forecast)',
                    "1. [Peter Knight's Dataiku](https://dataiku-xplore.av.ge.com/projects/GE90_ENGINE_REMOVAL_SIMULATION/flow/)",
                    '', '#### Notes',
                    '1. Generated the counts of engines per operator by month using TPP dated 2019-09-01. ',
                    '  * Engine Series = 115B and 110B1',
                    '  * TPP WS Level = 4 - SV - Heavy',
                    '  * Customers is in the list of customers for wales and Texl'
                ],
                'name':
                'Add_Readme'
            }, {
                'sub-menu': [{
                    'snippet': [
                        '<div class="alert alert-block alert-info">',
                        '<b>Tip:</b> Use blue boxes (alert-info) for tips and notes. ',
                        'If its a note, you dont have to include the word ?Note?.',
                        '</div>', ''
                    ],
                    'name':
                    'Blue'
                }, {
                    'snippet': [
                        '<div class="alert alert-block alert-warning">',
                        '<b>Example:</b> Yellow Boxes are generally used to include additional examples or mathematical formulas.',
                        '</div>', ''
                    ],
                    'name':
                    'Yellow'
                }, {
                    'snippet': [
                        '<div class="alert alert-block alert-success">',
                        'Use green box only when necessary like to display links to related content.',
                        '</div>', ''
                    ],
                    'name':
                    'Green'
                }, {
                    'snippet': [
                        '<div class="alert alert-block alert-danger">',
                        'It is good to avoid red boxes but can be used to alert users to not delete some important part of code etc. ',
                        '</div>', ''
                    ],
                    'name':
                    'Red'
                }],
                'name':
                'Boxes'
            }, {
                'snippet': [
                    '!setx HTTP_PROXY https://http-proxy.geps.ge.com:80',
                    '!setx HTTPS_PROXY https://http-proxy.geps.ge.com:80'
                ],
                'name':
                'Set_Proxy'
            }, {
                'snippet': [
                    'pip install jupyter_contrib_nbextensions && jupyter contrib nbextension install '
                ],
                'name':
                'Install_nbextensions'
            },
				{
					'snippet':[
						"import os",
						"import shutil  ",
						"import webbrowser",
						"from datetime import date",
						"today = date.today()",
						"def run_tensorboard(run_name,move_downloaded=False):",
						"    if move_downloaded:",
						"        paths=r\"C:\\Users\\212615890\\Downloads\"",
						"        file=max([os.path.join(paths, basename) for basename in os.listdir(paths)], key = os.path.getctime)",
						"        run_folder=r\"C:\\Users\\212615890\\tesorboard_runs\\\\\"+run_name+\"_\"+str(today)",
						"        if not os.path.exists(run_folder):",
						"            os.mkdir(run_folder)",
						"        shutil.move(file,run_folder)",
						"    !start tensorboard --logdir=\"C:/Users/212615890/tesorboard_runs\" --port 8008 && start http://localhost:8008/",
						"    !start C:\\Users\\212615890\\tesorboard_runs",
						"",
						"run_tensorboard(\'Kaggle_run_pytorch\')"
					],
				'name':'Add TensorBoard'}

            ],
            'name':
            'Favourites'
        })
/* ######################################################################################################## */
        snippets_menu.options['menus'].push({
            'sub-menu': [{
                'snippet': [
                    "proxyDict = { ",
                    "\"http\"  : \"http://http-proxy.geps.ge.com:80\", ",
                    "\"https\" : \"http://https-proxy.geps.ge.com:80\", ", "}",
                    "import requests ",
                    "with open(r\'C:\\Users\\212615890\\.jupyter\\custom\\custom.js\',\'wb\') as t:",
                    "    t.write(requests.get(\"https://raw.githubusercontent.com/bharatb964/packages/master/custom.js\",proxies=proxyDict).content)"
                ],
                'name':
                'Download Snippets'
            }, {
                'external-link':
                'https://github.com/bharatb964/packages/blob/master/custom.js',
                'name': 'Snippet File'
            }, {
                'external-link': 'https://github.com/bharatb964/packages',
                'name': 'Package_Github'
            }, {
                'external-link': 'https://pytorch.org/resources/',
                'name': 'Pytorch'
            }, {
                'external-link': 'https://www.kaggle.com/bharatb964/',
                'name': 'Kaggle_Notebooks'
            }],
            'name':
            'Bookmarks'
        })

        snippets_menu.options['menus'].push({
            'sub-menu': [{
                'snippet': [
                    "import cufflinks as cf",
                    "df = df.groupby(\'\')[\'\', \'\'].sum()", "",
                    "df.iplot(sharing=\'public\', theme=\'pearl\',filename=\'plots\',asFigure=True,# asPlot=True,",
                    "    kind=\'scatter\', x=[\'\'],y=[\'\'], # bar, box, histogram",
                    "    xTitle=\'x_label\', yTitle=\'y_label\', title=\'Title\', ",
                    "    mode=\'markers+lines\', size=8, width=2, #symbol=\'x\',",
                    "    #barmode=\'stack\',sortbars=True,  ",
                    "    #colors=[\'orange\', \'blue\'],  # green, pink,\'magenta\',\'grey\'",
                    "    #dimensions=('width','height'),subplots=True,shape=(2,1), subplot_titles=True, legend=False,    ",
                    ")",
                    "#cf.iplot(cf.subplots([fig1,fig2],shape=(2,1),shared_xaxes=True,shared_yaxes=False,))"
                ],
                'name':
                'Plots'
            }, {
                'snippet': [
                    'from wordcloud import WordCloud',
                    'def wordcloud(word_list,figsize=(12,12)):',
                    "    text=' '.join(word_list)",
                    '    wordcloud=WordCloud().generate(text)',
                    '    plt.figure(figsize=figsize)',
                    "    plt.imshow(wordcloud,interpolation='bilinear')",
                    "    plt.axis('off')", '    plt.show()'
                ],
                'name':
                'WordCloud'
            }, {
                'snippet': [
                    'def annotate(x,multiple_axes=False):', '    splot=x',
                    '    if multiple_axes==True:',
                    '        for i in range(splot.axes.shape[0]):',
                    '            for j in range(splot.axes.shape[1]):',
                    '                for p in splot.axes[i][j].patches:',
                    "                    splot.axes[i][j].annotate(format(p.get_height(), '.2f'), (p.get_x() + p.get_width() / 2., p.get_height()),",
                    "                               ha = 'center', va = 'center', xytext = (0, 10), textcoords = 'offset points')",
                    '    else:', '        for p in splot.axes.patches:',
                    "            splot.axes.annotate(format(p.get_height(), '.2f'), (p.get_x() + p.get_width() / 2., p.get_height()),",
                    "            ha = 'center', va = 'center', xytext = (0, 10), textcoords = 'offset points')",
                    ''
                ],
                'name':
                'Annotate'
            }, {
                'snippet': [
                    "import plotly.express as px", "df = px.data.tips()",
                    "px.scatter(df,", "           x=\"total_bill\",",
                    "           y=\"tip\",", "           facet_row=\"time\",",
                    "           facet_col=\"day\",", "           color=\"smoker\",",
                    "           trendline=\"ols\")"
                ],
                'name':
                'Pie_Chart'
            }],
            'name':
            'Plots'
        })

        });
```
