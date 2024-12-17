## Configuração do Ambiente Cloud9

### Passo 1: Criando o Ambiente no AWS Cloud9

1. Acesse o AWS Cloud9 no Console da AWS em: [AWS Cloud9 Console](https://us-east-1.console.aws.amazon.com/cloud9/).

<img width="506" alt="image" src="https://github.com/user-attachments/assets/2a3c3663-fb91-4852-971c-a84cb51563d8" />

3. Clique em "Criar ambiente" e forneça um nome para o seu ambiente. Para este tutorial, o nome utilizado foi `ponderada09`.

<img width="787" alt="image" src="https://github.com/user-attachments/assets/7bcb76c7-2938-438b-b61a-50047ac46c75" />

4. Escolha o tipo de instância EC2 para o ambiente Cloud9:

<img width="411" alt="image" src="https://github.com/user-attachments/assets/b6cd73b4-bfe0-4e03-9f56-44683a114c89" />

   - Tipo de instância: **t2.micro** (1 GiB RAM + 1 vCPU) – recomendado para pequenos projetos e testes.
6. Escolha a plataforma: **Amazon Linux 2023**.
7. Defina o tempo de inatividade para hibernação automática: **30 minutos**.
8. Configure as opções de rede e escolha **SSH** como a opção de conexão.
9. Após confirmar todas as configurações, clique em "Criar ambiente" para iniciar a criação do ambiente.

### Passo 2: Acessando o Ambiente

- Após a criação, o ambiente estará listado na sua interface do AWS Cloud9. Clique em "Abrir" para acessar o ambiente de desenvolvimento.

<img width="764" alt="image" src="https://github.com/user-attachments/assets/271e9aa3-55a8-476b-a62f-628b43a9d57f" />

Claro! Aqui está o texto adaptado para parecer que foi você quem fez:

---

### Provisionando uma Instância EC2 na AWS com Terraform

**Introdução:**  
Este tutorial tem como objetivo criar uma instância EC2 na AWS utilizando o Terraform, uma ferramenta de Infrastructure as Code (IaC) amplamente utilizada para automatizar a criação e o gerenciamento de infraestrutura em nuvem. O procedimento envolve a instalação do Terraform e da AWS CLI, além de configurar o AWS CLI para autenticação e definir uma instância EC2 básica no Terraform.

#### **Requisitos:**
Para seguir este tutorial, você precisará de:

- Terraform CLI (versão 1.2.0 ou superior)
- AWS CLI instalada
- Conta AWS com credenciais para criação de recursos
- Acesso às credenciais IAM para autenticação via Terraform (configure as variáveis de ambiente `AWS_ACCESS_KEY_ID` e `AWS_SECRET_ACCESS_KEY`)

```bash
export AWS_ACCESS_KEY_ID=seu_access_key
export AWS_SECRET_ACCESS_KEY=seu_secret_key
```

**Dica:**  
Caso você não tenha as credenciais IAM, consulte a documentação do provedor AWS para autenticação alternativa.

#### **Passo 1: Criando o Diretório de Configuração**
Primeiro, crie um diretório para armazenar seus arquivos de configuração do Terraform:

```bash
mkdir learn-terraform-aws-instance
cd learn-terraform-aws-instance
```

Em seguida, crie um arquivo `main.tf` para definir a infraestrutura.

```bash
touch main.tf
```

Abra o arquivo `main.tf` no seu editor de texto e cole a seguinte configuração:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "us-west-2"
}

resource "aws_instance" "app_server" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```

#### **Passo 2: Inicializando o Diretório**
Após criar a configuração, inicialize o diretório para baixar e instalar os provedores necessários (neste caso, o AWS provider):

```bash
terraform init
```

<img width="824" alt="image" src="https://github.com/user-attachments/assets/c7306e91-6a0d-4460-93b2-793f659faa33" />

O Terraform irá instalar o provedor AWS e gerar um arquivo de bloqueio `.terraform.lock.hcl`.

<img width="154" alt="image" src="https://github.com/user-attachments/assets/a51c3df5-ff55-4b31-b06b-02e5d4d64c46" />

#### **Passo 3: Formatando e Validando a Configuração**
A formatação consistente do código é recomendada. Utilize o comando `terraform fmt` para garantir que o código esteja bem formatado.

<img width="200" alt="image" src="https://github.com/user-attachments/assets/5a3a5212-9ef5-45dd-a3b5-826eae6e540f" />

```bash
terraform fmt
```

Em seguida, valide a configuração para garantir que não haja erros de sintaxe ou inconsistências internas:

<img width="296" alt="image" src="https://github.com/user-attachments/assets/725036cf-bebf-42f0-aca5-3fa660bea468" />

```bash
terraform validate
```

<img width="292" alt="image" src="https://github.com/user-attachments/assets/ac66a310-fd72-407b-9db3-25b6626d6f45" />

#### **Passo 4: Criando a Infraestrutura**
Agora que tudo está configurado, execute o comando `terraform apply` para criar a instância EC2 conforme definido no `main.tf`.

```bash
terraform apply
```

Terraform exibirá um plano de execução com as ações que serão realizadas. Quando o plano for aprovado, digite `yes` para confirmar a criação da instância.

```bash
Enter a value: yes
```

Terraform começará a criar a instância e, após alguns minutos, sua instância EC2 estará disponível.

#### **Passo 5: Inspecionando o Estado**
Após a criação da instância, o Terraform armazenará o estado da infraestrutura no arquivo `terraform.tfstate`. Para verificar o estado atual, utilize o comando:

```bash
terraform show
```

Isso exibirá detalhes da instância EC2 criada, incluindo informações como IP público e privado, ID da instância, entre outros.

#### **Passo 6: Gerenciando a Infraestrutura**
Agora que a infraestrutura está criada, você pode gerenciá-la com o Terraform. Para realizar modificações ou destruir a infraestrutura, use os comandos apropriados:

- Para destruir os recursos criados:

```bash
terraform destroy
```

