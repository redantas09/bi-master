<!-- antes de enviar a versão final, solicitamos que todos os comentários, colocados para orientação ao aluno, sejam removidos do arquivo -->
# Nome do projeto

#### Aluno: [Renata Aparecida Dantas Carneiro de Mesquita](https://github.com/link_do_github)
#### Orientador: [Anderson nascimento](https://github.com/link_do_github).

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

<!-- para os links a seguir, caso os arquivos estejam no mesmo repositório que este README, não há necessidade de incluir o link completo: basta incluir o nome do arquivo, com extensão, que o GitHub completa o link corretamente -->
---

### Resumo

<!-- trocar o texto abaixo pelo resumo do trabalho, em português -->

O presente trabalho tem como objetivo coletar, analisar e definir as principais necessidades do projeto de construção de uma visão de atendimento relacionada a cronograma de atividades de sonda. 
O trabalho passa pela descrição do modelo transacional e percorre todo o processo de BI, com a definição do modelo multidimensional, elaboração do Data Warehouse, projeto de ETL e elaboração de dashboards.

### 1. Introdução

O estudo de caso está sendo realizado com base em uma área responsável pela programação de atividades de sonda, o que inclui perfuração, completação, avaliação e manutenção (workover) de poços de petróleo.

A área necessita verificar e acompanhar o atendimento às datas de entrada em produção de poços novos, o atendimento a datas de integridade para aqueles poços que possuam questões de integridade a serem solucionadas e consequentemente um prazo para isso. Necessita também acompanhar a situação de prontidão para realização das atividades programadas, considerando cada uma das disciplinas que atuam no atendimento e a situação final de prontidão. A área deseja, então, ter uma visão completa do atendimento, considerando o cronograma elaborado, que permita ter um acompanhamento direto e detalhado da situação atual e com isso identificar necessidades de ajustes e ter uma tomada de decisão mais rápida e assertiva. 

O projeto irá trazer sondas como fato a ser estudado, levando em consideração as dimensões de prontidão SUB (equipamentos e serviços submarinos), prontidão SPO (equipamentos e serviços da disciplina de Poços), cronograma de atividades, base de poços, integridade, entrada de poços novos e data.O objetivo principal é unir todos os dados dimensões com os dados do fato e, com isso, obter as informações relevantes para atender às necessidades da área.

Deseja-se que o projeto permita obter relatórios com uma visão geral das atividades, com a visão de atendimento às datas limites de poços com situações relacionadas a integridade, com visão de atendimento às datas de entrada de poços novos e com a situação de prontidão para realização das atividades.

Por questões de sigilosidade dos dados está sendo usada no presente trabalho uma base de dados fictícia. 

### 2. Modelagem

A realização do projeto teve início com a descrição do modelo transacional. Foram usadas como fontes de dados planilhas em Excel.
Através dos dados obtidos foi possível gerar o modelo transacional conforme o diagrama a seguir:
![image](https://github.com/user-attachments/assets/f4d19418-e5b6-4b95-a1cc-43600cb7f9ae)

Em seguida foi montado o processo de BI, que propõe a criação de um banco de dados em SQL com a replicação do modelo transacional.
Porpõe-se a realização do ETL de forma semanal, uma vez que, conforme levantado com os usuários, essa é menor frequência na qual é realizada a atualização dos dados, com a carga das dimensões prontidão SUB, prontidão SPO, cronograma de atividades, base de poços, integridade e entrada de poços novos, executada através do software Pentaho Data Integration (PDI), onde um JOB será responsável pela atualização do Data Warehouse. A partir dos dados do Data Warehouse serão gerados dashboards, utilizando o Power BI, com as informações relevantes para as necessidades da empresa.

Na sequência, foi definido o modelo multidimensional referente ao presente projeto, o qual foi desenvolvido considerando o processo de análise das necessidades da área. Nele, constam as dimensões prontidão SUB, prontidão SPO, cronograma de sondas, base de poços, integridade, entrada de poços novos e data, conforme representado abaixo, figura extraída do Power Architect.

![image](https://github.com/user-attachments/assets/002a6ba0-e5a9-4548-9693-a8c137a50b01)

Prosseguiu-se, então, para a elaboração do Data Warehouse, que será a fonte integradora de informações da área. A arquitetura do DW será Global e Centralizada, pois embora outras áreas atuem no processo, o controle da programação é feito pela área em questão. Como a arquitetura será Global e Centralizada não serão construídos Data Marts, desta forma o processo de construção levará em conta apenas a construção do DW. Além disso, todo o projeto será criado para arquitetura chamada On-Premises, ou seja, o DW ficará armazenado em um servidor próprio da empresa, localizado em seu Datacenter particular.

O passo seguinte foi a descrição do projeto de ETL, sendo utilizado o software Pentaho Data Integration (PDI) para auxiliar sua criação. Foram realizadas a extração dos dados (Table input) das dimensões prontidão SUB, prontidão SPO, cronograma de sondas, base de poços, integridade e entrada de poços novos. Primeiramente foram realizados os ETLs do transacional e posteriormente o carregamento nas dimensões no DW, conforme mostrado nas figuras abaixo (nem todos as imagens estarão com o check em verde pois foi a foto não foi tirada no momento em que foi rodado). Em geral, foram utilizados os tratamentos de “string operations” para retirar possíveis espaços e formatar os dados em maiúsculo/minúsculo, "if field value is null” para substituir campos nulos, “concat Fields” para inclusão de uma coluna de status detalhado que concatena duas colunas existentes (status e identificação problema) “e “select values” para ordenar e renomear colunas. 

Dimensão prontidaospo: 
![image](https://github.com/user-attachments/assets/80726113-a02e-4201-8605-5b924b3a1bb0)
![image](https://github.com/user-attachments/assets/f1065ff3-c458-4752-a23a-391a8c8f71d5)

Dimensão prontidaosub:
![image](https://github.com/user-attachments/assets/043dc0b7-e62c-4464-8847-edc7015137eb)
![image](https://github.com/user-attachments/assets/74ad6fc2-a5c8-4beb-9eab-685ca96f7af8)

Dimensão cronosondas:
![image](https://github.com/user-attachments/assets/911978ac-91ec-4fe3-9bea-3f199bfb16ed)
![image](https://github.com/user-attachments/assets/1e3d4aa7-38e9-4ffb-ab4a-e54f18116659)

Dimensão basepocos:
![image](https://github.com/user-attachments/assets/90b13cd3-f889-430f-9952-d35157dd96d5)
![image](https://github.com/user-attachments/assets/7cb883bb-0e7c-431c-a92c-2d1e89939166)

Dimensão integridade:
![image](https://github.com/user-attachments/assets/e17ecf12-62e5-40cc-be2a-fad6a4657e82)
![image](https://github.com/user-attachments/assets/141173c1-21ce-48f2-a620-fff3dabb53ec)

Dimensão entradapocosnovos:
![image](https://github.com/user-attachments/assets/d85c47a0-0e1a-41bb-8947-385da396afa3)
![image](https://github.com/user-attachments/assets/40fd04d1-4195-46c5-aa3b-4fe8d8d245d2)

No caso da tabela fato sondas, foi realizado o carregamento no Data Warehouse (Dimension look-up/updade e Table output), utilizando comparação para não haver duplicação de dados (Delete).
![image](https://github.com/user-attachments/assets/dfcfacf4-5302-42d3-b1ad-3614b4879212)

Finalmente, foram elaborados, através do Power BI, os dashboards para atendimento às necessidades da área, cujos resultados serão apresentados no item seguinte.


### 3. Resultados

Com objetivo principal de fornecer as informações atualizadas e alinhadas com o plano e requisitos da área, as telas dos dashboards não só atendem as informações requeridas como aprofundam a um nível de detalhe de modo que a gestão tome ação baseada em evidência sobre não atendimento de questões de integridade, não atendimento de datas de entrada de poços novos e sobre situações de falta de prontidão para realização das atividades.

O primeiro dashboard elaborado foi o de visão geral do cronograma de atividades de sondas. Nesta tela é possível visualizar o total de dias de sonda no período, a quantidade de sondas equivalentes (cálculo da quantidade de sondas necessária para atendimento do total), a visão de dias de sonda por tipo de tarefa, a visão de dias de sonda por campo e uma tabela com as principais informações do cronograma. O filtro de ano possibilita selecionar o(s) ano(s) desejado(s).
![image](https://github.com/user-attachments/assets/cc011c96-5418-4491-b319-f456f47002dc)



### 4. Conclusões

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

---

Matrícula: 123.456.789

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
# bi-master
