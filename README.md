DESAFIO: OTIMIZAÇÃO DE SISTEMA BANCÁRIO EM PYTHON

Este repositório apresenta uma versão aprimorada do sistema bancário básico, agora refatorado com funções Python para tornar o código mais organizado e reutilizável.

Foram criadas funções específicas para:

Depósito

Saque

Exibição de extrato

Além disso, o projeto reforça conceitos fundamentais como controle de fluxo, organização modular e boas práticas de programação.

Projeto desenvolvido como parte de um desafio de aprendizado para praticar refatoração e lógica em Python.

Sistema Bancário em Python

Este é um projeto prático desenvolvido durante o curso Back-End com Python da DIO.  
O objetivo foi criar um sistema bancário funcional no terminal, utilizando funções em Python para organizar melhor o código e facilitar a manutenção.

 Funcionalidades

- Depositar: adiciona dinheiro ao saldo da conta.  
- Sacar: retira dinheiro respeitando saldo, limite e número máximo de saques.  
- Extrato: mostra o histórico de movimentações e o saldo atual.  
- Cadastrar usuário: adiciona usuários com CPF, nome, data de nascimento e endereço.  
- Criar conta: vincula contas a usuários já cadastrados.  
- Listar contas: mostra todas as contas cadastradas.

 Tecnologias

- Python 3  
- Funções para modularizar o código  
- Interação pelo terminal  
- Conceitos de lógica e boas práticas de programação

Como executar
Clone o repositório para sua máquina local:

Bash

git clone [gh repo clone Michelleyxz/SistemaBancario.Dio]
cd sistema-bancario
Execute o arquivo principal:

Bash

python seu_arquivo.py

Código do desafio

import textwrap

def menu():
    menu = """\n
    ___________ MENU ____________
    [d]\tDepositar
    [s]\tSacar
    [e]\tExtrato
    [nc]\tNova conta
    [lc]\tListar contas
    [nu]\tNovo usuário
    [q]\tSair
    -> """
    return input(textwrap.dedent(menu))


def depositar(saldo, valor, extrato, /):
    if valor > 0:
        saldo += valor
        extrato += f"Depósito:\tR$ {valor:.2f}\n"
        print("\n==== Depósito realizado com sucesso! ====")
    else:
        print("\n@@@ Operação falhou! O valor informado é inválido. @@@")
    return saldo, extrato


def sacar(*, saldo, valor, extrato, limite, numero_saques, limite_saques):
    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_saques >= limite_saques

    if excedeu_saldo:
        print("\n@@@ Operação falhou! Você não tem saldo suficiente. @@@")

    elif excedeu_limite:
        print("\n@@@ Operação falhou! O valor do saque excede o limite. @@@")

    elif excedeu_saques:
        print("\n@@@ Operação falhou! Número máximo de saques excedido. @@@")

    elif valor > 0:
        saldo -= valor
        extrato += f"Saque:\t\tR$ {valor:.2f}\n"
        numero_saques += 1
        print("\n===== Saque realizado com sucesso! =====")

    else:
        print("\n@@@ Operação falhou! O valor informado é inválido. @@@")

    return saldo, extrato


def exibir_extrato(saldo, /, *, extrato):
    print("\n======== EXTRATO ========")
    print("Não foram realizadas movimentações." if not extrato else extrato)
    print(f"\nSaldo:\t\tR$ {saldo:.2f}")
    print("=================================")


def criar_usuario(usuarios):
    cpf = input("Informe o CPF (somente número): ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuario:
        print("\n@@@ Já existe usuário com esse CPF! @@@")
        return

    nome = input("Informe o nome completo: ")
    data_nascimento = input("Informe a data de nascimento (dd-mm-aaaa): ")
    endereco = input("Informe o endereço (logradouro, nro - bairro - cidade/sigla estado): ")

    usuarios.append({"nome": nome, "data_nascimento": data_nascimento, "cpf": cpf, "endereco": endereco})

    print("\n==== Usuário criado com sucesso! ====")


def filtrar_usuario(cpf, usuarios):
    usuarios_filtrados = [usuario for usuario in usuarios if usuario["cpf"] == cpf]
    return usuarios_filtrados[0] if usuarios_filtrados else None


def criar_conta(agencia, numero_conta, usuarios):
    cpf = input("Informe o CPF do usuário: ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuario:
        print("\n==== Conta criada com sucesso! ====")
        return {"agencia": agencia, "numero_conta": numero_conta, "usuario": usuario}

    print("\n@@@ Usuário não encontrado, fluxo de criação de conta encerrado! @@@")


def listar_contas(contas):
    for conta in contas:
        linha = f"""
            Agência:\t{conta['agencia']}
            C/C:\t\t{conta['numero_conta']}
            Titular:\t{conta['usuario']['nome']}
        """
        print("-" * 100)
        print(textwrap.dedent(linha))


def main():
    LIMITE_SAQUES = 3
    AGENCIA = "0001"

    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0
    usuarios = []
    contas = []

    while True:
        opcao = menu()

        if opcao == "d":
            valor = float(input("Informe o valor do depósito: "))
            saldo, extrato = depositar(saldo, valor, extrato)

        elif opcao == "s":
            valor = float(input("Informe o valor do saque: "))
            saldo, extrato = sacar(
                saldo=saldo,
                valor=valor,
                extrato=extrato,
                limite=limite,
                numero_saques=numero_saques,
                limite_saques=LIMITE_SAQUES,
            )

        elif opcao == "e":
            exibir_extrato(saldo, extrato=extrato)

        elif opcao == "nu":
            criar_usuario(usuarios)

        elif opcao == "nc":
            numero_conta = len(contas) + 1
            conta = criar_conta(AGENCIA, numero_conta, usuarios)
            if conta:
                contas.append(conta)

        elif opcao == "lc":
            listar_contas(contas)

        elif opcao == "q":
            break

        else:
            print("Operação inválida, por favor selecione novamente a operação desejada.")


main()

1. Clone este repositório:  https://github.com/Michelleyxz/SistemaBancario.Dio.git
