# K8S Introdução 
__________________________________________________

Sejam bem vindos ao treinamento de K8S. 

O objetivo principal aqui é o aprendizado e troca de conhecimentos constantes. Por isso deixo algumas dicas principais para o melhor aproveitamento. São elas: 

1. Sejam curiososos. A curiosidade leva ao bom aprendizado;
2. Não tenham medo de errar, o errar é aprendizado;
3. Existem mais de uma solução para o mesmo problema. Não se culpem se a solução usada não for a "mais bonita" e nem "a mais rápida". Importem-se em aprender e evoluir. Soluções melhores e refinadas são fruto de muito treino e experiência.
4. Ninguém nasce sabendo, e portanto nenhuma pergunta ou questionamento é errado. Usem e abusem. Se não entenderem perguntem quantas vezes for necessário.

Lembrem-se sempre do mais importante: se o coração e a mente estiverem abertos, o aprendizado é consequencia!!!! 

# K8S - AULA 1 
__________________________________________________
O objetivo desta primeira etapa é prepararmos o material que vamos precisar para as aulas. Todos os scripts, desenvolvimentos e materiais deixarei disponivel aqui no github. Usem e abusem do material. 

O conteúdo da aula 1 está disponível no material PPT aula1.ppt. 

______________________________________________________


# K8S - Criação do ambiente para as aulas. 
__________________________________________________

Para as aulas iremos trabalhar com alguns cenários os quais deixarei as instruções abaixo. 

1. Requisitos minimos:
   - Computador ou Laptop com 8 GB de RAM (minimo necessário) e ou máquina virtual equivalente;
   - Sistema Operacional Windows com WSL instalado com Linux com distro a sua escolha (preferência distros like DEBIAN ou like REDHAT);
   - Quem utilizar computador/laptop sem windows usar Linux com distro a sua escolha (preferência distros like DEBIAN ou like REDHAT);
   - VSCODE instalado;
   - Criação de uma conta em um provedor de CLOUD Publica, preferencialmente Oracle Cloud ou AWS;
   - Terraform instalado e configurado;
   - Kubectl instalado;
   - Minikube instalado;

As instruções para os demais requisitos a serem instalados serão mostradas passo a passo abaixo:

# K8S - Criação de conta na Cloud Oracle Cloud gratuitamente
__________________________________________________

1. Acesse o link: https://www.oracle.com/br/cloud/
2. Clique no botão "teste a OCI gratuitamente";
   
   ![image](https://github.com/user-attachments/assets/f45196c1-d85b-4a16-983e-f1f106deb27a)
   
4. Clique no botão "começe já gratuitamente";
   
6. ![image](https://github.com/user-attachments/assets/4aa3d464-ec3b-440a-a3e8-7831f194bee4)
   
8. Preencha os dados solicitados até criar a conta com sucesso;
   
10. Uma vez criada a conta no e-mail cadastrado você deve receber os dados de acesso da sua conta semelhantes ao abaixo:
    
   ![image](https://github.com/user-attachments/assets/26dc43c1-dc65-4658-a7ae-c31b116ac55c)

Pronto, sua conta está disponível para uso. Lembre-se que o nivel de gratuidade dela é limitado e portanto use os recursos com sabedoria. 

# K8S - instalação do WSL ( para quem usar sistema operacional Windows), quem estiver usando ambiente com Linux, desconsidere este passo. 
__________________________________________________

Para quem for utilizar ambiente com sistema operacional WINDOWS, é necessário instalar o WSL. 

A melhor instrução é utilizar a documentação oficial da MICROSOFT disponível em: https://learn.microsoft.com/pt-br/windows/wsl/install. 

Uma vez instalado seguindo os passos do link anterior, para validação use os exemplos abaixo: 

1. Abra o CMD (sem modo administrador);
2. Digite o comando "wsl --version" e pressione a tecla ENTER. O resultado deve ser semelhante a imagem:
   
![image](https://github.com/user-attachments/assets/3cca528e-cd89-4506-9656-73f167945ded)

3. Para instalar uma distro a sua escolha faça o seguinte:
4.  Digite o comando "wsl --list --online" e na tela você verá o seguinte:

![image](https://github.com/user-attachments/assets/a79f38e6-47cf-4ac4-a1c1-539cada8a72c)

5. Escolha uma distro (preferencialmente Ubuntu ou Oracle Linux 9.x) e execute o comando: "wsl --install -d 'nome da distribuição escolhida' onde o nome da distribuição deve ser igual o valor da coluna "NAME" semelhante a imagem abaixo:

![image](https://github.com/user-attachments/assets/64d6bc60-19e0-49ce-a033-85b945aef184)

6. Aguarde a instalação finalizar;

7. Crie um usuário e senha quando solicitado;

Pronto, sua instalação WSL está disponível e acessível; 

# K8S - instalação do VSCODE 
______________________________________________________________________________

O VSCODE está disponivel através do link: https://code.visualstudio.com/ 

**Para usuários WINDOWS:**

1. Clique no botão:
   
![image](https://github.com/user-attachments/assets/6a70cc3c-7d7f-48ed-bfc6-4c95664812c7)

2. Assim que terminar o download, clique no arquivo para realizar a instalação. Para prosseguir, você precisará aceitar os termos de uso.

![image](https://github.com/user-attachments/assets/ab9a06f1-dd88-4c81-a4be-294a88e6c870)

3. Em seguida, escolha a pasta do seu computador onde será instalado o VS Code.

![image](https://github.com/user-attachments/assets/f1719bb1-205c-4e94-9f6e-e289e9c77d0c)

4. Na tela seguinte, o instalador notifica que irá criar os atalhos do programa. Se não quiser que isso ocorra, marque a opçao Não criar uma pasta no Menu Iniciar. Do contrário, apenas clique em Próximo.

![image](https://github.com/user-attachments/assets/f3870c23-9f59-49ae-9135-51deab59215b)

5. Agora, você deverá se atentar a um ponto importante: marcar a opção Adicione em PATH. Isso é necessário para que o software fique disponível nas suas variáveis de ambiente, portanto copie as configurações do exemplo abaixo para funcionar.

![image](https://github.com/user-attachments/assets/a39ca49e-6c81-463b-99ab-059995c10581)

6. O próximo passo é revisar se todas as configurações estão corretas. Caso sim, clique em Instalar; mas, do contrário, clique em Voltar e corrija o que for necessário.

![image](https://github.com/user-attachments/assets/0a373463-b8dd-4b12-9613-89c354bc587c)

7. Após a instalação, se tudo ocorrer bem, clique em Concluir para abrir o editor.

![image](https://github.com/user-attachments/assets/57feec8b-f4c8-4ba2-b559-b4b74748cc58)

_______________________________________________________________________________________________________________________

**Para usuários LINUX:**

LINUX LIKE DEBIAN (Ubuntu e afins): 

1. Primeiro passo é necessário instalar o pacote SNAP. Para isso Abra um terminal com permissão de elevação usando SUDO;
2. Execute o comando: sudo apt install snap e pressione ENTER;

![image](https://github.com/user-attachments/assets/6e4766c3-976f-490a-a883-b69236f4bc4f)

3. Instalado com sucesso, no mesmo terminal digite o comando: sudo snap install code --classic;
   
4. Uma vez instalado sempre que precisar atualizar a versão utilize o comando: sudo snap refresh code;


LINUX LIKE REDHAT, Fedora, CentOS e derivados: 

1. Abra um terminal com permissão de elevação usando SUDO;
2. Execute o comando: sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
3.  Baixe a chave do repositório do programa com o comando: sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
4.  Atualize o gerenciador de pacotes com o comando: sudo dnf check-update. Caso utilize versão 7.x do Redhat Linux ou equivalente o comando deve ser sudo yum update;
5.  Agora use o comando abaixo para instalar o programa; sudo dnf install code. Caso utilize versão 7.x do Redhat Linux ou equivalente o comando deve ser sudo yum install code;

_________________________________________________________________________________________________________________________________________


# K8S - instalação do kubectl 
____________________________________

Para execução de qualquer comando no kubernetes, é necessário a instalação do interpretador de comandos kubectl e dos demais componentes: 

A instalação está disnponível em: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl 

No link acima encontra-se a explicação para cada tipo de sistema operacional. Atentem-se ao que estiver utilizando. 

# K8S - instalação do Terraform 
____________________________________

Para criarmos os recursos rapidamente nas CLOUDS precisamos do Terraform que é uma ferramenta para produção e criação de infraestrutura como código. 

A instalação deve ser realizada seguindos os passos da documnentação do terraform. Mais uma vez atente-se ao seu sistema operacional e siga os passos recomendados do link: 
https://developer.hashicorp.com/terraform/tutorials/oci-get-started/install-cli?in=terraform%2Foci-get-started 

# K8S - instalação do Oracle Cloud Command Line
________________________________________________________

Para usarmos os recursos no ORACLE CLOUD é recomendável a instalação e configuração do OCI CLI. Os passos se encontram no link de acordo com o sistema operacional escolhido.

https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliinstall.htm

Uma vez instalado o OCI CLI é necessário comnfigurar o mesmo para uso com o Terraform. Isso pode ser feito seguindo os passos do link: https://developer.hashicorp.com/terraform/tutorials/oci-get-started/oci-build?in=terraform%2Foci-get-started 

______________________________________

Com todos os requisitos estamos prontos!!! 









   



