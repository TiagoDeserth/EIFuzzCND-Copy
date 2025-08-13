## 🖥️ EIFuzzCND: Classificador Fuzzy Incremental para Detecção de Novidades

## ⚠️ Aviso: Clone do Repositório Original 

🏷️ Este repositório é um clone do trabalho original desenvolvido por Lucas Ricardo Duarte Bruzzone como parte da sua dissertação de mestrado. Todo o crédito pelo algoritmo e pela pesquisa pertence ao autor original.

* **Autor original**: [Lucas Ricardo Duarte Bruzzone](https://github.com/lucas-bruzzone)

---

## 💻 Sobre o Projeto

🏷️ O EIFuzzCND é um algoritmo de aprendizado de máquina projetado para classificação multiclasse e detecção de novidades em fluxos de dados (*data streams*). Ele utiliza uma abordagem incremental com lógica *fuzzy* para se adaptar a mudanças de conceito (*concept drift*) e identificar novos padrões em tempo real, mesmo em cenários com latência para obter os rótulos verdadeiros dos dados.

## 🚀 Guia de Execução

🏷️ Este guia detalha todos os passos necessários para configurar o ambiente e executar o algoritmo na sua máquina local.

### 🗒️ 1. Pré-requisitos

Antes de começar, garanta que você tem os seguintes softwares instalados:

* ⚡ **Java Development Kit (JDK)**: Versão 8 ou superior;
* ⚡ **Apache Maven**: Para gerenciamento de dependências e compilação do projeto.

### 🛠️ 2. Preparação do Ambiente

#### 🗂️ a. Estrutura de Pastas

O código espera uma estrutura de pastas específicas para os datasets.

📂 1. Na raiz do projeto, crie uma pasta chamada `datasets`;

📊 2. Dentro de `datasets`, crie uma pasta para cada conjunto de dados que for usar (ex.: `meu_dataset`);

📈 3. Dentro da pasta de cada dataset, crie uma subpasta chamada `graphics_data`. O programa usará esta pasta para salvar os resultados e gráficos.

A estrutura final deve ser a seguinte:

```
EIFuzzCND-main/
├── datasets/
│   └── nome_do_dataset/
│       ├── graphics_data/
│       ├── nome_do_dataset-train.arff
│       └── nome_do_dataset-instances.arff
├── src/
│   └── ...
└── pom.xml
```

#### 📜 b. Preparação dos Datasets

O algoritmo precisa de **dois arquivos** no formato **`.arff`** (padrão Weka) para cada dataset:

* `<nome_do_dataset>-train.arff`: Um subconjunto dos dados para o treino inicial (fase Offline);
* `<nonme_do_dataset-instances.arff`: O restante dos dados para simular o fluxo (fase Online).

**Se os seus dados estiverem em `.csv`**:

1. **Converta para `.arff`**: Linha de comando: Após compilar o projeto com ``mvn package``, a biblioteca Weka já estará descarregada no seu sistema. Você pode usá-la para converter os seus ficheiros diretamente no terminal.
   - Abra um terminal na pasta onde está o seu dataset ``.csv``;
   - Execute o comando abaixo (ajuste o caminho para o seu utilizador do Windows):
     ``
     java -cp "C:\Users\SEU_UTILIZADOR\.m2\repository\nz\ac\waikato\cms\weka\weka-stable\3.8.6\weka-stable-3.8.6.jar" weka.core.converters.CSVLoader seu_ficheiro.csv > seu_ficheiro_convertido.arff
     ``

2. **Corrija a Codificação**: Após a conversão, abra os datasets `.arff` no Bloco de Notas (ou outro editor de texto), vá em `Arquivo > Guardar Como...` e mude a codificação para **ANSI**. Isto previne erros de leitura no Weka.

3. **Divida os Ficheiros**: Separe o seu ficheiro `.arff` em duas partes (`-train` e `-instances`), conforme necessário para o seu experimento.

### 3. Configuração do Código

Edite o ficheiro principal para indicar qual dataset você quer executar.

1.  Abra o arquivo: `src/main/java/EIFuzzCND/FuzzySystem.java`.
2.  Altere o valor da variável `dataset` para o nome da pasta do seu dataset.

    ```java
    public class FuzzySystem {
        public static void main(String[] args) throws IOException, Exception {
            // Altere esta linha para o nome da sua pasta de dataset
            String dataset = "meu_dataset"; 
            // ...
        }
    }
    ```
3. **(Opcional, mas importante)** Verifique se não há um espaço a mais na linha que carrega o dataset de treino (um bug no código original). A linha deve ser:
   ```java
   source = new ConverterUtils.DataSource(caminho + dataset + "-train.arff");
   ```