# Ananalise-de-sentimentos-Modelos

## Instala as bibliotecas
    !pip install pandas
    !pip install transformers

## Importa as bibliotecas
    import pandas as pd
    from transformers import pipeline

## Caminho do arquivo
    # Substitua pelo caminho real da sua planilha
    # file_path = '/content/drive/MyDrive/tweets.xlsx'  # Exemplo: Planilha no Google Drive
    file_path = '/Tweets.xlsx' # Exemplo: Planilha local

## Processa as informações
    try:
      # Lê a planilha do Excel
      df = pd.read_excel(file_path)

      # Verifica se a coluna 'Tweet' existe. Substitua 'Tweet' pelo nome da sua coluna.
      if 'TweetText' not in df.columns:
        print("Erro: A coluna 'TweetText' não foi encontrada na planilha.")
      else:
        # Cria o classificador de sentimentos
        classifier = pipeline("sentiment-analysis")

        # Aplica a análise de sentimentos à coluna 'Tweet'
        sentimentos = []
        for tweet in df['TweetText']:
            try:
                resultado = classifier(tweet)
                sentimentos.append(resultado[0]['label']) # Armazena apenas o rótulo (positivo/negativo)
            except Exception as e:
                print(f"Erro ao analisar o tweet: {tweet}. Erro: {e}")
                sentimentos.append("Erro") # Adiciona "Erro" na lista se houver problema no tweet

        # Adiciona a coluna de sentimentos ao DataFrame
        df['Sentimento'] = sentimentos
        print(df[['TweetText', 'Sentimento']])

    except FileNotFoundError:
      print(f"Erro: O arquivo '{file_path}' não foi encontrado.")
    except Exception as e:
      print(f"Ocorreu um erro: {e}")

## Gerando um gráfico de barras com as informações

    import matplotlib.pyplot as plt

    from transformers import pipeline

    file_path = '/Tweets.xlsx' # Exemplo: Planilha local

    try:
      # Lê a planilha do Excel
      df = pd.read_excel(file_path)

      # Verifica se a coluna 'Tweet' existe. Substitua 'Tweet' pelo nome da sua coluna.
      if 'TweetText' not in df.columns:
        print("Erro: A coluna 'TweetText' não foi encontrada na planilha.")
      else:
        # Cria o classificador de sentimentos
        classifier = pipeline("sentiment-analysis")

        # Aplica a análise de sentimentos à coluna 'Tweet'
        sentimentos = []
        for tweet in df['TweetText']:
            try:
                resultado = classifier(tweet)
                sentimentos.append(resultado[0]['label']) # Armazena apenas o rótulo (positivo/negativo)
            except Exception as e:
                print(f"Erro ao analisar o tweet: {tweet}. Erro: {e}")
                sentimentos.append("Erro") # Adiciona "Erro" na lista se houver problema no tweet


        # Adiciona a coluna de sentimentos ao DataFrame
        df['Sentimento'] = sentimentos
        print(df[['TweetText', 'Sentimento']])

        # Mostra as palavras associadas aos sentimentos (exemplo básico)
        positive_tweets = df[df['Sentimento'] == 'POSITIVE']['TweetText']
        negative_tweets = df[df['Sentimento'] == 'NEGATIVE']['TweetText']

        # Conta a quantidade de sentimentos positivos e negativos
        sentimento_counts = df['Sentimento'].value_counts()

        # Cria o gráfico de barras
        plt.figure(figsize=(8, 6))
        plt.bar(sentimento_counts.index, sentimento_counts.values)
        plt.xlabel("Sentimento")
        plt.ylabel("Número de Tweets")
        plt.title("Análise de Sentimentos dos Tweets")
        plt.show()

    except FileNotFoundError:
      print(f"Erro: O arquivo '{file_path}' não foi encontrado.")
    except Exception as e:
      print(f"Ocorreu um erro: {e}")

## Utilizando outro modelo de aprendizado
    # Substitua pelo caminho real da sua planilha
    file_path = '/Tweets.xlsx'  # Exemplo: Planilha local

    try:
        # Lê a planilha do Excel
        df = pd.read_excel(file_path)

        # Verifica se a coluna 'Tweet' existe. Substitua 'Tweet' pelo nome da sua coluna.
        if 'TweetText' not in df.columns:
            print("Erro: A coluna 'TweetText' não foi encontrada na planilha.")
        else:
            # Cria o classificador de sentimentos usando um modelo mais abrangente
            classifier = pipeline("sentiment-analysis", model="cardiffnlp/twitter-roberta-base-sentiment")

            # Aplica a análise de sentimentos à coluna 'Tweet'
            sentimentos = []
            for tweet in df['TweetText']:
                try:
                    resultado = classifier(tweet)
                    label = resultado[0]['label']
                    # Adapta os rótulos do modelo para serem consistentes com POSITIVE/NEUTRAL/NEGATIVE
                    if label == "LABEL_0":  # Geralmente negativo
                        sentimentos.append("NEGATIVE")
                    elif label == "LABEL_1":  # Geralmente neutro
                        sentimentos.append("NEUTRAL")
                    elif label == "LABEL_2":  # Geralmente positivo
                        sentimentos.append("POSITIVE")
                except Exception as e:
                    print(f"Erro ao analisar o tweet: {tweet}. Erro: {e}")
                    sentimentos.append("Erro")  # Adiciona "Erro" na lista se houver problema no tweet

            # Adiciona a coluna de sentimentos ao DataFrame
            df['Sentimento'] = sentimentos
            print(df[['TweetText', 'Sentimento']])

            # Conta a quantidade de cada sentimento
            sentimento_counts = df['Sentimento'].value_counts()

            # Cria o gráfico de barras
            plt.figure(figsize=(8, 6))
            plt.bar(sentimento_counts.index, sentimento_counts.values, color=['green', 'orange', 'red'])
            plt.xlabel("Sentimento")
            plt.ylabel("Número de Tweets")
            plt.title("Análise de Sentimentos dos Tweets")
            plt.show()

    except FileNotFoundError:
        print(f"Erro: O arquivo '{file_path}' não foi encontrado.")
    except Exception as e:
        print(f"Ocorreu um erro: {e}")

    
