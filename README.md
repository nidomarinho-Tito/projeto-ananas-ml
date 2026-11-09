# projeto-ananas-ml
API Preditiva de Cultivares de Abacaxi (Ananas comosus)
Projeto Ananas — Classificação de Cultivares de Abacaxi
Carreira Tech — Trilha Inteligência Artificial
Machine Learning Preditivo com Python

Autor: Nivaldo Marinho
E-mail: nidomarinho@gmail.com
GitHub (repositório do projeto): https://github.com/nidomarinho-Tito/projeto-ananas-ml
Vídeo de apresentação:https://drive.google.com/file/d/19YrNk9xEyny5M5uD5K8lVWqgzFIa5I7n/view?usp=sharing
1. Apresentação
O Projeto Ananas é uma aplicação didática de aprendizado de máquina voltada à classificação de cinco cultivares de abacaxi (Ananas comosus), com base em características morfoagronômicas do fruto. O projeto integra um pipeline de classificação desenvolvido com scikit-learn a uma API REST construída com FastAPI, permitindo enviar atributos de uma amostra e receber a previsão do modelo.

A implementação foi organizada em módulos Python para separar o treinamento, as regras de validação, o serviço de inferência e os endpoints da API. O desenvolvimento e a demonstração podem ser realizados no Google Colab.
Limitação importante: os dados utilizados são sintéticos. As previsões e métricas demonstram o funcionamento do sistema em uma distribuição simulada; não representam identificação agronômica validada nem recomendação para uso em campo.

2. Objetivo
Demonstrar, de ponta a ponta, o desenvolvimento de uma solução preditiva em Python: preparação de dados, treinamento e avaliação de um classificador, persistência do modelo, disponibilização por API, validação de entradas e testes automatizados.
3. Tecnologias utilizadas
- Python: linguagem principal do projeto.
- pandas e NumPy: manipulação e preparação dos dados.
- scikit-learn: pipeline de pré-processamento e modelo Random Forest.
- Joblib: serialização do pipeline treinado.
- FastAPI: criação dos endpoints HTTP.
- Pydantic: validação e tipagem das entradas.
- Uvicorn: execução do servidor ASGI.
- unittest: testes automatizados.
- Google Colab: ambiente de execução e apresentação do notebook.
As versões e restrições das bibliotecas devem ser consultadas no arquivo requirements.txt.
4. Organização do projeto
Projeto_Ananas/
├── app/
│   ├── __init__.py
│   ├── main.py              # Inicialização da API e configuração de CORS
│   ├── routers.py           # Rotas e respostas HTTP
│   ├── schemas.py           # Modelos de entrada e validação
│   └── services.py          # Carregamento do modelo e inferência
├── data/                    # Base sintética, proveniência e saídas em lote
├── models/                  # Modelo, métricas e arquivos de avaliação
├── scripts/
│   └── train_pipeline.py    # Treinamento e avaliação
├── tests/
│   └── test_api.py          # Testes da API
├── requirements.txt         # Dependências
└── README.md                # Documentação
Os arquivos gerados em data/ e models/ dependem da execução das respectivas etapas.
5. Como o sistema funciona
1. Preparação: é utilizada uma base sintética com atributos numéricos e categóricos de frutos.
2. Divisão dos dados: o conjunto é separado de forma estratificada em treinamento (70%), validação (15%) e teste (15%).
3. Treinamento: um pipeline aplica o pré-processamento e treina o classificador Random Forest.
4. Avaliação: o modelo é analisado por métricas e matriz de confusão; os índices do teste isolado são preservados.
5. Persistência: o pipeline treinado é salvo para reutilização sem necessidade de retreinamento a cada requisição.
6. Inferência: a API recebe os atributos do fruto, valida os campos e retorna a previsão produzida pelo modelo.
6. Executar no Google Colab
1. Acesse Google Colab.
2. Faça upload do notebook do Projeto Ananas (.ipynb).
3. Selecione Ambiente de execução → Executar tudo ou execute as células em sequência, acompanhando cada etapa.
4. Aguarde a instalação das dependências e a geração dos arquivos necessários.
5. Execute as células de treinamento, avaliação e testes.
6. Inicie a API pela etapa indicada no notebook e abra o link da documentação interativa Swagger disponibilizado pelo proxy do Colab.
7. Ao final, gere o pacote de entrega com os resultados produzidos na sessão.
Atenção: o endereço 127.0.0.1 é local ao ambiente que executa o servidor. No Colab, utilize o link do proxy fornecido pelo notebook para abrir a API no navegador.
7. Executar localmente
No terminal, dentro da pasta raiz do projeto:
# Instalar as dependências
python -m pip install -r requirements.txt

# Gerar os dados e treinar o pipeline
python scripts/train_pipeline.py

# Executar os testes automatizados
python -m unittest discover -s tests -v

# Iniciar a API
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000
Com o servidor em execução, acesse:
- Documentação Swagger: http://127.0.0.1:8000/docs
- Verificação de saúde: http://127.0.0.1:8000/health
8. Endpoints e respostas HTTP
Método	Rota	Finalidade
GET	/health	Verificar o estado da aplicação e do modelo.
POST	/v1/predict	Receber os atributos e solicitar uma classificação.
POST	/predict	Rota alternativa de previsão.


Exemplo de entrada
Envie o JSON abaixo no corpo da requisição POST /v1/predict, por meio do Swagger ou de um cliente HTTP:
{
  "massa_fruto_g": 1480,
  "comprimento_fruto_cm": 16.2,
  "diametro_fruto_cm": 12.1,
  "diametro_cilindro_mm": 18.5,
  "brix_sst": 17.6,
  "acidez_titulavel_pct": 0.52,
  "firmeza_polpa_n": 20.5,
  "cor_polpa": "amarelo_ouro",
  "presenca_espinhos": "ausente",
  "formato_fruto": "cilindrico"
}
O que esperar: após a validação, a API executa o pipeline e devolve uma previsão de cultivar. O formato exato da resposta pode ser conferido no Swagger, que documenta o esquema efetivamente utilizado pela aplicação.
Tratamento de erros
Código HTTP	Interpretação
200	Requisição processada com sucesso.
400	Inconsistência morfométrica identificada por regra didática.
422	Dados ausentes, inválidos ou incompatíveis com o esquema.
503	Modelo indisponível para inferência.
500	Falha interna inesperada.


A regra de consistência envolvendo o limite de 65% é uma heurística de demonstração e não possui validação agronômica.
9. Avaliação do modelo
O projeto utiliza métricas de classificação e uma matriz de confusão para observar o comportamento do algoritmo na base simulada. O conjunto de teste é mantido separado do treinamento; seus índices são registrados em models/test_indices.json e reutilizados na matriz de confusão.

Os hiperparâmetros são definidos antes da avaliação final. A validação intermediária tem caráter diagnóstico, não constituindo uma busca sistemática de hiperparâmetros. As probabilidades previstas não foram calibradas e não devem ser interpretadas como certeza da identificação em condições reais.

Onde consultar os resultados: os indicadores, gráficos e demais artefatos são produzidos durante a execução e armazenados na pasta models/ e nos relatórios de saída do projeto. Não são apresentados valores fixos neste README para evitar divergência entre execuções.
10. Origem dos dados e limitações
A base de treinamento é sintética, construída para demonstrar técnicas de Machine Learning. As características das cultivares e os parâmetros numéricos utilizados na simulação não devem ser tratados automaticamente como medições experimentais extraídas de catálogos oficiais.

Consulte data/proveniencia_base.md para a descrição da origem, das hipóteses adotadas e das limitações dos dados. Para aplicação científica ou operacional, seria necessário utilizar dados reais documentados, com metodologia de coleta, representatividade e validação independente.
11. Segurança e boas práticas
- Carregue arquivos .joblib somente de fontes confiáveis.
- O CORS permanece desativado por padrão; para autorizar uma origem específica, configure ANANAS_CORS_ORIGINS antes de iniciar a API.
- Execute os testes antes de apresentar ou distribuir o projeto.
- Registre as versões do ambiente para facilitar a reprodução dos resultados.
12. Entregáveis
Ao concluir as etapas do notebook, o pacote final pode reunir o código-fonte, a base sintética, os testes, o modelo treinado, as métricas, os gráficos e o relatório de execução. Confira o conteúdo do ZIP após sua geração, pois os arquivos de resultado dependem da execução completa.
13. Apresentação e contato
Projeto acadêmico: Carreira Tech — Trilha Inteligência Artificial
Área: Machine Learning Preditivo com Python
Desenvolvido por: Nivaldo Marinho
- Vídeo de apresentação: https://drive.google.com/file/d/19YrNk9xEyny5M5uD5K8lVWqgzFIa5I7n/view?usp=sharing
- Repositório no GitHub: https://github.com/nidomarinho-Tito/projeto-ananas-ml
- E-mail para contato: nidomarinho@gmail.com
Projeto desenvolvido com finalidade educacional, para demonstrar a integração entre aprendizado de máquina, organização de código Python e disponibilização de modelos por API REST.
