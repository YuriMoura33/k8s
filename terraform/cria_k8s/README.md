# Este terraform foi criado para criação do cluster Kubernetes no ambiente Oracle CLOUD. Lembre-se de ler as instruções de pré-requisitos em:

https://github.com/YuriMoura33/k8s/blob/main/aula1/Requisitos 

## 
Uma vez configurado os pré requisitos, conecte com sua [OCI credentials](https://learn.hashicorp.com/tutorials/terraform/oci-build?in=terraform/oci-get-started) e obtenha um token de acesso com o comando `oci session authenticate`). Tenha certeza da região correta que está trabalhando, ( a mesma usada na criação da conta OCI). 

Faça o download do código disponibilizado e na pasta onde o código foi baixado execute: 


1. `terraform init` 
2. `terraform plan` 
8. `terraform apply`
E é isso. 

Aguarde a criação da infraestrutura. 

Ao final do comando `terraform apply` o arquivo `kubeconfig` é gerado. 

Para se conectar ao cluster e validar se está correto faça o seguinte: 

Linux
```bash
export KUBECONFIG=$PWD/kubeconfig
kubectl get nodes
```

Windows
```powershell
$env:KUBECONFIG="$pwd\kubeconfig"
kubectl get nodes
```

O comando deve mostrar criado 4 nós (`node1` to `node4`.)

Você também pode fazer login nas VMs. No final da saída do Terraform você deve ver um comando que pode usar para SSH na primeira VM
(basta copiar e colar o comando gerado)

## Windows

Funciona com Windows 10/Powershell 5.1.

Pode ser necessário alterar a política de execução para irrestrita.

[PowerShell ExecutionPolicy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-5.1)

## Personalização

Verifique `variables.tf` para ver parâmetros ajustáveis. Você pode alterar o número
de nós, o tamanho dos nós ou alternar para instâncias Intel/AMD se desejar
. Lembre-se de que, se alternar para instâncias Intel/AMD, não obterá
vantagem do nível gratuito.

## Parando o cluster

`terraform destroy`

## Detalhes da implementação

Esta configuração do Terraform:

- gera um par de chaves OpenSSH e um token kubeadm
- implementa 4 VMs usando o Ubuntu 20.04
- usa o cloud-init para instalar e configurar tudo
- instala pacotes Docker e Kubernetes
- executa `kubeadm init` na primeira VM
- executa `kubeadm join` nas outras VMs
- instala o plugin Weave CNI
- transfere o arquivo `kubeconfig` gerado pelo `kubeadm`
- corrige esse arquivo para usar o endereço IP público da máquina

## Advertências

Isso não instala o [gerenciador do controlador de nuvem OCI][ccm],
o que significa que você não pode
criar serviços com `type: LoadBalancer`; ou melhor, se você criar
tais serviços, seu `EXTERNAL-IP` permanecerá `<pending>`.

Para expor serviços, use `NodePort`.

Da mesma forma, não há controlador de entrada nem classe de armazenamento.

Eles podem ser adicionados em uma iteração posterior deste projeto.
Enquanto isso, se você quiser instalá-lo manualmente, pode verificar
o [repositório github do gerenciador de controlador de nuvem OCI][ccm].

## Observações

O Oracle Cloud também tem um serviço Kubernetes gerenciado chamado
[Container Engine for Kubernetes (ou OKE)][oke]. Esse serviço
não tem as ressalvas mencionadas acima; no entanto, não faz parte
do nível gratuito.

## O que significa "Ampernetacle"?

É um *porte-manteau* entre Ampere, Kubernetes e Oracle.
Provavelmente não é o melhor nome do mundo, mas é o
que temos! Se você tiver uma ideia para um nome melhor, avise-nos. 😊

## Possíveis erros e como lidar com eles

### Problema de autenticação

Se você configurou a autenticação OCI usando um token de sessão
(com `oci session authenticate`), observe que esse token
é válido por 1 hora por padrão. Se você autenticar, esperar mais
de 1 hora e tentar `terraform apply`, você obterá
erros de autenticação.

#### Sintoma

A seguinte mensagem:

```
Erro: 401-NotAuthenticated
│ Serviço: Identity Compartment
│ Mensagem de erro: As informações necessárias para concluir a autenticação não foram fornecidas ou estavam incorretas.
│ ID da solicitação OPC: [...]
│ Sugestão: tente novamente ou entre em contato com o suporte para obter ajuda com o serviço: Identity Compartment
```

#### Solução

Autenticar ou reautenticar, por exemplo, com
`oci session authenticate`.

Se for solicitado o nome do perfil, certifique-se de digitar `DEFAULT`
para que o Terraform use automaticamente o token de sessão.

Se você usou anteriormente `oci session authenticate`, você
deve ser capaz de atualizar a sessão com
`oci session refresh --profile DEFAULT`.

### Problema de capacidade

#### Sintoma

Se você receber uma mensagem como a seguinte:
```
Erro: 500-InternalError
│ ...
│ Serviço: Instância principal
│ Mensagem de erro: Capacidade do host esgotada.
```

Isso significa que não há servidores suficientes disponíveis no momento
no OCI para criar o cluster.

#### Solução

Uma solução é alternar para um *domínio de disponibilidade* diferente.
Isso pode ser feito alterando a variável de entrada `availability_domain`. (Obrigado @uknbr pela contribuição!)

Observação 1: algumas regiões têm apenas um domínio de disponibilidade. Nesse
caso, você não pode alterar o domínio de disponibilidade.

Nota 2: As contas OCI (especialmente as contas gratuitas) são vinculadas a uma
região única, então se você tiver esse problema e não puder alterar o
domínio de disponibilidade, você pode [criar outra conta][criar conta].

### Usando a região errada

#### Sintoma

Ao fazer `terraform apply`, você recebe esta mensagem:

```
oci_identity_compartment._: Criando...
╷
│ Erro: 404-NotAuthorizedOrNotFound
│ Serviço: Identity Compartment
│ Mensagem de erro: Falha na autorização ou recurso solicitado não encontrado
│ ID da solicitação OPC: [...]
│ Sugestão: O recurso foi excluído ou o serviço Identity Compartment precisa de uma política para acessar este recurso. Referência de política: https://docs.oracle.com/en-us/iaas/Content/Identity/Reference/policyreference.htm
│
│
│ com oci_identity_compartment._,
│ na linha 1 do main.tf, no recurso "oci_identity_compartment" "_":
│ 1: recurso "oci_identity_compartment" "_" {
│
╵
```

#### Solução

Edite `~/.oci/config` e altere a linha `region=` para colocar a região correta.

Para saber qual é a região correta, você pode tentar fazer login em
https://cloud.oracle.com/ com sua conta; após fazer login,
você deve ser redirecionado para uma URL que se parece com
https://cloud.oracle.com/?region=us-ashburn-1 e nisso
e
