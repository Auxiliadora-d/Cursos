# Cursos 
 Python

# Sistema Bancário com validação de CPF, utilizando o TKinter do python para interface


import tkinter as tk
from tkinter import messagebox

# Dicionário simulando um banco de dados de clientes
clientes = {
    "12345678900": {"nome": "Chico Bento", "saldo": 1000.0, "transacoes": [], "data_nascimento": "01/01/2000", "endereco": "Rua da Roça, 123", "contas": []},
    "98765432100": {"nome": "Magali", "saldo": 2000.0, "transacoes": [], "data_nascimento": "02/02/2002", "endereco": "Rua das Frutas, 456", "contas": []}
}

def validar_cpf(cpf):
    # Validação simples: verifica se o CPF tem 11 dígitos e é numérico.
    return len(cpf) == 11 and cpf.isdigit()

def sacar(cpf, valor):
    if cpf in clientes:
        if clientes[cpf]["saldo"] >= valor:
            clientes[cpf]["saldo"] -= valor
            clientes[cpf]["transacoes"].append(f"Saque de R${valor:.2f}")
            messagebox.showinfo("Sucesso", f"Saque de R${valor:.2f} realizado com sucesso. Novo saldo: R${clientes[cpf]['saldo']:.2f}")
        else:
            messagebox.showerror("Erro", "Saldo insuficiente.")
    else:
        messagebox.showerror("Erro", "CPF não encontrado.")

def depositar(cpf, valor):
    if cpf in clientes:
        clientes[cpf]["saldo"] += valor
        clientes[cpf]["transacoes"].append(f"Depósito de R${valor:.2f}")
        messagebox.showinfo("Sucesso", f"Depósito de R${valor:.2f} realizado com sucesso. Novo saldo: R${clientes[cpf]['saldo']:.2f}")
    else:
        messagebox.showerror("Erro", "CPF não encontrado.")

def exibir_extrato(cpf):
    if cpf in clientes:
        cliente = clientes[cpf]
        extrato = f"Cliente: {cliente['nome']}\nSaldo atual: R${cliente['saldo']:.2f}\n\nMovimentações:\n"
        extrato += "\n".join(cliente["transacoes"])
        messagebox.showinfo("Extrato", extrato)
    else:
        messagebox.showerror("Erro", "CPF não encontrado.")

def criar_usuario(cpf, nome, data_nascimento, endereco, saldo_inicial=0.0):
    if not validar_cpf(cpf):
        messagebox.showerror("Erro", "CPF inválido.")
        return

    if cpf in clientes:
        messagebox.showerror("Erro", "CPF já cadastrado.")
        return

    try:
        saldo_inicial = float(saldo_inicial)
        if saldo_inicial < 0:
            raise ValueError("O saldo inicial não pode ser negativo.")
    except ValueError as ve:
        if str(ve) == "could not convert string to float":
            messagebox.showerror("Erro", "Saldo inicial inválido: Insira um número.")
        else:
            messagebox.showerror("Erro", f"Saldo inicial inválido: {ve}")
        return

    clientes[cpf] = {"nome": nome, "saldo": saldo_inicial, "transacoes": [], "data_nascimento": data_nascimento, "endereco": endereco, "contas": []}
    messagebox.showinfo("Sucesso", f"Cliente {nome} criado com sucesso.")

def abrir_nova_janela_criacao_usuario():
    # Função para abrir uma janela independente para criação de novo usuário
    nova_janela = tk.Toplevel()
    nova_janela.title("Criar Novo Usuário")
    nova_janela.geometry("400x300")

    tk.Label(nova_janela, text="CPF:").grid(row=0, column=0, padx=10, pady=10)
    novo_cpf_entry = tk.Entry(nova_janela)
    novo_cpf_entry.grid(row=0, column=1, padx=10, pady=10)

    tk.Label(nova_janela, text="Nome:").grid(row=1, column=0, padx=10, pady=10)
    novo_nome_entry = tk.Entry(nova_janela)
    novo_nome_entry.grid(row=1, column=1, padx=10, pady=10)

    tk.Label(nova_janela, text="Data de Nascimento:").grid(row=2, column=0, padx=10, pady=10)
    nova_data_nascimento_entry = tk.Entry(nova_janela)
    nova_data_nascimento_entry.grid(row=2, column=1, padx=10, pady=10)

    tk.Label(nova_janela, text="Endereço:").grid(row=3, column=0, padx=10, pady=10)
    novo_endereco_entry = tk.Entry(nova_janela)
    novo_endereco_entry.grid(row=3, column=1, padx=10, pady=10)

    tk.Label(nova_janela, text="Saldo Inicial:").grid(row=4, column=0, padx=10, pady=10)
    novo_saldo_entry = tk.Entry(nova_janela)
    novo_saldo_entry.grid(row=4, column=1, padx=10, pady=10)

    def on_salvar_novo_usuario():
        cpf = novo_cpf_entry.get()
        nome = novo_nome_entry.get()
        data_nascimento = nova_data_nascimento_entry.get()
        endereco = novo_endereco_entry.get()
        saldo_inicial = novo_saldo_entry.get()

        if not cpf or not nome or not data_nascimento or not endereco:
            messagebox.showerror("Erro", "Todos os campos são obrigatórios.")
            return

        criar_usuario(cpf, nome, data_nascimento, endereco, saldo_inicial)
        nova_janela.destroy()

    tk.Button(nova_janela, text="Salvar", command=on_salvar_novo_usuario).grid(row=5, column=0, columnspan=2, pady=10)

def abrir_nova_janela_criacao_conta():
    # Função para abrir uma janela independente para criação de nova conta
    nova_janela = tk.Toplevel()
    nova_janela.title("Criar Nova Conta")
    nova_janela.geometry("400x200")

    tk.Label(nova_janela, text="CPF:").grid(row=0, column=0, padx=10, pady=10)
    novo_cpf_entry = tk.Entry(nova_janela)
    novo_cpf_entry.grid(row=0, column=1, padx=10, pady=10)

    tk.Label(nova_janela, text="Agência:").grid(row=1, column=0, padx=10, pady=10)
    nova_agencia_entry = tk.Entry(nova_janela)
    nova_agencia_entry.grid(row=1, column=1, padx=10, pady=10)

    tk.Label(nova_janela, text="Conta:").grid(row=2, column=0, padx=10, pady=10)
    nova_conta_entry = tk.Entry(nova_janela)
    nova_conta_entry.grid(row=2, column=1, padx=10, pady=10)

    def on_salvar_nova_conta():
        cpf = novo_cpf_entry.get()
        agencia = nova_agencia_entry.get()
        conta = nova_conta_entry.get()

        if not cpf or not agencia or not conta:
            messagebox.showerror("Erro", "Todos os campos são obrigatórios.")
            return

        if validar_cpf(cpf) and cpf in clientes:
            clientes[cpf]['contas'].append({"agencia": agencia, "conta": conta})
            messagebox.showinfo("Sucesso", f"Conta criada com sucesso para o cliente {clientes[cpf]['nome']}. Agência: {agencia}, Conta: {conta}")
            nova_janela.destroy()
        else:
            messagebox.showerror("Erro", "CPF inválido ou não cadastrado. Por favor, crie um usuário primeiro.")

    tk.Button(nova_janela, text="Salvar", command=on_salvar_nova_conta).grid(row=3, column=0, columnspan=2, pady=10)

def exibir_lista_contas():
    lista = ""
    for cpf, dados in clientes.items():
        if dados['contas']:  # Verifica se o cliente possui contas cadastradas
            lista += f"Cliente: {dados['nome']}\nCPF: {cpf}\n"
            for conta in dados['contas']:
                lista += f"Agência: {conta['agencia']}, Conta: {conta['conta']}\n"
            lista += "\n"
    messagebox.showinfo("Listas de Contas", lista)

def on_sacar_button_click():
    cpf = cpf_entry.get()
    valor_str = valor_entry.get()

    if not valor_str:
        messagebox.showerror("Erro", "O campo de valor não pode ficar vazio.")
        return

    try:
        valor = float(valor_str)
        if valor <= 0:
            raise ValueError("O valor deve ser maior que zero.")
    except ValueError as ve:
        if str(ve) == "could not convert string to float":
            messagebox.showerror("Erro", "Valor inválido: Insira um número.")
        else:
            messagebox.showerror("Erro", f"Valor inválido: {ve}")
        return

    if validar_cpf(cpf):
        try:
            sacar(cpf, valor)
            valor_entry.delete(0, tk.END)  # Limpar o campo de valor
            cpf_entry.delete(0, tk.END)    # Limpar o campo de CPF
        except Exception as e:
            messagebox.showerror("Erro", f"Falha ao realizar o saque: {e}")
    else:
        messagebox.showerror("Erro", "CPF inválido.")

def on_depositar_button_click():
    cpf = cpf_entry.get()
    valor_str = valor_deposito_entry.get()

    if not valor_str:
        messagebox.showerror("Erro", "O campo de valor não pode ficar vazio.")
        return

    try:
        valor = float(valor_str)
        if valor <= 0:
            raise ValueError("O valor deve ser maior que zero.")
    except ValueError as ve:
        if str(ve) == "could not convert string to float":
            messagebox.showerror("Erro", "Valor inválido: Insira um número.")
        else:
            messagebox.showerror("Erro", f"Valor inválido: {ve}")
        return

    if validar_cpf(cpf):
        try:
            depositar(cpf, valor)
            valor_deposito_entry.delete(0, tk.END)  # Limpar o campo de valor de depósito
            cpf_entry.delete(0, tk.END)             # Limpar o campo de CPF
        except Exception as e:
            messagebox.showerror("Erro", f"Falha ao realizar o depósito: {e}")
    else:
        messagebox.showerror("Erro", "CPF inválido.")

def on_exibir_extrato_button_click():
    cpf = cpf_entry.get()

    if validar_cpf(cpf):
        try:
            exibir_extrato(cpf)
            cpf_entry.delete(0, tk.END)  # Limpar o campo de CPF
        except Exception as e:
            messagebox.showerror("Erro", f"Falha ao exibir o extrato: {e}")
    else:
        messagebox.showerror("Erro", "CPF inválido.")

# Interface gráfica
janela = tk.Tk()
janela.title("Sistema Bancário")
janela.geometry("700x500")

# Criação de frames para melhor organização
frame_cpf = tk.Frame(janela)
frame_cpf.grid(row=0, column=0, columnspan=3, padx=10, pady=10)

cpf_label = tk.Label(frame_cpf, text="Digite o CPF:")
cpf_label.pack(side=tk.LEFT, padx=5)

cpf_entry = tk.Entry(frame_cpf)
cpf_entry.pack(side=tk.LEFT, padx=5)

# Campos para valor de saque e depósito
frame_valor = tk.Frame(janela)
frame_valor.grid(row=1, column=0, columnspan=3, padx=10, pady=10)

valor_label = tk.Label(frame_valor, text="Valor para Saque:")
valor_label.pack(side=tk.LEFT, padx=5)

valor_entry = tk.Entry(frame_valor)
valor_entry.pack(side=tk.LEFT, padx=5)

valor_deposito_label = tk.Label(frame_valor, text="Valor para Depósito:")
valor_deposito_label.pack(side=tk.LEFT, padx=5)

valor_deposito_entry = tk.Entry(frame_valor)
valor_deposito_entry.pack(side=tk.LEFT, padx=5)

# Botões de operação
frame_botoes = tk.Frame(janela)
frame_botoes.grid(row=2, column=0, columnspan=3, padx=10, pady=10)

sacar_button = tk.Button(frame_botoes, text="Sacar", command=on_sacar_button_click)
sacar_button.pack(side=tk.LEFT, padx=5)

depositar_button = tk.Button(frame_botoes, text="Depositar", command=on_depositar_button_click)
depositar_button.pack(side=tk.LEFT, padx=5)

extrato_button = tk.Button(frame_botoes, text="Exibir Extrato", command=on_exibir_extrato_button_click)
extrato_button.pack(side=tk.LEFT, padx=5)

# Botão para abrir a nova janela de criação de usuário
criar_usuario_button = tk.Button(janela, text="Criar Novo Usuário", command=abrir_nova_janela_criacao_usuario)
criar_usuario_button.grid(row=3, column=0, columnspan=3, pady=10)

# Botão para abrir a nova janela de criação de conta
criar_conta_button = tk.Button(janela, text="Criar Conta", command=abrir_nova_janela_criacao_conta)
criar_conta_button.grid(row=4, column=0, columnspan=3, pady=10)

# Botão para exibir lista de contas
lista_contas_button = tk.Button(janela, text="Listas de Contas", command=exibir_lista_contas)
lista_contas_button.grid(row=5, column=0, columnspan=3, pady=10)

# Iniciar o loop principal da interface
janela.mainloop()
