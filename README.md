#  API for Google Sheets
O código foi feito no Google Colab, com a biblioteca gspread importada, para vincular o código ao Google Sheet ( Google Planilha)
* Documentação do gspread: https://docs.gspread.org/en/v6.0.0/
<br>
É necessário criar uma planilha no Google Sheet, de acordo com os nomes que estão no código

<br>

# COMANDOS INICIAIS
import gspread //_Importa a biblioteca_ <br>
import pandas as pd //_Importa o Pandas_ <br>
from tabulate import tabulate //_Importa Tabela_ <br>
from datetime import datetime <br>
from google.colab import output //_Limpa Tela_ <br> 
import time <br> //_Tempo de Tela_

//**Conexão com o Colab e Autenticação** <br>
from google.auth import default <br>
from google.colab import auth <br>

//**Autenticação de usuário** <br>
auth.authenticate_user() <br>
creds, _ = default() <br>

//**Autorização de Usuário** <br>
usuario = gspread.authorize(creds) <br>

//**Conexão das Planilhas** <br>
planilha = usuario.open('login_translogistica') <br>
worksheet = planilha.get_worksheet(0) #VIAGENS <br>
worksheet1 = planilha.get_worksheet(1) #VALORES <br> 
worksheet3 = planilha.get_worksheet(2) #VEÍCULOS <br>
worksheet5 = planilha.get_worksheet(3) #CADASTROS <br> 
worksheet2 = planilha.get_worksheet(4) #CONFIGURAÇÕES <br>
worksheet6 = planilha.get_worksheet(5) #GERENTE

