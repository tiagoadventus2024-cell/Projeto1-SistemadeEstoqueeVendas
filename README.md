# Sistema Boutique - Gestão de Estoque

O Sistema Boutique é uma aplicação robusta de interface de linha de comando (CLI) desenvolvida em Python, voltada para o gerenciamento dinâmico e eficiente do inventário de lojas de vestuário. O projeto foi projetado com foco rigoroso nos princípios da Programação Orientada a Objetos (POO), na aplicação de algoritmos otimizados de estruturas de dados e na persistência segura de informações em arquivos estruturados.

Esta aplicação foi totalmente adaptada para execução em ambientes de script tradicionais e notebooks interativos (como o Google Colab), fornecendo controle de integridade em tempo real para todas as movimentações de mercadorias.

---

## Funcionalidades do Sistema

O sistema fornece uma interface interativa baseada em menu com 10 operações essenciais para o controle de negócios:

1. **Cadastro de Produtos:** Inserção de novas peças com categorização automática baseada em herança ou definição de categorias personalizadas.
2. **Edição Avançada:** Modificação seletiva de atributos (descrição, cor, tamanho, preço e quantidade) com reaplicação das regras de validação.
3. **Remoção Otimizada:** Exclusão definitiva de itens do inventário utilizando índices mapeados para evitar processamento redundante.
4. **Busca por Código (Alta Performance):** Localização instantânea de produtos por meio do algoritmo de Busca Binária.
5. **Busca por Descrição (Textual):** Varredura posicional no catálogo por termos ou fragmentos de nomes, ignorando distinções entre maiúsculas e minúsculas.
6. **Movimentação de Vendas:** Registro de saídas de estoque com validação impeditiva para saldos insuficientes ou valores inválidos.
7. **Inventário Geral Paginado:** Exibição estruturada em colunas de todo o estoque, ordenada nativamente por código e dividida em páginas para leitura legível.
8. **Filtros Segmentados:** Isolamento de visualização de itens por categorias específicas ou por grade de tamanhos.
9. **Relatório de Estoque Crítico:** Identificação imediata de produtos com quantidades abaixo do limite de segurança estipulado pelo gestor.
10. **Encerramento com Salvamento Automático:** Sincronização e gravação de todos os dados em disco antes do fechamento do programa.

---

## Arquitetura e Engenharia de Software

### 1. Programação Orientada a Objetos (POO)
O domínio do sistema foi modelado para refletir conceitos consolidados de engenharia de software:
* **Herança e Polimorfismo:** A classe abstrata/base `Roupa` encapsula os comportamentos e atributos comuns. As subclasses `RoupaCasual`, `RoupaFormal` e `RoupaEsportiva` herdam os mecanismos base e definem suas categorias de forma fixa e automatizada em seus construtores.
* **Encapsulamento e Validações de Negócio:** Atributos críticos não são atribuídos livremente. O construtor e os métodos de edição disparam validadores internos para assegurar que:
  * O preço de venda seja estritamente positivo (maior que zero).
  * A quantidade em estoque nunca seja negativa.
  * O tamanho pertença à grade padrão padronizada (PP, P, M, G, GG, XG, UNICO) ou seja uma numeração estritamente digital (ex: 38, 40, 42).

### 2. Estruturas de Dados e Análise de Complexidade (Big-O)
Para otimizar o consumo de memória e o tempo de execução, o sistema gerencia o inventário através de dois vetores síncronos redundantes:
* **Vetor Não Ordenado (`vetor_nao_ordenado`):** Utilizado para inserções em tempo constante $O(1)$ e para filtragens/buscas textuais lineares.
* **Vetor Ordenado (`vetor_ordenado`):** Mantido em ordenação crescente contínua com base no código do produto.
  * **Algoritmo de Inserção:** Baseado na lógica de posicionamento estável do *Insertion Sort*. Ao cadastrar, o sistema localiza a posição correta e desloca os elementos em complexidade $O(n)$, eliminando a necessidade de ordenar todo o vetor a cada consulta.
  * **Busca Binária:** Implementada para a busca por código único. Reduz o espaço de busca pela metade a cada iteração, alcançando uma eficiência de tempo de $O(\log n)$. É ideal para bases de dados de grande escala.
  * **Busca Linear:** Utilizada na pesquisa por descrição e filtros, operando em tempo $O(n)$ ao percorrer o vetor não ordenado.

### 3. Persistência de Dados e Reconstrução Dinâmica
A camada de persistência utiliza o formato estruturado JSON (`estoque_roupas.json`). O diferencial técnico está na capacidade de serialização e desserialização:
* **Serialização:** O método `to_dict()` converte as instâncias ativas do sistema em dicionários Python nativos, injetando metadados sobre a classe de origem (`tipo_classe`).
* **Desserialização com Factory Pattern:** Ao iniciar, o sistema lê o arquivo e utiliza um mapa de classes (`MAPA_CLASSES`) para instanciar dinamicamente os objetos de acordo com suas subclasses originais (`RoupaCasual`, `RoupaFormal`, etc.), restabelecendo todos os métodos e validações em memória de forma transparente.

### 4. Robustez da Interface CLI
* **Sanitização de Entradas:** Métodos customizados como `ler_inteiro()` e `ler_float()` tratam exceções de conversão de dados (`ValueError`), impedindo o colapso do sistema caso o usuário digite caracteres alfabéticos em campos numéricos.
* **Paginação de Relatórios:** Impede o estouro de buffer visual no terminal ao segmentar exibições longas em blocos configuráveis (padrão de 5 itens por página).

---

## Estrutura do Arquivo de Persistência

Exemplo de como as informações são organizadas de forma limpa e padronizada no arquivo `estoque_roupas.json`:

```json
[
    {
        "tipo_classe": "RoupaCasual",
        "codigo": 123,
        "descricao": "Camiseta Algodao Premium",
        "categoria": "Casual",
        "tamanho": "M",
        "cor": "Azul Marinho",
        "preco": 89.90,
        "quantidade": 45
    },
    {
        "tipo_classe": "RoupaFormal",
        "codigo": 124,
        "descricao": "Blazer Slim Fit",
        "categoria": "Formal",
        "tamanho": "G",
        "cor": "Preto",
        "preco": 349.00,
        "quantidade": 12
    }
]

```
### Colaboradores
Kaua Barroso Silva 
Tiago Soares da Cruz
Erick Maycon da Silva Carneiro
Argeu Viana Almeida
