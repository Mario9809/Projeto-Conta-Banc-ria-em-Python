**Sistema Bancário Simulado em Python**
Projeto de estudo desenvolvido em Python para simular operações bancárias realizadas por empresas, utilizando uma interface de linha de comando. A aplicação permite cadastrar clientes, consultar informações e registrar movimentações financeiras em memória, além de gerar arquivos de texto com dados e históricos de operações.

> **Projeto educacional:** este código é uma simulação, não um sistema bancário real. Há funcionalidades que precisam de ajustes, especialmente cálculos de tarifas, validações e agendamento de transferências.

Funcionalidades:

Cadastro de clientes: razão social, CNPJ, tipo de conta (Comum ou Plus), saldo inicial e senha.
Exclusão e listagem: remoção de clientes pelo CNPJ e exibição dos cadastros da sessão.
Débito: tentativa de retirada de valores mediante identificação por CNPJ e senha, com regras de tarifa e limite conforme o tipo de conta.
Depósito: adição de valores ao saldo da conta informada.
Extrato: exibição de movimentações com data, valor, tarifa e saldo registrado.
Transferências: movimentação de valores entre clientes cadastrados.
TED automático (experimental): tentativa de programar uma transferência para um horário definido no código.
Geração de arquivos `.txt`: gravação de informações cadastrais e de extratos.

Tecnologias utilizadas

Python 3
Módulos da biblioteca padrão: `datetime` e `time`
Estruturas como listas, dicionários, funções, condicionais e laços de repetição
Leitura de dados pelo terminal e gravação em arquivos de texto

Menu disponível

Opção	Operação
1	Cadastrar novo cliente
2	Excluir cliente
3	Listar clientes
4	Realizar débito
5	Realizar depósito
6	Consultar extrato
7	Transferir entre contas
8	TED automático (experimental)
9	Sair

Arquivos gerados

Ao cadastrar clientes, o código escreve arquivos no formato `dados_cliente_<CNPJ>.txt`. Ao consultar um extrato, gera ou atualiza `extrato_<CNPJ>.txt`.
Esses arquivos são criados no diretório de execução do programa. O conteúdo cadastral inclui a senha informada pelo usuário. Por isso, utilize apenas dados fictícios e, se for compartilhar exemplos dos resultados neste repositório, remova qualquer informação pessoal ou credencial.
O programa não lê automaticamente os arquivos salvos para restaurar as contas após ser encerrado.

Os nomes do PDF e dos arquivos de exemplo devem ser ajustados aos arquivos reais.
Limitações e melhorias futuras
Este é um protótipo de aprendizagem. Entre os próximos passos estão:
Corrigir as fórmulas de tarifas e os abatimentos de saldo em débitos e transferências.
Revisar o agendamento de TED: na implementação atual, há risco de débito duplicado e o programa fica bloqueado enquanto aguarda o horário.
Validar valores digitados, evitando entradas incorretas e operações com valores negativos.
Não exibir nem salvar senhas em texto simples; separar autenticação dos dados públicos de cadastro.
Melhorar a persistência dos dados, permitindo carregar os clientes de sessões anteriores.
Criar testes automatizados para as principais operações.

Objetivo de aprendizagem
Praticar lógica de programação em Python por meio de um sistema com menu interativo, manipulação de estruturas de dados, uso de funções, cálculos, registros de transações e escrita de arquivos.
