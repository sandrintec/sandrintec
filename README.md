 #!/usr/bin/python
   2 # -*- coding: utf-8 -*-
   3 import random
   4 import sys
   5 
   6 class Numeros:
   7     """
   8     Gera os numeros para cada letra do bingo.
   9     
  10     O bingo inicia em 1 até o numero 75, dividido em 5 colunas 
  11     B de 1 a 15, I de 16 a 30, N de 31 a 45, G de 46 a 60 e O de 61 a 75
  12     """
  13     def __init__(self):
  14         self.numeros = [[],[],[],[],[]]
  15         for x in range(0,5):
  16             for y in range(x*15+1,(x+1)*15+1):
  17                 self.numeros[x].append(y)
  18 
  19 class Cartela:
  20     """
  21     Gerencia as cartelas do bingo.
  22     
  23     Cria uma nova cartela gerada apartir de numeros aletorio preenchendo cada 
  24     coluna da cartela do bingo de acordo com a quantidade de numeros informadas
  25     nos parametros de inicialização da classe
  26     """                
  27     def __init__(self,num,chave,b=5,i=5,n=5,g=5,o=5):
  28         self.pedrasmarcadas = 0
  29         self.cartela = [[],[],[],[],[]]
  30         self.quantidade = [b,i,n,g,o]
  31         self.chave = chave
  32         for x in range(0,5):
  33             self.cartela[x] = random.sample(num.numeros[x],self.quantidade[x])
  34             self.cartela[x].sort()
  35             
  36     def __eq__(self,other):
  37         """
  38         Compara duas cartelas.
  39         """
  40         return(self.cartela == other.cartela)
  41     
  42     def __str__(self):
  43         """
  44         Texto com a chave da cartela.
  45         """
  46         return("Cartela %s" % (self.chave))
  47         
  48     def exibeCartela(self):
  49         """
  50         Imprime na saida padrão os numeros da cartela.
  51         """
  52         print "=" * 3*5
  53         print "Cartela %s" % (self.chave)        
  54         print "=" * 3*5
  55         print "%3s%3s%3s%3s%3s" % ("B","I","N","G","O")
  56         for x in range(0,5):
  57             print "%3d%3d%3d%3d%3d" % (self.cartela[0][x],self.cartela[1][x], \
  58             self.cartela[2][x],self.cartela[3][x],self.cartela[4][x])
  59         print "=" * 3*5
  60     
  61     def existePedra(self,pedra):
  62         """
  63         Verifica a existencia da pedra na cartela.
  64 
  65         Identifica a qual coluna a pedra pertence em seguida verifica se nesta
  66         coluna existe a pedra,que é usada como retorno, ou uma lista vazia é
  67         retornada caso não exista a pedra na cartela
  68         """
  69         col = pedra / 15
  70         if pedra % 15 == 0:
  71             col = col - 1
  72 
  73 #        retorno = []
  74 #        for x in self.cartela[col]:
  75 #            if pedra == x:
  76 #                retorno = [x]
  77 #        return(retorno) 
  78 
  79 # O mesmo codigo pode ser escrito como abaixo:
  80         
  81         return([x for x in self.cartela[col] if pedra == x])
  82 
  83 class Bingo: 
  84     def __init__(self):
  85         """
  86         Iniciando o bingo.
  87     
  88         É inicializado o vetor de cartelas, as pedras restantes (no inicio do
  89         bingo: todas as pedras são as restantes) e as pedras do bingo separadas
  90         por coluna 5 colunas B I N G O.
  91         """
  92         self.cartelas = []
  93         self.restantes = range(1,76)
  94         self.pedras = Numeros()
  95         
  96     def removeNumerodosrestantes(self,numero):
  97         """
  98         Remove um numero da lista de numeros restantes no bingo.
  99     
 100         Cada numero da lista de restantes é comparado com o numero a ser
 101         removido, apenas os restantes diferentes do numero a ser removido são
 102         inseridos na nova lista. Incrementa-se pedras marcadas nas cartelas que
 103         possuem o numero a ser removido.
 104         """
 105         self.restantes = filter(lambda arg:arg != numero,self.restantes)
 106         
 107         #Verificando quais cartelas possuem o numero removido para incrementar
 108         # o numero de pedras marcadas nas cartelas
 109         for cartela in self.cartelas:
 110             if cartela.existePedra(numero):
 111 #                cartela.pedrasmarcadas = cartela.pedrasmarcadas + 1            
 112                 cartela.pedrasmarcadas +=  1
 113     
 114     def adicionaCartela(self,cartela):
 115         """
 116         Adiciona uma nova cartela a lista de cartelas do Bingo.
 117         """
 118         self.cartelas.append(self.confereCartela(cartela))
 119 #        self.cartelas.append(Cartela(self.pedras,chave))
 120 
 121     def confereCartela(self,comparar):
 122         """
 123         Compara se uma cartela exite idêntica na lista de cartelas do bingo.
 124         
 125         Retorna a cartela comparada caso não exista uma com os mesmos numeros na
 126         lista de cartelas do bingo e False caso ela exista na lista
 127         """
 128         for x in self.cartelas:
 129 #               if x.cartela == comparar.cartela:
 130 # Agora usando a implementação de igualdade __eq__ da classe Cartela
 131                 if x == comparar:
 132                     comparar = self.confereCartela(Cartela(self.pedras,comparar.chave))
 133                     break                   
 134         return(comparar)
 135 
 136     def sorteaPedra(self):
 137         """
 138         Escolhe uma pedra entre as restantes no bingo.
 139         """
 140         return(random.choice(self.restantes))
 141 
 142 def inicio():
 143     """
 144     iniciando o bingo.
 145     
 146     Simulação de utilização do aplicativo Bingo.
 147     """
 148     
 149     b = Bingo()
 150 
 151     #Criando Cartelas
 152     for x in range(0,100):
 153         b.adicionaCartela(Cartela(b.pedras,x+1))
 154         print b.cartelas[x]
 155     #    b.cartelas[x].exibeCartela()
 156 
 157     #Demostração que cartelas repetidas não entram na lista de cartelas
 158     print b.cartelas[0]
 159     #eu gostaria de ter tirado uma copia do objeto aqui mas nao sei como fazer
 160     #desta forma apenas uma copia do endereço do conteudo do objeto foi feita
 161     #ou seja a fala é exatamente a mesma primeira cartela
 162     falsa = b.cartelas[0]
 163     #quando altero a chave da falsa estou alterando a chave da primera tb
 164     falsa.chave = "Falsa"
 165     falsa.exibeCartela()
 166     b.adicionaCartela(falsa)
 167     #e isso a prova q uma mesma cartela (levando em cosideração os numeros) não
 168     #é adicionada duas vezes na lista de cartela do bingo
 169     b.cartelas[x+1].exibeCartela()
 170     #isso é a prova de que nao foi feito uma copia
 171     b.cartelas[0].exibeCartela()
 172 
 173     
 174     # simulando um bingo
 175     while b.restantes:
 176         pedrasorteada = b.sorteaPedra()
 177         print pedrasorteada,
 178         b.removeNumerodosrestantes(pedrasorteada)
 179         for cartela in b.cartelas:
 180             if cartela.pedrasmarcadas == 25:
 181                 print
 182                 print "BATEU"
 183                 print "Faltavam %d" % (len(b.restantes)) 
 184                 print b.restantes
 185                 cartela.exibeCartela()
 186                 sys.exit()
 187 
 188 #Executando o aplicativo se for chamado diretamente
 189 if __name__ == '__main__':
 190     inicio()
