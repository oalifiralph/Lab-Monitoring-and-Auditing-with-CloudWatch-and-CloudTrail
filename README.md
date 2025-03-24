<div align="center" style="font-family: Arial, sans-serif; font-size: 20px; line-height: 1.5;">

# ☁️ **Curso DPCN-EDN** ☁️
# 👨🏻‍💻 Solutions Architect Associate  👨🏻‍💻

###

<img src="https://figmaresource.com/wp-content/uploads/2024/05/AWS-Marketplace-Logo-PNG-to-svg-1.svg" width="100" alt="AWS Logo"> 
<img src="https://cloud-icons.onemodel.app/aws/Architecture-Service-Icons_01312023/Arch_Management-Governance/64/Arch_Amazon-CloudWatch_64.svg" width="100" alt="AWS CloudWatch"> 
<img src="https://cloud-icons.onemodel.app/aws/Architecture-Service-Icons_01312023/Arch_Management-Governance/64/Arch_AWS-CloudTrail_64.svg" width="100" alt="AWS CloudTrail">
<img src="https://cloud-icons.onemodel.app/aws/Architecture-Service-Icons_01312023/Arch_Compute/64/Arch_Amazon-EC2_64@5x.png" width="100" alt="AWS EC2">
<img src="https://cloud-icons.onemodel.app/aws/Architecture-Service-Icons_01312023/Arch_Storage/64/Arch_Amazon-Simple-Storage-Service_64.svg" width="100" alt="AWS S3">
<img src="https://cloud-icons.onemodel.app/aws/Architecture-Service-Icons_01312023/Arch_App-Integration/Arch_64/Arch_Amazon-Simple-Notification-Service_64.svg" width="100" alt="AWS SNS">
<img src="https://github.com/oalifiralph/oalifiralph/blob/main/img/EDN-Logo.svg" width="113" alt="Logo-EDN"> 

</div>

# 👨🏻‍🔬 Laboratório: 

## Monitoramento e Auditoria com CloudWatch e CloudTrail

**Turma:** DPCN 07  
**Aluno:** Álifi Ralph 

## 📌 Objetivos:
✅ Criar um alarme para monitorar a CPU de uma instância **EC2** e enviar notificações por e-mail com **CloudWatch e SNS**

✅ Ativar o rastreamento de eventos e armazenar logs em um bucket **S3** com **CloudTrail e S3**

✅ Acessar e analisar os logs gerados com **CloudTraill**

## 💡 Cenário:

Uma startup de tecnologia está migrando sua infraestrutura para a **AWS** e precisa garantir monitoramento eficiente e auditoria detalhada para cumprir normas de conformidade.

- O time de operações quer receber alertas caso a CPU de suas instâncias **EC2** atinja um limite crítico.
- O setor de segurança precisa rastrear todas as atividades na conta **AWS** para detectar ações suspeitas e cumprir requisitos de auditoria.
- Os logs devem ser armazenados com segurança no **S3** para futura análise e conformidade regulatória.

Para atender a esses requisitos, você configurará **CloudWatch**, **SNS**, **CloudTrail** e **S3**, garantindo monitoramento proativo e auditoria contínua.

## 🔧 Pré-requisitos:
- **Conta AWS**: Conta ativa com permissões para **EC2**, **CloudWatch**, **CloudTrail**, **SNS** e **S3**.
- **Permissões IAM**: `CloudWatchFullAccess`, `AWSCloudTrail_FullAccess`, `AmazonSNSFullAccess`, `AmazonS3FullAccess`, `AmazonEC2FullAccess`.
- **Navegador Web**.

---

## Passos para Criar uma Instância de lançamento no EC2:

## 1. Acesse o console do EC2

<img src="https://raw.githubusercontent.com/oalifiralph/Lab-Monitoring-and-Auditing-with-CloudWatch-and-CloudTrail/refs/heads/main/EC2-Launch/EC2-Launch-01.png" width="1000" alt="Dashboard">-

### 1.1. **Crie uma Instância de lançamento**:
   - **Nome**: `Instancia-Teste-CloudWatch`.
   - **AMI**: `Amazon Linux 2 (HVM) - x86_64`.
   

<img src="https://raw.githubusercontent.com/oalifiralph/Lab-Monitoring-and-Auditing-with-CloudWatch-and-CloudTrail/refs/heads/main/EC2-Launch/Launch-EC2.png" width="1000" alt="EC2-Launch">-

### 1.2 **Crie uma Instância de lançamento**:

- **Tipo de instância**: `t2.micro`.
- **Key pair**: Selecione ou crie um par de chaves. (Você precisará acessar a instância, então saiba onde está salvo o seu par de chaves).
- **Configurações de rede**:
     - **VPC**: Sua VPC padrão.
     - **Subnet**: Qualquer sub-rede pública.
     - **Atribuição automática de IP público**: `Habilitar`
   - **Firewall**:
     - Crie um grupo de segurança:
       - **Name**: `SG-Teste-CloudWatch-SeuNome`
       - **Entrada**: `SSH`, Fonte: `Meu IP`

<img src="https://github.com/oalifiralph/Lab-Monitoring-and-Auditing-with-CloudWatch-and-CloudTrail/blob/main/EC2-Launch/Launch-EC2-2.png?raw=true" width="1000" alt="EC2-Launch">-
<img src="https://raw.githubusercontent.com/oalifiralph/Lab-Monitoring-and-Auditing-with-CloudWatch-and-CloudTrail/refs/heads/main/EC2-Launch/Launch-EC2-3.png" width="1000" alt="EC2-Launch">-

### 1.3 **Crie uma Instância de lançamento**:
   - **Configure storage**: `Padrão`
   - **Detalhes avançados**: `Nenhum`

### 1.4 **Revise as Configurações pelo Sumário**.

- **Clique em Instância de lançamento para Criar a Instância**
- **Anote o ID da Instância**.

---

## **Passos para Configurar um Alarme no CloudWatch**

### 2.1 Acesse o console do **CloudWatch**

<img src="https://github.com/oalifiralph/Lab-Monitoring-and-Auditing-with-CloudWatch-and-CloudTrail/blob/main/CloudWatch/CloudWatch-Painel.png?raw=true" width="1000" alt="CloudWatch-Painel">-

### 2.2 **Criando Alarme**:

   - **Alarmes** -> **Criar alarme**.
   - **Selecione a métrica**:
     - **Todas as métricas** -> **EC2** -> **Métricas por instância**.
     - Cole o **ID da instância** (anotada do passo anterior) na caixa de pesquisa.
     - Marque **CPUUtilization** da sua instância.
     - **Métricas gráficas Aba (1)**: Verifique.
     - Selecione a métrica.

<img src="https://github.com/oalifiralph/Lab-Monitoring-and-Auditing-with-CloudWatch-and-CloudTrail/blob/main/CloudWatch/CloudWatch-Metrics.png?raw=true" width="1000" alt="CloudWatch-Metric">-

   - **Especifique a métrica e as condições**:
     - **Nome da Métrica** `Utilização de CPU`
     - **ID**: `i-03421e1e82927bdea`
     - **Estatística**: `Média`
     - **Período**: `5 minutos`
     - **Condições**:
       - **Tipo de limite**: `Estático`
       - **Sempre que CPUUtilization for...**: Maior que...: `70`
      
<img src="https://github.com/oalifiralph/Lab-Monitoring-and-Auditing-with-CloudWatch-and-CloudTrail/blob/main/CloudWatch/Specify-Metric.png?raw=true" width="1000" alt="CloudWatch-Metric">-
       

    - **Configuração adicional**:
    - **Pontos de dados para alarme**: `1 de 1`
    - **Tratamento de dados ausentes**: `trate os dados ausentes como bons (sem ultrapassar o limite).`
       
   - **Próximo**.

### 2.3 **Configurar ações**:

   - **Disparador de estado de alarme**: `Em alarme`
   - **Selecione um tópico SNS**: Crie um tópico SNS (confirme a assinatura).
   - **Próximo**.
4. **Adicione um nome e uma descrição**:
   - **Alarm name**: `AlarmeCPU-Instancia-SeuNome` 
   - **Próximo**.
5. **Visualizar e criar**: Revisar e Criar alarme.



