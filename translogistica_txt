import gspread
import pandas as pd
from tabulate import tabulate
from datetime import datetime
import time

from google.auth import default
from google.colab import auth

from tabulate import tabulate
from google.colab import output

auth.authenticate_user()
creds, _ = default()

usuario = gspread.authorize(creds)

planilha = usuario.open('login_translogistica')
worksheet = planilha.get_worksheet(0) #VIAGENS
worksheet1 = planilha.get_worksheet(1) #VALORES
worksheet3 = planilha.get_worksheet(2) #VEÍCULOS
worksheet5 = planilha.get_worksheet(3) #CADASTROS
worksheet2 = planilha.get_worksheet(4) #CONFIGURAÇÕES
worksheet6 = planilha.get_worksheet(5) #GERENTE

def cadastro():
  print("-----------------|CADASTRO DE MOTORISTA|-----------------")
  usuario_cad = input("INFORME O NOME DO USUÁRIO: ")
  senha_cad = input("INFORME A SENHA DO USUÁRIO: ")
  nome = input("INFORME O NOME DO MOTORISTA: ")
  cnh = input("INFORME A CNH: ")
  tel = input("INFORME O TELEFONE: ")
  output.clear()
  print(
    "DESEJA CONCLUIR O CADASTRO?"
    "\n[1] SIM"
    "\n[2] NÃO"
)
  cad_opcao = input("OPÇÃO: ")
  output.clear()
  print(
      "CADASTRANDO PERFIL DE MOTORISTA..."
      "\n"
  )
  time.sleep(3)
  output.clear()

  if cad_opcao == "1":
    stts = "DISPONÍVEL"
    nova_linha = [usuario_cad, senha_cad, nome, cnh, tel, stts]
    worksheet5.append_row(nova_linha)
    print("!CADASTRO REALIZADO COM SUCESSO!")
    time.sleep(3)
    output.clear()

  if cad_opcao == "2":
    print("!CADASTRO CANCELADO!")
    output.clear()

def listagem_cadastro_disponivel():
  cadastro_disponivel = worksheet5.get_all_values()
  filtro_cad_disponivel =[linha for linha in cadastro_disponivel if linha[5] == "DISPONÍVEL"]
  tabela_cad_disponivel = tabulate(filtro_cad_disponivel, tablefmt="rounded_grid", stralign="center")
  print(tabela_cad_disponivel)

def listagem_cadastro_em_viagem():
  cadastro_viagem = worksheet5.get_all_values()
  filtro_cad_viagem =[linha for linha in cadastro_viagem if linha[5] == "EM VIAGEM"]
  tabela_cad_viagem = tabulate(filtro_cad_viagem, tablefmt="rounded_grid", stralign="center")
  print(tabela_cad_viagem)

def listagem_veiculos_disponiveis():
  veiculo_disponivel = worksheet3.get_all_values()
  filtro_veic_disponivel =[linha for linha in veiculo_disponivel if linha[3] == "DISPONÍVEL"]
  tabela_veic_disponivel = tabulate(filtro_veic_disponivel, tablefmt="rounded_grid", stralign="center")
  print(tabela_veic_disponivel)

def listagem_veiculos_em_viagem():
  em_viagem = worksheet3.get_all_values()
  filtro_em_viagem =[linha for linha in em_viagem if linha[3] == "EM VIAGEM"]
  tabela_em_viagem = tabulate(filtro_em_viagem, tablefmt="rounded_grid", stralign="center")
  print(tabela_em_viagem)

def todas_as_viagens():
  dados = worksheet.get_all_values()
  tabela_formatada = tabulate(dados,  tablefmt="rounded_grid", stralign="center")
  print(tabela_formatada)

def listagem():
  print("-----------------|LISTAGEM|-----------------")
  print("LISTAGENS DISPONÍVEIS:"
          "\n[1] - CADASTRO"
          "\n[2] - VEÍCULOS"
          "\n[3] - VIAGENS"
  )
  op_listagem = input("INFORME A LISTAGEM QUE DESEJA VER -> ")
  output.clear()

  if op_listagem == "1":
    print("            LISTAGEM DE CADASTROS            ")
    print(
        "1) CADASTROS DISPONÍVEIS"
        "\n2) CADASTROS EM VIAGEM"
    )

    op_cad = input("QUAL CADASTRO DESEJA OLHAR? ")
    output.clear()
    if op_cad == "1":
      listagem_cadastro_disponivel()
    if op_cad == "2":
      listagem_cadastro_em_viagem()

  if op_listagem == "2":
    print("            LISTAGEM DE VEÍCULOS            ")
    print(
        "1) VEÍCULOS DISPONÍVEIS"
        "\n2) VEÍCULOS EM VIAGEM"
    )
    op_veic = input("QUAL VIAGEM DESEJA OLHAR? ")
    output.clear()
    if op_veic == "1":
      listagem_veiculos_disponiveis()
    if op_veic == "2":
      listagem_veiculos_em_viagem()

  if op_listagem == "3":
    output.clear()
    print("            TODAS AS VIAGENS            ")
    todas_as_viagens()

def inicial():
  print("-----------------|PARTIDA DE VIAGEM|-----------------")
  destino = input("INFORME O DESTINO: ")
  saida = input("INFORME O HORÁRIO DE SAÍDA: ")
  data = input("INFORME A DATA: ")
  output.clear()
  print("VEJA ABAIXO UMA LISTA DOS VEÍCULOS DISPONÍVEIS")
  listagem_veiculos_disponiveis()
  placa = input("INFORME A PLACA DO VEÍCULO: ")
  od_inicial = input("INFORME O ODOMÊTRO DO VEÍCULO: ")
  output.clear()
  celula = worksheet3.find(placa)

  if celula:
    linha = celula.row
    coluna = celula.col
    tp_veiculo = worksheet3.cell(linha, coluna - 1).value
    comb = worksheet3.cell(linha, coluna + 1).value
    codigo = int(worksheet2.cell(2, 1).value) + 1
    worksheet2.update_cell(2, 1, codigo)
    worksheet3.update_cell(linha, coluna + 2, "EM VIAGEM")

  print("|VEJA A LISTA DE MOTORISTAS DISPONÍVEIS|")
  listagem_cadastro_disponivel()
  cnh = input("INFORME A CNH DO MOTORISTA RESPONSÁVEL PELA VIAGEM: ")
  output.clear()
  cell = worksheet5.find(cnh)

  if cell:
    linha_cell = cell.row
    coluna_cell = cell.col
    nome = worksheet5.cell(linha_cell, coluna_cell + -1).value
    worksheet5.update_cell(linha_cell, coluna_cell + 2, "EM VIAGEM")
    nova_linha = [codigo, nome, placa, tp_veiculo, comb, destino, saida, data, od_inicial]
    worksheet.append_row(nova_linha)
    print("INICIANDO VIAGEM...")
    time.sleep(4)
    print("!VIAGEM INICIADA!")
    time.sleep(3)
    output.clear()

def final():
  print("-----------------|CONCLUSÃO DE VIAGEM|-----------------")
  codigo = input("INFORME O CÓDIGO DA VIAGEM: ")
  celula = worksheet.find(codigo)
  if celula:
    linha = celula.row
    coluna = celula.col
    od_final = float(input("DIGITE O VALOR FINAL DO ODOMETRO: "))
    od_inicial = float(worksheet.cell(linha, coluna + 8).value)
    km = od_final - od_inicial
    worksheet.update_cell(linha, coluna + 9 , od_final)
    worksheet.update_cell(linha, coluna + 10 , km)

    if worksheet.cell(linha, coluna + 4).value == "Diesel":
      v_comb = float(worksheet1.cell(2, 2).value)
    elif worksheet.cell(linha, coluna + 4).value == "Gasolina":
      v_comb = float(worksheet1.cell(3, 2).value)
    elif worksheet.cell(linha, coluna + 4).value == "Alcool":
      v_comb = float(worksheet1.cell(4, 2).value)
    valor = float(km * v_comb)
    valor_total = "{:.2f}".format(valor).replace(',', '.')
    worksheet.update_cell(linha,coluna + 11, valor_total) #funcionando

    h_chegada = input("INFORME A HORA DA CHEGADA: ")
    output.clear()
    horaChegadaFinal = h_chegada.split(":")
    horasChegadaFinal = horaChegadaFinal[0]
    minutosChegadaFinal = horaChegadaFinal[1]

    h_saida = worksheet.cell(linha, coluna + 6).value
    horaSaidaFinal = h_saida.split(":")
    horasSaidaFinal = horaSaidaFinal[0]
    minutosSaidaFinal = horaSaidaFinal[1]

    horasFinal = int(horasChegadaFinal) - int(horasSaidaFinal)
    if "-" in str(horasFinal):
      horasFinal = horasFinal * -1
    if len(str(horasFinal)) == 1:
      horasFinal = "0" + str(horasFinal)

    minutosFinal = int(minutosChegadaFinal) - int(minutosSaidaFinal)
    if "-" in str(minutosFinal):
      minutosFinal = minutosFinal * -1
    if len(str(minutosFinal)) == 1:
      minutosFinal = "0" + str(minutosFinal)

    total = str(horasFinal) + ":" + str(minutosFinal)
    worksheet.update_cell(linha, coluna + 13, total)
    worksheet.update_cell(linha, coluna + 12, h_chegada)
    print("FINALIZANDO VIAGEM...")
    time.sleep(3)
    print("VIAGEM FINALIZADA")
    output.clear()

def excluir():
  print("-----------------|EXCLUSÃO DE VIAGEM|-----------------")
  print("\nEXCLUSÕES DISPONÍVEIS:")
  print(
      "[1] EXCLUIR VIAGEM"
      "\n[2] EXCLUIR CADASTRO"
  )
  op_exclusao = input("INFORME A OPÇÃO QUE DESEJA EXCLUIR -> ")
  output.clear()

  if op_exclusao == "1":
    print("           EXCLUIR VIAGEM           ")
    cod = input("INFORME O CÓDIGO DA VIAGEM A SER EXCLUÍDO: ")
    output.clear()
    celula = worksheet.find(cod)
    worksheet.delete_row(celula.row)
    print("EXCLUINDO VIAGEM...")
    time.sleep(4)
    output.clear()

  if op_exclusao == "2":
    print("           EXCLUIR CADASTRO           ")
    user_ex = input("INFORME O USUÁRIO A SER EXCLUÍDO: ")
    output.clear()
    celula = worksheet5.find(user_ex)
    worksheet5.delete_row(celula.row)
    print("EXCLUINDO CADASTRO...")
    time.sleep(4)
    output.clear()

def login():
  while True:
    print("              ==========================="
        "\n              |                         |              "
        "\n              |SISTEMA DE TRANSLOGÍSTICA|              "
        "\n              |                         |              "
        "\n              ==========================="
        "\n"
    )
    print(
    "FUNÇÃO:"
    "\n[1] GERENTE"
    "\n[2] MOTORISTA"
    "\n[3] SAIR DO SISTEMA"
  )
    func = input("INFORME A SUA A FUNÇÃO -> ")

    if func == "1":
      output.clear()
      login_gerente()

    if func == "2":
      output.clear()
      print("MOTORISTA")
      todas_as_viagens()

    if func == "3":
      output.clear()
      print("SAINDO DO SISTEMA...")
      time.sleep(5)
      print("FINALIZADO!")
      break

def login_gerente():
  while True:
    print("            LOGIN DE GERENTE           ")
    user = input("\nINFORME O NOME DE USUÁRIO: ")
    output.clear()

    try:
      celula = worksheet6.find(user)

      if celula:
        linha = celula.row
        senha = input("INFORME A SENHA:")
        senha_confirma = worksheet6.cell(linha, 2).value
        output.clear()

        while senha != senha_confirma:
          print("SENHA INVÁLIDA, TENTE NOVAMENTE!")
          senha = input("INFORME A SENHA:")
          output.clear()

        else:
          print("LOGANDO GERENTE...")
          time.sleep(4)
          output.clear()
          menu()
          break

    except gspread.CellNotFound:
      print("USUÁRIO INVÁLIDO, POR FAVOR TENTE NOVAMENTE!")
      print("")

def login_motorista():
  while True:
    print("            LOGIN DE MOTORISTA            ")
    user_moto = input("INFORME O NOME DE USUÁRIO: ")
    output.clear()

    try:
      celula = worksheet5.find(user_moto)

      if celula:
        linha = celula.row
        senha_moto = input("INFORME A SENHA:")
        senha_moto_confirma = worksheet5.cell(linha, 2).value
        output.clear()

        while senha_moto != senha_moto_confirma:
          print("SENHA INVÁLIDA, TENTE NOVAMENTE!")
          senha_moto = input("INFORME A SENHA: ")
          output.clear()

        else:
          print("LOGANDO MOTORISTA...")
          time.sleep(4)
          output.clear()
          menu2()
          break

    except gspread.CellNotFound:
      print("USUÁRIO INVÁLIDO, POR FAVOR TENTE NOVAMENTE!")
      output.clear()

def menu():
  while True:
    print("========================!BEM VINDO AO MENU DE GERENTES!========================")
    print("\n|OPÇÕES DO GERENTE|")
    print(
        "[1] CADASTRO"
        "\n[2] LISTAGEM"
        "\n[3] INICIAR VIAGEM"
        "\n[4] CONCLUIR VIAGEM"
        "\n[5] MENU DE EXCLUSÃO"
        "\n[0] SAIR DO MENU"
    )
    menu_gerente = input("INFORME SUA OPÇÃO -> ")
    output.clear()

    if menu_gerente == "1":
      cadastro()
    if menu_gerente == "2":
      listagem()
    if menu_gerente == "3":
      inicial()
    if menu_gerente == "4":
      final()
    if menu_gerente == "5":
      excluir()
    if menu_gerente == "0":
      break

def menu2():
  while True:
    print("========================!BEM VINDO AO MENU DE MOTORISTAS!========================")
    print("\n|OPÇÕES DO MOTORISTA|")
    print(
        "[1] VIZUALIZAR VIAGENS"
        "\n[2] SAIR DO MENU"
    )

    menu_motorista = input("INFORME A SUA OPÇÃO -> ")
    output.clear()

    if menu_motorista == "1":
      codigo = input("INFORME O CÓDIGO DA SUA VIAGEM: ")
      celula = worksheet.find(codigo)

      if celula:
        viagem = worksheet.get_all_values()
        linha = celula.row
        filtro = [linha for linha in viagem if linha[0] == codigo]
        tabela_formatada = tabulate(filtro,  tablefmt="rounded_grid", stralign="center")
        print(tabela_formatada)

    if menu_motorista == "2":
      print("SAINDO DO MENU...")
      time.sleep(3)
      output.clear()
      break
