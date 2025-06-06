import tkinter as tk
from tkinter import messagebox

historico_cores = []

def calcular_resultado(p1, p2, b1, b2):
    try:
        p1, p2, b1, b2 = int(p1), int(p2), int(b1), int(b2)
        soma_player = p1 + p2
        soma_banker = b1 + b2

        if soma_player > soma_banker:
            cor = 'Azul'
            codigo = 'A'
        elif soma_banker > soma_player:
            cor = 'Vermelho'
            codigo = 'V'
        else:
            cor = 'Empate'
            codigo = 'E'

        historico_cores.append(codigo)
        return soma_player, soma_banker, cor
    except ValueError:
        return None, None, "Erro: insira apenas números válidos (1 a 6)"

def analisar_tendencia():
    if not historico_cores:
        return "Sem dados suficientes para prever."

    freq = {'A': 0, 'V': 0, 'E': 0}
    for c in historico_cores:
        freq[c] += 1

    ultima_cor = historico_cores[-1]
    repeticoes = 1
    for i in range(len(historico_cores) - 2, -1, -1):
        if historico_cores[i] == ultima_cor:
            repeticoes += 1
        else:
            break

    # Regra simples:
    if repeticoes >= 3:
        # Alterna após 3 ou mais iguais
        candidatos = {'A', 'V', 'E'} - {ultima_cor}
        sugestao = max(candidatos, key=lambda x: freq[x])
    else:
        sugestao = ultima_cor

    cor_convertida = {'A': 'Azul', 'V': 'Vermelho', 'E': 'Empate'}[sugestao]

    resultado = f"""
Histórico: {' → '.join(historico_cores)}
Frequências:
Azul: {freq['A']}
Vermelho: {freq['V']}
Empate: {freq['E']}

Última cor: {ultima_cor} ({cor_convertida})
Repetições seguidas: {repeticoes}

🎯 Sugestão: Apostar em {cor_convertida.upper()}
"""
    return resultado.strip()

def processar():
    p1 = entry_p1.get()
    p2 = entry_p2.get()
    b1 = entry_b1.get()
    b2 = entry_b2.get()

    soma_p, soma_b, cor = calcular_resultado(p1, p2, b1, b2)

    if cor.startswith("Erro"):
        messagebox.showerror("Erro", cor)
        return

    resultado_text = f"Soma Player: {soma_p} | Soma Banker: {soma_b}\nResultado: {cor}\n\n"
    resultado_text += analisar_tendencia()

    label_resultado.config(text=resultado_text)

# Interface
root = tk.Tk()
root.title("Previsor Bac Bo por Dados")

tk.Label(root, text="Player - Dado 1:").grid(row=0, column=0)
entry_p1 = tk.Entry(root, width=5)
entry_p1.grid(row=0, column=1)

tk.Label(root, text="Dado 2:").grid(row=0, column=2)
entry_p2 = tk.Entry(root, width=5)
entry_p2.grid(row=0, column=3)

tk.Label(r

