# Cursos 
 Python

# Sistema Bancário com validação de CPF, utilizando o TKinter do python para interface


import tkinter as tk
from tkinter import messagebox

# Dicionário simulando um banco de dados de clientes
clientes = {
    "12345678900": {"nome": "Chico Bento", "saldo": 1000.0, "transacoes": []},
    "98765432100": {"nome": "Magali", "saldo": 2000.0, "transacoes": []}
}

def validar_cpf(cpf):
    # Aqui você pode implementar uma lógica de validação de CPF mais robusta
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
            valor_entry.delete(0, tk.END)  # Limpar o campo de valor de saque
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
        except Exception as e:
            messagebox.showerror("Erro", f"Falha ao realizar o depósito: {e}")
    else:
        messagebox.showerror("Erro", "CPF inválido.")

def on_exibir_extrato_button_click():
    cpf = cpf_entry.get()

    if validar_cpf(cpf):
        try:
            exibir_extrato(cpf)
        except Exception as e:
            messagebox.showerror("Erro", f"Falha ao exibir o extrato: {e}")
    else:
        messagebox.showerror("Erro", "CPF inválido.")

# Interface gráfica
janela = tk.Tk()
janela.title("Sistema Bancário")
janela.geometry("500x400+700+100")

cpf_label = tk.Label(janela, text="Digite o CPF:")
cpf_label.grid(row=0, column=0, sticky='w', padx=10, pady=10)

cpf_entry = tk.Entry(janela)
cpf_entry.grid(row=0, column=1, padx=10, pady=10)

valor_label = tk.Label(janela, text="Valor para saque:")
valor_label.grid(row=1, column=0, sticky='w', padx=10, pady=10)

valor_entry = tk.Entry(janela)
valor_entry.grid(row=1, column=1, padx=10, pady=10)

sacar_button = tk.Button(janela, text="Sacar", command=on_sacar_button_click)
sacar_button.grid(row=1, column=2, padx=10, pady=10)

valor_deposito_label = tk.Label(janela, text="Valor para depósito:")
valor_deposito_label.grid(row=2, column=0, sticky='w', padx=10, pady=10)

valor_deposito_entry = tk.Entry(janela)
valor_deposito_entry.grid(row=2, column=1, padx=10, pady=10)

depositar_button = tk.Button(janela, text="Depositar", command=on_depositar_button_click)
depositar_button.grid(row=2, column=2, padx=10, pady=10)

extrato_button = tk.Button(janela, text="Exibir Extrato", command=on_exibir_extrato_button_click)
extrato_button.grid(row=3, column=0, columnspan=3, pady=10)

janela.mainloop()
