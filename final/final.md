# Avaliação Final

O objetivo desta avaliação é combinar o conteúdo de dois arquivos do tipo CSV (_Comma Separeted Values_)
em um único arquivo, também do tipo CSV.

Os dois arquivos podem ser obtidos no
[site da B3 (bolsa de valores)](https://www.b3.com.br/pt_br/market-data-e-indices/servicos-de-dados/market-data/consultas/boletim-diario/dados-publicos-de-produtos-listados-e-de-balcao/).
Eles são os seguintes:
- Cadastro de Instrumentos (Listados)
- Posições em Aberto em Derivativos (Listado)

## Cadastro de Instrumentos (Listados)
Este arquivo informações sobre todos os produtos negociados na bolsa no dia do relatório. O arquivo é formado, dentre outras,
pelas seguintes colunas:
- **RptDt:** Data do relatório
- **TckrSymb:** Código de negociação
- **Asst:** Código do produto relacionado, para o caso do produto negociado ser um derivado
- **SctyCtgyNm:** Um dos níveis de classificação do instrumento (produto) negociado
- **XprtnDt:** Data de vencimento do instrumento (quando for o caso)
- **OptnTp:** Indica, para instrumentos do tipo opção (direito), se trata-se de um direito de compra (Call) ou um direito de venda (Put)
- **ExrcPric:** Preço de exercício. Indica, para instrumentos do tipo opção (direito), o preço em que o titular tem o direito de exercer. 
- **OptnStyle:** Indica, para instrumentos do tipo opção (direito), se é uma opção americana ou europeia.

## Posições em Aberto em Derivativos (Listado)
Este arquivo contém informações sobre instrumentos que foram vendidos sem que o vendedor tivesse a posse do
instrumento (sim, na bolsa é possível vender algo sem ter, mas em outras situações fora da bolsa isso pode ser considerado um crime).

Este arquivo contém, dentre outras colunas, as seguintes:
- **RptDt:** Data do relatório
- **TckrSymb:** Código do instrumento negociado
- **CvrdQty:** Quantidade de instrumentos vendidos mas que estão protegidos pelo instrumento base. 
- **TtlBlckdPos:** Quantidade de instrumentos vendidos que estão protegidos por outro instrumento derivado.
- **UcvrdQty:** Quantidade de instrumentos vendidos sem qualquer proteção.
- **TtlPos:** Total de instrumentos vendidos.

# Objetivo
Seu objetivo é combinar o conteúdo dos dois arquivos acima formando um novo arquivo com as seguintes colunas:
#### Do arqvuivo: Cadastro de Instrumentos (Listados)
- **RptDt:** Data do relatório
- **TckrSymb:** Código de negociação
- **Asst:** Código do produto relacionado, para o caso do produto negociado ser um derivado
- **SctyCtgyNm:** Um dos níveis de classificação do instrumento (produto) negociado
- **XprtnDt:** Data de vencimento do instrumento (quando for o caso)
- **OptnTp:** Indica, para instrumentos do tipo opção (direito), se trata-se de um direito de compra (Call) ou um direito de venda (Put)
- **ExrcPric:** Preço de exercício. Indica, para instrumentos do tipo opção (direito), o preço em que o titular tem o direito de exercer. 
- **OptnStyle:** Indica, para instrumentos do tipo opção (direito), se é uma opção americana ou europeia.

#### Do arquivo: Posições em Aberto em Derivativos (Listado)
- **CvrdQty:** Quantidade de instrumentos vendidos mas que estão protegidos pelo instrumento base. 
- **TtlBlckdPos:** Quantidade de instrumentos vendidos que estão protegidos por outro instrumento derivado.
- **UcvrdQty:** Quantidade de instrumentos vendidos sem qualquer proteção.
- **TtlPos:** Total de instrumentos vendidos.

# Observações
É importante saber que nem todos os **Instrumentos Listados** estarão presentes no arquivo
**Posições em Aberto em Derivativos (Listado)**. Portanto, você deverá armazenar todos os dados dos dois arquivos em
um _array_ de _structs_ adequado (as _structs_ estão definidas mais a frente). Em seguida, você deverá percorrer os
dois _arrays_ encontrando os **TckrSymb** presentes em ambos. Quando encontrar um desses códigos, você deverá escrever
os dados em um novo arquivo.

# Structs
São três as _structs_ a serem usadas no presente trabalho.
#### Arqvuivo: Cadastro de Instrumentos (Listados)
```c
typedef struct {
    char RptDt[11];
    char TckrSymb[15];
    char Asst[15];
    char SctyCtgyNm[21];
    char XprtnDt[11];
    char OptnTp[5];
    char ExrcPric[11];
    char OptnStyle; // A = Americana,  E = Europeia
}InstrumentosListados;
```

#### Arquivo: Posições em Aberto em Derivativos (Listado)
```c
typedef struct {
  char TckrSymb[15];
  unsigned int CvrdQty;
  unsigned int TtlBlckdPos;
  unsigned int UcvrdQty;
  unsigned int TtlPos;
}PosicoesAbertas;
```

#### Arquivo resultante
```c
typedef struct {
    char TckrSymb[15];
    char Asst[15];
    char SctyCtgyNm[21];
    char XprtnDt[11];
    char OptnTp[5];
    char ExrcPric[11];
    char OptnStyle; // A = Americana,  E = Europeia
    unsigned int CvrdQty;
    unsigned int TtlBlckdPos;
    unsigned int UcvrdQty;
    unsigned int TtlPos;
}InstrumentosListados;
```

# Iniciando
1. Crie um arquivo chamado `consolidar.c` que conterá seu código. Depois de compilado, seu programa
deverá se chamar `consolidar` e ser executado como abaixo
```$ executar listados.csv abertos.csv resultado.csv```
em que
- `listados.csv` é o arquivo que contém o cadastro de instrumentos listados
- `abertos.csv` é o arquivo que contém os dados de posições em aberto (isto é, produtos vendidos sem que o vendedor tivesse o produto para vender)
- `resultado.csv` é o arquivo com os dados consolidados após a junção dos conteúdos dos arquivos `listados.csv`e `abetos.csv`.
2. Seu programa deverá criar um _array_ dinamicamente alocado para cada um dos arquivos.
3. A medida que você for obtendo novos dados, se o tamanho do _array_ ficar insuficiente, você deverá usar a função `realloc` para aumentar o tamanho do
4. _ array_ a fim de comportar os novos dados
5. O arqvuivo `resultado.csv` deverá ser um arquivo do tipo CSV contendo as seguintes colunas
```TckrSymb;Asst;SctyCtgyNm;XprtnDt;OptnTp;ExrcPric;OptnStyle;CvrdQty;TtlBlckdPos;UcvrdQty;TtlPos```
6. Seus dados devem estar como comentário no início do arquivo, contendo
```
MATRÍCULA: ................. 
NOME: ..................
USUÁRIO: ...............
```

# Entrega
- A entrega poderá ser feita até as 15:00 do dia 20/07/2022 via classroom.
- Apenas o arquivo `consolidar.c` deverá ser enviado.
- Se o código não compilar, será atribuída nota zero.
- Caso o código compile, o funcionamento do mesmo será testado.
- A estilização do código vale 20% da nota, o funcionamento vale 80%.
