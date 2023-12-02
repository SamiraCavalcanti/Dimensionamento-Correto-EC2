# Laboratório 


# Dimensionamento Correto da Instância EC2   
![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/11bf554c-f351-4323-afe9-d4ee6f0a8285)

Este laboratório demonstra o procedimento para identificar as oportunidades de dimensionamento correto para otimização     
de custos em duas instâncias do Amazon Elastic Compute Cloud (Amazon EC2), em que você adota configurações mais adequadas    
para a utilização de recursos de carga de trabalho simulada com base nas métricas observadas do Amazon CloudWatch.   

# OBJETIVOS
Ao final deste laboratório, você deverá ser capaz de fazer o seguinte:    

• Instalar e configurar o agente do CloudWatch em instâncias do Amazon EC2.     
• Executar comandos interativos em instâncias do Amazon EC2 para simular carga de memória e CPU.    
• Revisar as métricas da instância do Amazon EC2 usando um painel do CloudWatch.    
• Modificar os tamanhos ou tipos de instância do Amazon EC2 com base nas métricas do CloudWatch observadas.    
• Configurar alarmes do CloudWatch   

# TAREFA 1.1: INSTALAR O CLOUDWATCH AGENT EM SUAS INSTÂNCIAS DO AMAZON EC2 DA APLICAÇÃO WEB
Nesta tarefa, vamos instalar o CloudWatch Agent nas instâncias da aplicação web.
Usaremos o AWS Systems Manager para executar os comandos de instalação e configuração para 
ambas as instâncias do EC2 simultaneamente, por meio do grupo de recursos já criado para este laboratório.

Comando executado através do AWS System Manager para instalar o CloudWatch Agent nas instâncias da aplicação web

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/b503af02-0617-490b-a3fe-5a2e3ca63244)

# TAREFA 1.2: CONFIGURAR E INICIAR AGENTES DO CLOUDWATCH NO GRUPO DE RECURSOS WEB-APPLICATION-INSTANCES
Comando executado, no arquivo(JSON) de configuração que foi pré-criado e armazenado no Repositório de parâmetros do Systems Manager
especifica as métricas e os logs que o agente deve coletar, incluindo métricas personalizadas.

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/35f8bb98-15b2-461e-8913-8ff136123056)

# Tarefa 2: Fazer teste de estresse do servidor web e analisar as métricas da CPU
Depois da insnstalção e configuração do CloudWatch Agent nas instâncias do EC2,    
vamos simular o uso e examinar as métricas de desempenho usando o stress-ng    
uma ferramenta para carregar e aplicar estresse à sua instância do EC2 do servidor web. 

- Primeiro conectamos a instaência web-Server
- usando o comando cd ~ && stress-ng --version, navegamos para o diretório inicial e validamos a instalação da ferramenta stress-ng
- usando o comando stress-ng --cpu 1 carregarregamos e aplicamos estresse à instância do Web-Server

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/65a8c2ce-49b5-4255-9d11-d5d909d744a7)

Aqui examinamos a utilização da CPU para sua instância do Web-Server e analisá-la usando o CloudWatch.
No CloudWatch dois painéis com no nome EC2_Metric_Comparison  ja tinham sido criados .

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/d55e3be8-bc58-429e-9c9f-fe97fa0114c6)

Teste de stress com 3 CPUs

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/2fdb3e4b-583d-4c8e-b822-efa0675ae8a2)

Resultado 

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/6c1e02a2-9b8f-4061-84aa-f9bf7f3d9ce9)


** Com a carga de trabalho relatando no primeiro 25% de utilização de CPU, você pode concluir que a instância do Web-Server
está superprovisionada na área da CPU. O custo contínuo da t3.xlarge pode ser otimizado selecionando um tamanho de 
instância menor que forneça menos recursos de CPU.

# Tarefa 3: Redimensionar e fazer o teste de estresse no servidor web outra vez para analisar as métrica de CPU

TAREFA 3.1: REDIMENSIONAR O WEB-SERVER
Depois de interromper a instância o alterei seu tipo de t3.xlarge que tem 4 vCPUs para t3.medium com 2 vCPUs

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/cc25526e-967b-4def-92ef-8987b9dc09d8)

# TAREFA 3.2: FAZER TESTE DE ESTRESSE DO WEB-SERVER OUTRA VEZ PARA ANALISAR AS MÉTRICAS DE CPU
** Considere: para uma aplicação real, considere esta uma oportunidade de executar mais testes para garantir boas características de desempenho.

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/909a7252-60d4-4ed8-b970-4eb8a4780062)



# Tarefa 4: Criar alarmes de utilização da CPU
Agora que alteramos o tamanho da instância do servidor web, é importante monitorar a métrica 
de utilização da CPU para garantir que a instância não seja subutilizada no futuro. 
Para facilitar isso, você configura um alarme do CloudWatch para monitorar a utilização da CPU.

TAREFA 4.1: ALARME PARA UTILIZAÇÃO EXCESSIVA DA CPU      
![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/676f6692-2e10-4c27-b726-c094d72b298c)

TAREFA 4.2: ALARME PARA CPU SUBUTILIZADA

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/5d6494d8-c3fe-49ef-a6c6-4d400357e849)

# TAREFA 4.3: ADICIONAR ALARMES AO PAINEL
Agora que criamos dois alarmes, vamos adicional ao painel para começar a desenvolver uma só visualização sobre a integridade da aplicação.

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/877b9481-1ed7-4ed1-b82b-fb98d02a1df6)

# Tarefa 5: Testar a carga do servidor do banco de dados e analisar as métricas de memória
Usaremos o comando head para simular a utilização de memória e analisar as análises de memória para sua instância do DB-Server.      

O comando head -c 10G /dev/zero | tail  Simula a utilização de memória alocando 10 GB de informações de /dev/zero e enviando-as para o programa final. 

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/b1939c0d-ab34-4afb-bc5d-0934cfe60764)

** Depois de simular o uso de memória com os comandos head e tail, o uso de memória na instância do servidor de banco de dados é próximo de 70%.

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/c3220a51-2417-494c-a780-5058b9f0812a)

# Tarefa 6: abranger a instância do DB-Server e analisar as análises de memória
Depois de analisar as análises do DB-Server, você observa que, embora o uso de memória não caiba em um tamanho de instância menor, 
o uso de CPU foi mínimo e pode ser redimensionado. Isso representa um potencial para encontrar um tipo de instância melhor de acordo 
com o uso do servidor de banco de dados.

TAREFA 6.1 ANALISAR OS TIPOS DE INSTÂNCIA PROFISSIONAL
Filtrando os tipos de instâncias na minha região por arquitetura ,vCPUs e memória

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/bf0661cc-79c7-438a-a1b8-14cc6ac58d2c)

Depois de interromper a instância alterei seu tipo  t3.xlarge para r5.large que proporciona       
uma vantagem de custo de ~24% com relação à t3.xlarge para essa carga de trabalho.      

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/72485841-3c35-4e72-9f0a-cf56587e9dd8)


# TAREFA 6.3: SIMULAR A UTILIZAÇÃO DE MEMÓRIA NA INSTÂNCIA DO DB-SERVER E ANALISAR AS MÉTRICAS DE MEMÓRIA OUTRA VEZ

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/6f3f8b5b-067f-4cc5-8f89-2aba8d2a6e50)


# Tarefa 7: Criar alarmes de utilização de memória
Agora que otimizamos a instância do servidor de banco de dados, é importante monitorar a métrica de utilização da       
memória para garantir que a instância não seja utilizada demais nem de menos no futuro. Para facilitar isso, configuramos      
um alarme do CloudWatch para monitorar a utilização da memória.

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/a121ce07-f3a0-47fe-a43a-3e284a344db1)


TAREFA 7.2: CRIAR UM ALARME COMPOSTO
Criamos um alarme composto que combina o alarme de utilização de memória que criamos anteriormente para         
DB-Server e o alarme de superutilização da CPU para Web-Server. Isso permite monitorar problemas de desempenho        
em várias instâncias do EC2 com um único alarme      

![image](https://github.com/SamiraCavalcanti/Dimensionamento-Correto-EC2/assets/86758007/2eb2035e-a42a-42fc-9192-6d17aeecc5c2)


# Conclusão
Você realizou as seguintes tarefas:

Instalou e configurou o agente do CloudWatch em instâncias do Amazon EC2.       
Executou comandos interativos em instâncias do Amazon EC2 para simular carga de memória e CPU.      
Revisou as métricas da instância do Amazon EC2 utilizadas com um painel do CloudWatch.      
Modificou os tamanhos ou tipos de instância do Amazon EC2 com base nas métricas do CloudWatch observadas.        
Configurou alarmes do CloudWatch.          




























