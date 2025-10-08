# **Projeto Sorte no Azar**

Este é um simples jogo de apostas via terminal onde o jogador pode depositar um saldo, apostar em cores e tentar a sorte. O jogo conta com uma lógica de proteção para a "casa" e usa a IA do Google (Gemini) para gerar frases engraçadas quando o jogador perde.

## **Funcionalidades**

* **Sistema de Depósito e Saldo:** O jogador começa fazendo um depósito para ter um saldo para apostar.  
* **Apostas em Três Cores:** É possível apostar em PRETO, VERMELHO ou BRANCO, cada um com seu multiplicador de prêmio.  
* **Sorteio com Pesos:** A cor BRANCO é mais rara de ser sorteada, mas oferece um prêmio muito maior (14x).  
* **Lógica Secreta da "Caixa da Casa":** O jogador só pode ganhar um prêmio se a casa já tiver acumulado um lucro de R$ 10,00. Caso contrário, o sorteio é secretamente manipulado para garantir a derrota do jogador.  
* **Pausa para Suspense:** Uma espera de 5 segundos após a aposta para criar um suspense antes de revelar o resultado.  
* **Frases Geradas por IA:** Ao perder, o jogador recebe uma frase de "consolo" gerada pelo modelo Gemini do Google.

## **Como Executar**

**1\. Pré-requisitos:**

* Ter o Python 3 instalado no seu computador.

2\. Instalar as Bibliotecas:

Abra o seu terminal e execute o comando:

Bash

pip install google-generativeai

3\. Configurar a Chave da API:

Este código foi feito para rodar no Google Colab, que usa userdata.get(). Se você for rodar localmente, precisa da sua chave da API do Google AI Studio.

Abra a função gerador\_de\_frase() e substitua a parte da chave:

Python

\# Mude DE:  
from google.colab import userdata  
GOOGLE\_API\_KEY \= userdata.get('GOOGLE\_API\_KEY')

\# PARA:  
GOOGLE\_API\_KEY \= "SUA\_CHAVE\_API\_AQUI" 

4\. Rodar o Jogo:

Salve o código em um arquivo (ex: jogo.py) e execute no terminal:

Bash

python jogo.py

## **Entendendo o Código**

O programa é dividido em várias funções pequenas, cada uma com uma responsabilidade.

### **main()**

**O que faz?** É a função que inicia tudo. Ela só mostra a mensagem de boas-vindas e chama a função jogo().

Python

def main():  
    print(' \#\#\#\#\#\#\#\#\#\# BEM VINDO AO SORTE NO AZAR \#\#\#\#\#\#\#\#\#\# ')  
    print('--------------------------------------------------')  
    jogo()

### **realizar\_deposito()**

**O que faz?** Pergunta ao usuário se ele quer depositar. Se sim, pega um valor válido e retorna o saldo. Se não, ou se der erro, retorna 0\.

Python

def realizar\_deposito():  
    resposta \= input('\\nVamos fazer um deposito? (S/N) ')  
    if (resposta \== 's') or (resposta \== 'S'):  
        \# ... lógica para pegar o valor ...  
        return saldo  
    \# ... resto da lógica ...  
    return 0

### **pegar\_aposta(saldo\_disponivel)**

**O que faz?** Pede o valor da aposta e verifica se o jogador tem saldo suficiente. Retorna o valor da aposta se for válido, ou 0 se for inválido.

Python

def pegar\_aposta(saldo\_disponivel):  
    aposta \= float(input('\\nValor da aposta: R$ '))  
    if (aposta \> 0) and (aposta \<= saldo\_disponivel):  
        return aposta  
    \# ... resto da lógica ...  
    return 0

### **escolher\_cor()**

**O que faz?** Mostra o menu de cores e pede para o jogador escolher um número. Retorna o NOME da cor (ex: "PRETO") se a escolha for válida, ou uma string vazia se for inválida.

Python

def escolher\_cor():  
    opcao\_numero \= int(input('\\nDigite o número da cor (1, 2 ou 3): '))  
    if opcao\_numero \== 1:  
        return 'PRETO'  
    \# ... resto da lógica ...  
    return ""

### **gerador\_de\_frase()**

**O que faz?** Se conecta com a API do Google, envia o pedido para gerar uma frase e retorna o texto recebido.

Python

def gerador\_de\_frase():  
    \# ... lógica da API do Google ...  
    response \= model.generate\_content("Gere...")  
    return response.text.strip()

### **jogo()**

**O que faz?** É o coração do programa. Ela controla o fluxo principal:

1. Chama realizar\_deposito() para começar.  
2. Entra em um loop que dura enquanto o jogador tiver saldo.  
3. A cada rodada, chama pegar\_aposta() e escolher\_cor().  
4. Aplica a **Regra da Casa** para decidir se o sorteio será justo ou manipulado.  
5. Calcula o resultado, atualiza os saldos e mostra as mensagens na tela.

---

## **A Regra da Casa**

A parte mais importante da lógica do negócio está dentro da função jogo():

Python

if caixa\_da\_casa \< 10.00:  
    \# O sistema força uma derrota...  
    opcoes\_de\_derrota \= \[cor for cor in opcoes if cor \!= opcaoEscolhida\_cor\]  
    resultado\_sorteio \= random.choice(opcoes\_de\_derrota)  
else:  
    \# O sorteio é justo...  
    resultado\_sorteio \= random.choices(opcoes, weights=\[7, 7, 1\], k=1)\[0\]

Essa verificação garante que a casa só pague prêmios depois de ter um "fôlego" financeiro, tornando o sistema mais seguro. Para o jogador, isso é completamente invisível.
