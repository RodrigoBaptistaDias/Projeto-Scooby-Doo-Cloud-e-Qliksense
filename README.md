# Projeto-Scooby-Doo-Google-Cloud-e-Qliksense
Um projeto juntando conceitos de SQL no Google Cloud em conjunto com visualização de dados pelo QlikSense

### Banco de Dados
O banco de dados utilizado é o "Scooby-Doo Complete - Episode List - Update 10 19 21", retirado do Kaggle

[Scooby-Doo Kaggle](https://www.kaggle.com/datasets/williamschooleman/scoobydoo-complete?resource=download%5D)

Caso haja interesse o Data Dictionary do mesmo pode ser encontrado aqui 

[Data Dictionary Scooby-Doo](https://github.com/rfordatascience/tidytuesday/tree/master/data/2021/2021-07-13#data-dictionary)

### AVISO
Gosto de escrever bastante, então como pode ver, tem bastante texto, caso você seja alguém ocupado ou com pouco tempo, as imagens estão com o código comentado de forma mais direta, no geral o texto são motivações do porque eu fiz e o que passava na minha mente enquanto tratava os dados.

Obrigado pela preferência!

### MOTIVO

Como se é esperado de todo e qualquer projeto, a motivação para fazer o mesmo. Acredito que principalmente para sair da zona de conforto, uma vez que foi minha primeira experiência com o Bigquery do Google Cloud. Não posso falar o mesmo do QlikSense, um antigo amigo ao qual foi trocado pelo seu irmão "mais bonito" mas não tão talentoso, PowerBi. Utilizei dessa experiência para ter um primeiro contato novamente com o mesmo, como um aquecimento antes de correr, cada projeto é um polichinelo.

E o motivo de ser do Scooby-Doo? Levando em conta que meu último banco de dados foi com dados de OVNIS e o próximo vai ser com as pinturas dos Bob Ross, a resposta parece mais simples, me divirto com bancos de dados não convenientes, além de ser original me dá uma liberdade diferente.

### OBJETIVO

A ideia no começo foi tentar pegar características dos personagens, como "Quem capturou mais?", "Qual o vilão que apareceu mais vezes seguidas", acredito que mantive com o objetivo inicial, contudo conforme fui pesquisando descobri que a temporada Mystery Incorporation (2011-2013), é realmente muito boa e acabei tentando entender melhor quais são os episódios/temporadas melhor avaliadas da série, afim de poder assistir depois no tempo livre.

O objetivo do QlikSense foi esse, você quer ver Scooby Doo, mas o último que viu foi o de 1969 (que por sinal é o primeiro), por qual devo começar? Quanto tempo devo dedicar? Etc...

### GOOGLE CLOUD BIG QUERY

Admito que em um primeiro momento foi complicado entender bem a plataforma, mas nada que 3 vídeos de indianos não resolvessem. 
Depois de entender melhor como funcionava o BigQuery e ver que não era um monstro de 7 cabeças, foi só deixar o SQL falar por mim, infelizmente por ser um banco de dados mais tranquilo não consegui colocar as Window Functions muito menos os JOINS, contudo como eu comentei antes, cada polichinelo aquece um pouco.

### Episódio Mais Famoso

No começo a ideia era fazer algo simples, se eu fosse assistir um episódio hoje e me importasse de uma forma não saúdavel com a sua nota no IMDB, qual episódio eu assistiria?

Se você achou que era de Scooby-Doo se enganou, porque é de Supernatural. Admito que na hora achei que tinha carregado o banco de dados errado, mas colocando o FORMATO, conseguimos ver que se trata de um CROSSOVER, muito bom por sinal. 

Por motivos de desafio maior, quase todas as Colunas eram STRINGS, ao invés de BOOLEANS, FLOATS e INTEGERS. Contudo com o SAFE_CAST eu não temo nada. Foi necessário definir a coluna do IMDB como float64 para que fosse possível retirar os NULLs, caso contrário era possível os retirar também, porém filtrando com texto, 

Defini também um número de avaliações mínimo para que a nota fosse mais coerente, infelizmente na parte das avaliações tinham alguns outliers, a utilização de um BoxPlot para tratamento dos mesmos via Python é uma saída interessante, contudo um passo de cada vez, no momento estava entendendo BigQuery com SQL, sendo assim defini que seriam maior que 130, que era a base mais ou menos dos melhores episódios


![image](https://github.com/RodrigoBaptistaDias/Projeto-Scooby-Doo-Cloud-e-Qliksense/assets/137357721/3fad4ead-81b2-4b10-be13-2e0043a5df36)




### Melhor Série e Temporada

Aqui eu fiz questão de deixar o fanatismo dos fãs de Supernatural de lado, eu já sabia qual temporada tinha os melhores episódios avaliados, mas nada adianta se o resto da temporada for uma tortura, então decidi ver qual série e temporada detém a melhor nota

Nessa é possível ver que não só a Mystery Incorporation tem as melhores notas, como mantém uma média similar nas duas temporadas, ou seja, eu tinha baixado a primeira temporada e estava baixando a segunda enquanto fazia a análise.

Aqui vemos alguns pontos interessantes, como os mesmos conseguiram ser relevantes em 1970 e conseguem ser relevantes até os dias de hoje, tendo a Scooby-Doo Guess Who? (2020) como a mais nova no meio, apesar de ter poucas avaliações, por ser mais nova, consegue manter uma média muito boa.
Um problema visíviel é baixa com relação a segunda temporada, isso pode ser devido desde a um final de temporada ruim, como falta de divulgação, contudo é algo que sobrepassa as notas, pois mesmo que as temporadas seguintes tenham notas altas acabam por ter números muito menores de avaliações.

![image](https://github.com/RodrigoBaptistaDias/Projeto-Scooby-Doo-Cloud-e-Qliksense/assets/137357721/cd61baa6-b0c2-4467-b752-c8466519e501)


### Maior Número de Capturas

Depois de concluir qual série do Scooby Doo eu iria ver, decidi explorar um pouquinho mais do banco de dados só por brincadeira, quem capturou mais monstros ao longo dessa trajetório? Pergunta simples

Tive algumas dificuldades nesse ponto por não conseguir usar o CROSSTAB do Postgre, porém se não vai de um jeito vai de outro, tive que registrar cada personagem, pois a coluna se dava no formato "Caught.Daphne", e era BOOLEAN, o que dificultava a vida ao extremo, contudo usando COUNTIF e meu melhor amigo SAFE_CAST foi mais tranquilo.

O código não ficou da maneira bonita como eu preferiria, mas o que importa é que saída de dados ficou da melhor maneira possível, bem mais legível do que na forma original.

Para surpresa de ninguém que mais capturou foi o Scooby, o que me impressiona na verdade é ele ter capturado tanto a mais que o Salsicha, sendo que os dois estão sempre juntos.

![image](https://github.com/RodrigoBaptistaDias/Projeto-Scooby-Doo-Cloud-e-Qliksense/assets/137357721/8f556864-e3df-474c-97b1-20b141d6ae0d)

Essas foram as pequenas análises que fiz para explorar um pouco da inteface do BIGQUERY utilizando SQL, continuarei analisando esse banco de dados com outras ideias mais aprofundadas, contudo fiquei bem satisfeito e me senti até confortável em utilizar o BIGQUERY no final do projeto.


### QLIKSENSE

O que eu tentei passar com o dashboard são as informações gerais das séries, tendo como ver em que canal ela passa (ou passava), se é um série, crossover ou filme, tempo médio de duração dos episódios, número de episódios.

Seguindo das notas do IMDB por série, e para melhor visualização ao lado, temos os 5 melhores episódios de cada série, ordenados também pela média do IMDB

Abaixo coloquei as notas por ano, para ter uma ideia dos piores anos (1994 e 2008) e os melhores (1997 e 2010 até 2013), além de ser uma boa visualização para se saber em que época saiu a série quando selecionada

![image](https://github.com/RodrigoBaptistaDias/Projeto-Scooby-Doo-Cloud-e-Qliksense/assets/137357721/0b673162-3261-4eff-8a83-cb823de79c15)



### Demonstração de escolha

![image](https://github.com/RodrigoBaptistaDias/Projeto-Scooby-Doo-Cloud-e-Qliksense/assets/137357721/c5fc633d-cb57-405d-9a64-6239e79a6007)


