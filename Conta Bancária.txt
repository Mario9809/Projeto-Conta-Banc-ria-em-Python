from datetime import datetime, timedelta
import time

def salvar_dados_clientes():
  for cliente in LISTA_CLIENTES:
      caminho_arquivo = f'dados_cliente_{cliente["CNPJ"]}.txt'
      with open(caminho_arquivo, 'w') as arquivo:
          arquivo.write(str(cliente))
   

def salvar_extrato_cliente(cliente):
  caminho_arquivo = f'extrato_{cliente["CNPJ"]}.txt'
  try:
    with open(caminho_arquivo, 'r') as arquivo_leitura:
        extrato_anterior = arquivo_leitura.readlines()
  except FileNotFoundError:
      extrato_anterior = []

  with open(caminho_arquivo, 'w') as arquivo:
      for linha in extrato_anterior:
        arquivo.write(linha)

      arquivo.write(str(cliente["Transacoes"]) + '\n')


    



LISTA_CLIENTES = []


def novo_cliente():
  Razao_social = input("Digite sua razão Social: ")
  CNPJ = int(input("Digite seu CNPJ: "))
  Tipo_de_conta = input("Digite seu tipo de conta (Comum ou Plus): ")
  Valor_inicial_da_conta = float(input("Digite o valor inicial da conta: "))
  Senha_do_usuario = input("Digite sua Senha: ")
  dicionario = {
      "RazaoSocial": Razao_social,
      "CNPJ": CNPJ,
      "TipoDeConta": Tipo_de_conta,
      "ValorInicialDaConta": Valor_inicial_da_conta,
      "SenhaDoUsuario": Senha_do_usuario,
      "Transacoes": []
  }
  LISTA_CLIENTES.append(dicionario)
  

def Apaga_cliente():
  CNPJdel = int(input("Digite o CNPJ do cliente a ser removido: "))
  for cliente in LISTA_CLIENTES:
    if cliente['CNPJ'] == CNPJdel:
      LISTA_CLIENTES.remove(cliente)
      print("Cliente removido com sucesso.")
      break
  else:
    print("CNPJ não encontrado.")


def Listar_clientes():
  print("LISTA DE CLIENTES:")
  for cliente in LISTA_CLIENTES:
    print(cliente)
  print()


def Debitar():
  CNPJ = int(input("Digite seu CNPJ: "))
  Senha_do_usuario = input("Digite sua Senha: ")
  valor = float(input("Digite o valor a ser debitado: "))
  for conta in LISTA_CLIENTES:
    if conta["CNPJ"] == CNPJ and conta["SenhaDoUsuario"] == Senha_do_usuario:
      if conta['TipoDeConta'] == 'Comum':
        tarifa = valor * 0.05
        limite = 1000
      else:
        tarifa = valor * 0.03
        limite = 5000
      if conta["ValorInicialDaConta"] + limite >= valor + valor*tarifa:
        conta["ValorInicialDaConta"] -= valor - tarifa
        conta["Transacoes"].append({
            "Data":
            datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
            "Valor da Transacao":
            valor,
            "Tarifa":
            tarifa,
            "Saldo atual":
            conta["ValorInicialDaConta"]
        })
        print(f"Débito de R$ {valor} Realizado")
      else:
        print("Saldo insuficiente para realizar o débito")
      break
  else:
    print("CNPJ não encontrado ou senha inválida")


def Deposito():
  CNPJ = int(input("Digite seu CNPJ: "))
  deposito = float(input("Digite o valor a ser depositado: "))

  for conta in LISTA_CLIENTES:
    if conta["CNPJ"] == CNPJ:
      conta["ValorInicialDaConta"] += deposito
      conta["Transacoes"].append({
          "Data":
          datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
          "Valor da Transacao":
          deposito,
          "Tarifa":
          0,
          "Saldo atual":
          conta["ValorInicialDaConta"]
      })
      print(f"Depósito de R$ {deposito} Realizado")

    else:
      print("CNPJ inválido")
    break


def extrato():
  CNPJ = int(input("Digite seu CNPJ: "))
  Senha_do_usuario = input("Digite sua Senha: ")

  for conta in LISTA_CLIENTES:
      if conta["CNPJ"] == CNPJ and conta["SenhaDoUsuario"] == Senha_do_usuario:
          print(f"Razão Social: {conta['RazaoSocial']}")
          print(f"CNPJ: {conta['CNPJ']}")
          print(f"Tipo de Conta: {conta['TipoDeConta']}")
          print("Histórico de Operações:")
          for transacao in conta["Transacoes"]:
              print(
                  f"Data: {transacao['Data']} - Valor da Transação: R$ {transacao['Valor da Transacao']} - Tarifa: R$ {transacao['Tarifa']} - Saldo Atual: R$ {transacao['Saldo atual']}"
              )
          salvar_extrato_cliente(conta)  
          break
  else:
      print("CNPJ ou senha incorretos")


def transferencia():
  CNPJ_origem = int(input("Digite o CNPJ da conta de origem: "))
  Senha_do_usuario = input("Digite sua Senha: ")
  CNPJ_destino = int(input("Digite o CNPJ da conta de destino: "))
  valor = float(input("Digite o valor a ser transferido: "))

  conta_origem = None
  conta_destino = None

  for cliente in LISTA_CLIENTES:
    if cliente["CNPJ"] == CNPJ_origem and cliente[
        "SenhaDoUsuario"] == Senha_do_usuario:
      if cliente['TipoDeConta'] == 'Comum':
        tarifa = valor * 0.05
      else:
        tarifa = valor * 0.03
      conta_origem = cliente
    elif cliente["CNPJ"] == CNPJ_destino:
      conta_destino = cliente

  if conta_origem is not None and conta_destino is not None and conta_origem[
      "ValorInicialDaConta"] >= valor:
    conta_origem["ValorInicialDaConta"] -= valor - tarifa
    conta_destino["ValorInicialDaConta"] += valor
    conta_origem["Transacoes"].append({
        "Data":
        datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
        "Valor da Transacao":
        valor,
        "Tarifa":
        tarifa,
        "Saldo atual":
        conta_origem["ValorInicialDaConta"]
    })
    conta_destino["Transacoes"].append({
        "Data":
        datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
        "Valor da Transacao":
        valor,
        "Tarifa":
        tarifa,
        "Saldo atual":
        conta_destino["ValorInicialDaConta"]
    })
    print(f"Transferência de R$ {valor} realizada com sucesso.")
  elif conta_origem is None or conta_destino is None:
    print("Conta de origem ou conta de destino não encontrada.")
  else:
    print("Saldo insuficiente para realizar a transferência.")


def TED_automatico():
    def realizar_transferencia(valor, tarifa, conta_origem, conta_destino):
        horario_atual = datetime.now()
        conta_origem["ValorInicialDaConta"] -= valor - tarifa
        conta_destino["ValorInicialDaConta"] += valor
        conta_origem["Transacoes"].append({
            "Data":
            horario_atual.strftime("%Y-%m-%d %H:%M:%S"),
            "Valor da Transacao":
            valor,
            "Tarifa":
            tarifa,
            "Saldo atual":
            conta_origem["ValorInicialDaConta"]
        })
        
        print(f"Transferência realizada em {horario_atual}")

    horario_debito = datetime.now().replace(hour=22, minute=34, second=0, microsecond=0)

    CNPJ_origem = int(input("Digite o CNPJ da conta de origem: "))
    Senha_do_usuario = input("Digite sua Senha: ")
    CNPJ_destino = int(input("Digite o CNPJ da conta de destino: "))
    valor = float(input("Digite o valor a ser transferido: "))

    conta_origem = None
    conta_destino = None

    for cliente in LISTA_CLIENTES:
        if cliente["CNPJ"] == CNPJ_origem and cliente["SenhaDoUsuario"] == Senha_do_usuario:
            if cliente['TipoDeConta'] == 'Comum':
                tarifa = valor * 0.05
                limite = 1000
            else:
                tarifa = valor * 0.03
                limite = 5000
            conta_origem = cliente
        elif cliente["CNPJ"] == CNPJ_destino:
            conta_destino = cliente

    if conta_origem is not None and conta_destino is not None and conta_origem["ValorInicialDaConta"] - limite >= valor + valor*tarifa:
        horario_atual = datetime.now()

        if horario_atual < horario_debito:
            horario_agendado = horario_debito
            conta_origem["ValorInicialDaConta"] -= valor - tarifa
            conta_destino["ValorInicialDaConta"] += valor
            conta_origem["Transacoes"].append({
                "Data":
                horario_agendado.strftime("%Y-%m-%d %H:%M:%S"),
                "Valor da Transacao":
                valor,
                "Tarifa":
                tarifa,
                "Saldo atual":
                conta_origem["ValorInicialDaConta"]
            })

            print(f"Transferência agendada para {horario_agendado}")
            while datetime.now() < horario_agendado:
                time.sleep(30)

            realizar_transferencia(valor, tarifa, conta_origem, conta_destino)
        else:
            realizar_transferencia(valor, tarifa, conta_origem, conta_destino)

    else:
        print("Conta de origem, conta de destino não encontradas ou saldo insuficiente.")


while True:
  print(
      "1. Novo cliente 2. Apaga cliente 3. Listar clientes 4. Débito 5. Depósito 6. Extrato 7. Transferência entre contas 8. TED Automático 9. Sair"
  )
  Escolha = int(input("Digite o número da opção selecionada: "))
  if Escolha == 1:
    novo_cliente()
    salvar_dados_clientes()

  elif Escolha == 2:
    Apaga_cliente()

  elif Escolha == 3:
    Listar_clientes()

  elif Escolha == 4:

    Debitar()

  elif Escolha == 5:

    Deposito()

  elif Escolha == 6:

    extrato()
    

  elif Escolha == 7:

    transferencia()

  elif Escolha == 8:

    TED_automatico()

  elif Escolha == 9:

    break

print(LISTA_CLIENTES)
