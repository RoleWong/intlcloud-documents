A partir de 7 de dezembro de 2020, 21:30 (UTC +8), apenas o **faturamento por hora de tráfego** está disponível para usuários novos do CDN. Não é possível alterar o modo de faturamento após a ativação do serviço.
A faixa de preços do faturamento por hora de tráfego é o mesmo do faturamento por dia de tráfego. Consulte [Visão geral do faturamento](https://intl.cloud.tencent.com/document/product/228/2949).

> Observação:
>
> Para obter detalhes sobre o faturamento de nomes de domínio do ECDN, consulte [Visão geral do faturamento](https://intl.cloud.tencent.com/document/product/570/37505).

## Visão geral do faturamento

### Divisão de área

As regiões **dentro da China Continental** e **fora da China Continental** têm planos de faturamento diferentes.

- Na China Continental, as taxas são iguais para todas as regiões.

- As regiões fora da China Continental são divididas em oito áreas de faturamento, de acordo com a localização dos nós de servidores do CDN do Tencent Cloud. São elas: Região 1 da Ásia-Pacífico, Região 2 da Ásia-Pacífico, Região 3 da Ásia-Pacífico, Oriente Médio, Europa, América do Norte, América do Sul e África, conforme mostrado abaixo:

  |  Áreas   |                        Países e regiões abrangidos                        |
  | :------ | :----------------------------------------------------- |
  | Ásia-Pacífico 1 |        Hong Kong (China), Macau (China), Vietnã, Singapura, Tailândia        |
  | Ásia-Pacífico 2 |      Taiwan (China), Japão, Coreia do Sul, Malásia, Indonésia      |
  | Ásia-Pacífico 3 |      Filipinas, Índia, Austrália e outros países e regiões da Ásia-Pacífico       |
  |  Oriente Médio   |              Arábia Saudita, Emirados Árabes Unidos, Turquia               |
  |  Europa   | Reino Unido, Rússia, Alemanha, Itália, Irlanda, França, Holanda, Espanha |
  América do Norte    | Estados Unidos, Canadá                                           |
  |  América do Sul   | Brasil                          |
  |  África   |                          África do Sul                          |

> Observação:
>
> 
>
> As taxas de serviço do CDN para a China Continental e regiões fora da China Continental são cobradas separadamente com base nos preços unitários e usos correspondentes.

### Modo de faturamento

| Modo de faturamento          | Descrição                                                         | Liquidação de fatura                                                     | Regras de faturamento           | Referência                                                     |
| :---------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------- | :----------------------------------------------------------- |
| Faturamento por largura de banda |          As taxas são cobradas com base na largura de banda de pico diária. Os valores de largura de banda são coletados uma vez a cada cinco minutos para gerar um total de 288 pontos estatísticos de largura de banda por dia, entre os quais o mais alto é o pico de largura de banda diário. | É pago conforme o uso diariamente. As taxas incorridas entre 00:00:00 e 23:59:59 de um dia serão cobradas antes das 18:00 do dia seguinte. | Faixa....| [Visão geral do faturamento] |(https://intl.cloud.tencent.com/document/product/228/2949) |
| Faturamento por dia de tráfego | As taxas são cobradas com base no tráfego diário real gerado pelos nós do seu CDN.                     | É pago conforme o uso diariamente. As taxas incorridas entre 00:00:00 e 23:59:59 de um dia serão cobradas antes das 18:00 do dia seguinte. | Faixa cumulativa mensal | [Visão geral do faturamento](https://intl.cloud.tencent.com/document/product/228/2949) |
| Faturamento por hora de tráfego (padrão) | As taxas são cobradas com base no tráfego real por hora gerado pelos nós do seu CDN.         | O pagamento é conforme o uso por hora. As taxas incorridas em uma hora serão cobradas nas 2 a 4 horas seguintes. | Faixa cumulativa mensal | [Visão geral do faturamento](https://intl.cloud.tencent.com/document/product/228/2949) |

### Plano de faturamento de clientes VIP

Se o consumo real ou estimado do serviço do seu CDN for superior a USD 20.000, entre em contato com a equipe de vendas do Tencent Cloud para obter um plano de faturamento mensal mais flexível e econômico.

| Modo de faturamento       | Descrição                                                         | Caso de uso                                                     | Referência                                                     |
| :------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Média diária de pico de largura de banda | Um total de 288 pontos estatísticos de largura de banda do CDN são coletados a cada dia, entre os quais o mais alto é o pico diário de largura de banda. Em seguida, os pontos de pico diário de largura de banda de todos os dias válidos em um mês são calculados para obter a média de pico de largura de banda, ou seja, o valor de largura de banda faturável. Por fim, o valor do resultante é usado para o faturamento com base no preço do seu contrato de serviço. | Se o consumo real ou estimado do serviço do seu CDN for superior a USD 20.000, entre em contato com a equipe de vendas do Tencent Cloud. | [Visão geral do faturamento](https://intl.cloud.tencent.com/document/product/228/2949) |
| 95º percentil mensal        | Um total de 288 pontos estatísticos de largura de banda do CDN são coletados a cada dia, então, os pontos estatísticos de todos os dias válidos em um mês (a partir do 1º dia) são classificados em ordem decrescente. Em seguida, os pontos que representam os 5% mais altos são descartados. Por fim, o ponto estatístico restante com o maior valor é usado como largura de banda para cobrança com base no preço que consta em seu contrato de serviço. | Se o consumo real ou estimado do seu serviço CDN for superior a USD 20.000, entre em contato com o departamento de vendas da Tencent Cloud. | [Visão geral do faturamento](https://intl.cloud.tencent.com/document/product/228/2949) |
| Tráfego mensal         | O tráfego gerado em um mês é acumulado para faturamento com base no preço do seu contrato de serviço.             | Se o consumo real ou estimado do seu serviço CDN for superior a USD 20.000, entre em contato com o departamento de vendas da Tencent Cloud. | /

> Observação:
>
> 
>
> - Cada nó do CDN coleta dados de tráfego em tempo real e os relata ao centro de faturamento, que agrega as entradas de dados aos dados de tráfego total. Para o faturamento por largura de banda, os valores de largura de banda são coletados em granularidade de 5 minutos, ou seja, valor de largura de banda = tráfego total de 5 minutos / 300 s. Por exemplo, se o tráfego total gerado em 5 minutos for 30 MB, a largura de banda correspondente será (30 * 8) / 300 = 0,8 Mbps (1 MB = 8 Mbps).
> - Conversão de base: 1 GB = 1.000 MB; 1 MB = 1.000 KB; 1 Gbps = 1.000 Mbps; 1 Mbps = 1.000 Kbps.

## Detalhes de faturamento

### Faturamento por largura de banda de pico

A largura de banda do CDN adota o **preço escalonado** da seguinte forma:

| Faixa de largura de banda (USD/Mbps/dia) | China Continental (CN) | América do Norte (NA) | Europa (EU) | Região 1 da Ásia-Pacífico (AP1) | Região 2 da Ásia-Pacífico (AP2) | Região 3 da Ásia-Pacífico (AP3) | Oriente Médio (ME) | África (AA) | América do Sul (SA) |
| :----------------------- | :------------ | :-------- | :-------- | :------------ | :------------ | :------------ | :-------- | :-------- | :-------- |
| 0 Mbps a 500 Mbps          | 0,0815         | 0,2069    | 0,2069    | 0,3647        | 0,3928        | 0,5140        | 0,7391    | 0,5612    | 0,5612    |
| 500 Mbps a 5 Gbps          | 0,0800         | 0,1964    | 0,1964    | 0,3216        | 0,3402        | 0,4679       | 0,6754   | 0,5137    | 0,5137    |
| 5 Gbps a 50 Gbps           | 0,0754         | 0,1491    | 0,1491    | 0,2703        | 0,2859        | 0,3828        | 0,6075    | 0,4702    | 0,4702    |
| ≥ 50 Gbps                 | 0,0738       | 0,1055    | 0,1055    | 0,2436        | 0,2545        | 0,3267           | 0,5301    | 0,4281       | 0,4281       |

> Observação:
>
> 
>
> Se sua largura de banda de pico do CDN for igual ou superior a 50 Gbps, [entre em contato conosco](https://intl.cloud.tencent.com/contact-sales) para obter informações sobre descontos.

#### Método de cálculo

Supondo que a largura de banda de pico do CDN na China Continental seja X e não haja consumo fora dela, o método de cálculo para os preços escalonados é o seguinte:

1. Se X < 500 Mbps, o valor da fatura será X * 0,0815.
2. Se 500 Mbps ≤ X < 5.000 Mbps, o valor da fatura será X * 0,0800.
3. Se 5.000 Mbps ≤ X < 50.000 Mbps, o valor da fatura será X * 0,0754.
4. Se X ≥ 50.000 Mbps, entre em contato conosco para contratação offline. Temos mais opções de desconto para você.

Você pode usar a [Calculadora de preços](https://buy.cloud.tencent.com/calculator/cdn) para obter uma estimativa de valores.

### Faturamento por tráfego (por hora e por dia)

O preço do tráfego do CDN é baseado em uma **faixa cumulativa mensal** da seguinte forma:

| Faixa de tráfego (USD/GB) | China Continental (CN) | América do Norte (NA) | Europa (EU) | Região 1 da Ásia-Pacífico (AP1) | Região 2 da Ásia-Pacífico (AP2) | Região 3 da Ásia-Pacífico (AP3) | Oriente Médio (ME) | África (AA) | América do Sul (SA) |
| :------------------ | :------------ | :-------- | :-------- | :------------ | :------------ | :------------ | :-------- | :-------- | :-------- |
| 0 TB a 2 TB           | 0,0323         | 0,0452    | 0,0452    | 0,0665        | 0,0798        | 0,0897          | 0,1080    | 0,1039      | 0,1039      |
| 2 TB a 10 TB          | 0,0308         | 0,0378    | 0,0378    | 0,0592        | 0,0737        | 0,0780        | 0,1000    | 0,0970    | 0,0970    |
| 10 TB a 50 TB         | 0,0277         | 0,0319    | 0,0319    | 0,0533        | 0,0677        | 0,0723        | 0,0940    | 0,0907    | 0,0907    |
| 50 TB a 100 TB        | 0,0231         | 0,0261    | 0,0261    | 0,0475        | 0,0590        | 0,0654        | 0,0863    | 0,0842    | 0,0842    |
| ≥ 100 TB             | 0,0169          | 0,0200    | 0,0200    | 0,0446        | 0,0503        | 0,0577        | 0,0794    | 0,0781    | 0,0781    |

> Observação:
>
> 
>
> Se o tráfego do CDN for igual ou superior a 100 TB, [entre em contato conosco](https://intl.cloud.tencent.com/contact-sales) para obter informações sobre descontos.

#### Método de cálculo

Ao contrário do faturamento por largura de banda, o faturamento por tráfego é baseado em uma faixa cumulativa mensal. Veja abaixo um exemplo que descreve como funciona o modo de faturamento por dia de tráfego:

- Conforme mostrado na figura abaixo, suponha que o tráfego gerado na China Continental em 1º de janeiro seja de 3 TB e não haja consumo fora da China Continental. A zona cinza representa a faixa de faturamento real e a zona verde mostra o tráfego gerado em 1º de janeiro. Como 2 TB está na faixa de faturamento de 0 TB a 2 TB e o 1 TB restante está na faixa de 2 TB a 10 TB, as taxas reais para 1º de janeiro serão 2 * 1.000 * 0,0323 + 1 * 1.000 * 0,0308.
  ![img](https://mc.qcloudimg.com/static/img/bfdae242f6cca57421a65e46a96b0c67/image.png)
- Conforme mostrado na figura abaixo, suponha que o tráfego gerado na China Continental em 2 de janeiro seja também de 3 TB e não haja consumo fora da China Continental. Como o faturamento por tráfego é baseado no tráfego cumulativo mensal, todos os 3 TB estão na faixa de 2 TB a 10 TB, então as taxas reais para 2 de janeiro serão 3 * 1.000 * 0,0308.
  ![img](https://mc.qcloudimg.com/static/img/f62d1056c1c2cab249cec62ad6e74ddc/image.png)
- Conforme mostrado na figura abaixo, suponha que o tráfego gerado na China Continental em 3 de janeiro seja de 7 TB e que não haja consumo fora da China Continental. Desses 7 TB, 4 TB estão na faixa de 2 TB a 10 TB e os 3 TB restantes estão na faixa de 10 TB a 50 TB, então as taxas reais para 3 de janeiro serão 4 * 1.000 * 0,0308 + 3 * 1.000 * 0,0277.
  ![img](https://mc.qcloudimg.com/static/img/954e2d483e31afd411f9a91ebd7f66c8/image.png)

Desta forma, é possível calcular a taxa para cada dia em um mês. Quando for 1º de fevereiro, o consumo será acumulado partindo de 0 para o cálculo do faixa. Você pode usar a [Calculadora de preços](https://buy.cloud.tencent.com/calculator/cdn) para obter uma estimativa de valores.

### Faturamento por média de pico de largura de banda

1. Suponha que o faturamento do CDN comece oficialmente em 1º de janeiro e o preço contratado seja P USD/Mbps/mês.
2. Um dia válido se refere a um dia em que mais de 0 Kbps de largura de banda é consumido.
3. Supondo que haja 14 dias válidos em janeiro, consideramos o valor máximo dos 288 pontos estatísticos para cada um desses 14 dias como Max_1, Max_2, Max_3…e Max_14, respectivamente. A largura de banda faturável será Média (Max_1, Max_2, Max_3...e Max_14), então a taxa de janeiro será: Média (Max_1, Max_2, Max_3…e Max_14) * P * 14 / 31.

### Faturamento por 95º percentil mensal de largura de banda

1. Suponha que o faturamento do CDN comece oficialmente em 1º de janeiro e o preço contratado seja P USD/Mbps/mês.
2. Um dia válido se refere a um dia em que mais de 0 Kbps de largura de banda é consumido.
3. Supondo que haja 14 dias válidos em janeiro, 14 * 288 pontos estatísticos serão coletados e classificados em ordem decrescente. Os 5% dentre os pontos estatísticos mais altos são descartados, de modo que Max95 é o ponto mais alto nos pontos estatísticos restantes, que é a largura de banda faturável. A taxa para janeiro é: Max95 * P * 14 / 31.

## Escolha de um modo de faturamento

> Observação:
>
> 
>
> Se você percebeu que o modo de faturamento escolhido não é adequado para suas necessidades de negócios durante o uso, é possível alterá-lo. Para obter mais informações, consulte [Alteração do modo de faturamento](https://intl.cloud.tencent.com/document/product/228/32326).

**Opções:**
O CDN oferece dois modos de faturamento: o **faturamento por tráfego** e o **faturamento por largura de banda**. É possível escolher o modo de faturamento conforme necessário.

**Exemplo de cálculo:**
Supondo que o tráfego consumido entre 0h e 23h59min de ontem foi de 200 GB, e não houve consumo nas regiões fora da China Continental, a largura de banda de pico foi de 40 Mbps conforme a curva abaixo:
![img](https://mc.qcloudimg.com/static/img/3ecfe86a031782ebeaf0b1f7595cc69f/image.png)

Se você escolher o modo de faturamento por tráfego, precisará pagar: 200 * 0,037 = 7,4 USD

Se você escolher o modo de faturamento por largura de banda, precisará pagar: 40 * 0,094 = 3,76 USD 

Nesse exemplo específico, o faturamento por largura de banda é um modelo mais econômico.

## Serviços de valor agregado

O suporte QUIC é um serviço de valor agregado que é cobrado com base no número de solicitações QUIC.

### Faturamento de solicitação QUIC

Para obter detalhes sobre o faturamento de solicitação QUIC, consulte [QUIC](https://intl.cloud.tencent.com/zh/document/product/228/39746).

> Observação:
>
> - Você só é cobrado quando há solicitações QUIC.

| Item faturável | Preço unitário (USD/10 mil solicitações) | Método de cobrança | Ciclo de Liquidação |
| :---------- | :---------------- | :--------- | :------- |
| Solicitação QUIC | 0,007 | Pagamento conforme o uso | Liquidação por hora |

## Em atraso

O Tencent Cloud vai notificá-lo por meio de vários canais, incluindo e-mail e SMS se você tiver pagamentos em atraso. É disponibilizado um período de carência de 24 horas. Se você não conseguir recarregar sua conta em 24 horas, o serviço do CDN será suspenso. Depois que sua conta for recarregada, o status do nome de domínio será automaticamente restaurado para o status anterior à suspensão do serviço (este processo leva algum tempo).

> Observação:
>
> 
>
> Quando o seu serviço de aceleração for suspenso por atraso no pagamento, todos os seus nomes de domínio serão desabilitados e todas as solicitações de acesso serão encaminhadas ao servidor de origem. Você pode consultar informações, mas não pode modificar as configurações no console do CDN. Seus nomes de domínio e as configurações relacionados ao CDN serão mantidos por 12 meses.