# Como configurar/instalar o `ClamAV` no `Linux Ubuntu`

## Resumo

Neste documento estão contidos os principais comandos e configurações para configurar/instalar o `ClamAV` no `Linux Ubuntu`.

## _Abstract_

_In this document are contained the main commands and settings to set up/install the `ClamAV` on `Linux Ubuntu`._

## 1. Como configurar/instalar o `ClamAV` no Linux Ubuntu [1]

Para instalar o `ClamAV` no Linux Ubuntu, você pode seguir estes passos:

1. Abra o `Terminal Emulator`. Você pode fazer isso pressionando: `Ctrl + Alt + T`


2. Certifique-se de que seu sistema esteja limpo e atualizado.

    2.1 Limpar o `cache` do gerenciador de pacotes `apt`. Especificamente, ele remove todos os arquivos de pacotes (`.deb`) baixados pelo `apt` e armazenados em `/var/cache/apt/archives/`. Digite o seguinte comando: `sudo apt clean` 
    
    2.2 Remover pacotes `.deb` antigos ou duplicados do cache local. É útil para liberar espaço, pois remove apenas os pacotes que não podem mais ser baixados (ou seja, versões antigas de pacotes que foram atualizados). Digite o seguinte comando: `sudo apt autoclean`

    2.3 Remover pacotes que foram automaticamente instalados para satisfazer as dependências de outros pacotes e que não são mais necessários. Digite o seguinte comando: `sudo apt autoremove -y`

    2.4 Buscar as atualizações disponíveis para os pacotes que estão instalados em seu sistema. Digite o seguinte comando e pressione `Enter`: `sudo apt update`

    2.5 **Corrigir pacotes quebrados**: Isso atualizará a lista de pacotes disponíveis e tentará corrigir pacotes quebrados ou com dependências ausentes: `sudo apt --fix-broken install`

    2.6 Limpar o `cache` do gerenciador de pacotes `apt`. Especificamente, ele remove todos os arquivos de pacotes (`.deb`) baixados pelo `apt` e armazenados em `/var/cache/apt/archives/`. Digite o seguinte comando: `sudo apt clean` 
    
    2.7 Para ver a lista de pacotes a serem atualizados, digite o seguinte comando e pressione `Enter`:  `sudo apt list --upgradable`

    2.8 Realmente atualizar os pacotes instalados para as suas versões mais recentes, com base na última vez que você executou `sudo apt update`. Digite o seguinte comando e pressione `Enter`: `sudo apt full-upgrade -y`
    


3. **Instale o ClamAV:** Após a atualização, você pode instalar o `ClamAV` com o seguinte comando: `sudo apt install clamav clamav-daemon -y`

    Este comando instala o `ClamAV` e o `daemon` do `ClamAV`, que é um serviço em segundo plano para a varredura regular de vírus.

4. **Atualize a Base de Dados de Vírus:** Depois de instalar o `ClamAV`, é importante atualizar a base de dados de vírus. Isso pode ser feito com o comando: `sudo freshclam`

    Se você receber uma mensagem de erro devido ao `daemon` do `ClamAV` já estar em execução, você pode parar o `daemon` temporariamente com `sudo systemctl stop clamav-freshclam`, executar a atualização e depois reiniciar o `daemon` com sudo `systemctl start clamav-freshclam`.

5. **Verificar o Sistema para Vírus:** Uma vez que o `ClamAV` está instalado e a base de dados está atualizada, você pode começar a verificar o seu sistema para vírus. Um exemplo de comando para escanear todo o sistema é: `sudo clamscan -r --bell -i --log=/home/edenedfsls/arquivo_de_log_do_clamav.txt /`

    Este comando escaneia recursivamente todo o sistema (`/`), soando um alarme (`--bell`) quando encontrar algo e mostrando apenas os arquivos infectados (`-i`).

6. **Agendar Varreduras Automáticas:** Se desejar, você pode configurar varreduras automáticas. Isso geralmente envolve a criação de um `cron job`, que é um pouco mais avançado e depende de suas necessidades específicas.

Lembre-se que o `ClamAV` é mais frequentemente usado para escanear e-mails e servidores de arquivos em ambientes de rede. Enquanto ele pode ser usado para varredura de `desktop`, ele não possui algumas características de antivírus mais comerciais, como proteção em tempo real.

### 1.1 Código completo para configurar/instalar

Para instalar o `ClamAV` no `Linux Ubuntu` sem precisar digitar linha por linha, você pode seguir estas etapas:

1. Abra o `Terminal Emulator`. Você pode fazer isso pressionando: `Ctrl + Alt + T`

2. Digite o seguinte comando e pressione `Enter`:

    ```
    sudo apt clean                                                            ─╯
    sudo apt autoclean
    sudo apt autoremove -y
    sudo apt update
    sudo apt --fix-broken install
    sudo apt clean
    sudo apt list --upgradable
    sudo apt full-upgrade -y
    sudo apt install clamav clamav-daemon -y
    sudo freshclam
    sudo clamscan -r --bell -i --log=/home/edenedfsls/arquivo_de_log_do_clamav.txt /
    ```


## 2. Agendar varreduras automáticas

Para agendar uma varredura automática com o `ClamAV` no Ubuntu, você pode usar o `cron`, um agendador de tarefas do Linux. Aqui está um guia passo a passo de como fazer isso:

1. **Abra o Crontab:** Para editar o arquivo crontab para o seu usuário, abra o terminal e digite: `crontab -e`

2. **Escolha um Editor:**

    Se for a primeira vez que você está usando o `crontab`, ele pode pedir para você escolher um editor. Geralmente, `nano` é a opção mais fácil para iniciantes.

3. **Adicione a Tarefa Agendada:** No editor, adicione uma linha para agendar a tarefa. O formato é: `m h dom mon dow command`

    Onde:

    - `m` é o minuto (0 a 59)
    - `h` é a hora (0 a 23)
    - `dom` é o dia do mês (1 a 31)
    - `mon` é o mês (1 a 12)
    - `dow` é o dia da semana (0 a 6, onde 0 é domingo)

    Por exemplo, para agendar uma varredura todos os dias às 12, você escreveria: `0 12 * * * sudo clamscan -r --bell -i / | mail -s "Resultado da Varredura Diária ClamAV" edendenis@gmail.com`

    Este comando executa `clamscan` no diretório raiz (`/`) todos os dias às 12 e envia os resultados para o seu e-mail. Certifique-se de substituir `edendenis@gmail.com` pelo seu endereço de e-mail real.

4. **Salve e Saia:** Se você estiver usando o `nano`, você pode salvar e sair pressionando `CTRL + X`, depois `Y` para confirmar as alterações, e `Enter` para sair.

5. **Verifique se o Agendamento foi Criado:** Para verificar se a tarefa foi agendada corretamente, você pode listar suas tarefas cron com: `crontab -l`

6. **Configuração do Mail:** O comando acima assume que você tem um programa de envio de e-mails configurado em seu sistema (como mail). Se não tiver, você precisará configurar um para receber os relatórios de varredura por e-mail.

Essa é uma maneira básica de agendar varreduras com o ClamAV. Dependendo do seu ambiente e necessidades, você pode precisar de configurações adicionais, especialmente se estiver trabalhando em um ambiente de servidor ou empresarial.

## 3. Limpeza de Vírus `ClamAV` [3]

Para limpar os vírus detectados pelo `ClamAV` em seu sistema, siga estas etapas:

1. **Quarentena ou Exclusão de Arquivos Infectados:** Primeiro, você precisa decidir se deseja colocar os arquivos infectados em quarentena ou excluí-los. A quarentena move os arquivos para um local seguro, onde eles não podem causar danos, mas ainda estão disponíveis se você precisar deles. A exclusão remove os arquivos do seu sistema completamente.

2. **Usar a Linha de Comando:** Se você está usando o `ClamAV` em um ambiente de linha de comando, pode usar o comando `clamscan` para quarentenar ou excluir arquivos infectados. Aqui está um exemplo de como usar o comando: `clamscan --infected --remove --recursive /caminho/para/verificar`

    Este comando irá verificar todos os arquivos no diretório especificado, remover arquivos infectados (`--remove`) e verificar subdiretórios (`--recursive`).

3. **Verificar Log do ClamAV para Detalhes:** O `ClamAV` gera um log com detalhes sobre a varredura. Verifique o log para entender quais arquivos foram infectados e o tipo de malware encontrado.

4. **Backup de Dados Importantes:** Antes de excluir quaisquer arquivos, certifique-se de fazer backup de dados importantes. Isso evita a perda acidental de informações valiosas.

5. **Atualize o Banco de Dados de Vírus:** Certifique-se de que o banco de dados de vírus do `ClamAV` esteja atualizado para a detecção mais recente de ameaças. Você pode fazer isso executando `freshclam`.

6. **Rever Arquivos em Quarentena:** Se você optar por quarentenar arquivos, revise-os cuidadosamente. Alguns podem ser falsos positivos, especialmente se forem arquivos de sistema ou programas legítimos.

7. **Repita a Varredura:** Após limpar ou quarentenar os arquivos, execute outra varredura completa para garantir que todos os vírus foram removidos.

8. **Consulte um Profissional:** Se você não se sentir confortável para realizar esses passos ou se a infecção for severa, considere buscar ajuda profissional.

Lembre-se de que a manutenção regular e as varreduras de vírus são essenciais para a segurança do sistema. Mantenha seu sistema operacional e software atualizados para evitar vulnerabilidades.

## 4. Adicionar um programa à lista de exceções do ClamAV

Para adicionar, por exemplo, o `Metasploit` à lista de exceções do `ClamAV`, você precisa editar os arquivos de configuração do antivírus para que ele ignore os diretórios e arquivos associados ao `Metasploit`. Vamos ver como fazer isso para cada um:

1 **Localize o diretório de instalação do `Metasploit` usando o comando `find`:** Este comando buscará em todo o sistema de arquivos pelo diretório do `Metasploit`. Como pode demorar um pouco, é uma boa prática limitar a busca aos diretórios onde o `Metasploit` é normalmente instalado, como `/opt` ou `/usr`. Vamos usar o seguinte comando para buscar no diretório `/opt`: `sudo find /opt -name metasploit-framework`

2. **Use o comando locate:** O comando locate é outra forma de encontrar arquivos e diretórios. Antes de usá-lo, atualize o banco de dados do `locate` com o comando `sudo updatedb`. Depois, procure pelo `Metasploit` com: `locate metasploit-framework`

3. **Verifique diretamente no diretório padrão:** O `Metasploit` é frequentemente instalado em `/opt/metasploit-framework`. Verifique se o diretório existe com: `ls /opt/metasploit-framework`

Depois de encontrar o diretório do `Metasploit`, você pode seguir os passos que mencionei anteriormente para adicionar exceções no `ClamAV`.

### 4.1 Reiniciar o serviço do `ClamAV`

Para reiniciar o serviço `ClamAV` no seu sistema `Linux`, você pode usar o comando `systemctl`. Aqui está como você pode fazer isso:

1. Abra um terminal.

2. Digite o seguinte comando para reiniciar o serviço `ClamAV`: `sudo systemctl restart clamav-daemon`

    Este comando irá parar e então reiniciar o serviço clamav-daemon, que é o `daemon` do ClamAV.

    2.1 Se você estiver usando uma versão mais antiga do `Linux` que não suporta `systemctl`, você pode precisar usar os seguintes comandos: Para parar o serviço: `sudo service clamav-daemon stop`

    2.2 **Para iniciar o serviço novamente:** `sudo service clamav-daemon start`

    Após reiniciar o serviço, as novas configurações de exclusão que você adicionou ao arquivo de configuração do ClamAV serão aplicadas. É sempre uma boa prática verificar se o serviço está funcionando corretamente após a reinicialização, o que você pode fazer com o comando: `sudo systemctl status clamav-daemon`

    Ou, para sistemas mais antigos: `sudo service clamav-daemon status`

Isso irá mostrar o `status` atual do serviço `ClamAV`, ajudando a confirmar se ele está ativo e funcionando corretamente.


## Referências

[1] OPENAI. ***Antivírus para linux ubuntu:*** https://chat.openai.com/c/0bfa681a-6952-4c86-a386-76d82f4380a1 (texto adaptado). ChatGPT. Acessado em: 28/11/2023 13:51.

[2] OPENAI. ***Vs code: editor popular:*** https://chat.openai.com/c/b640a25d-f8e3-4922-8a3b-ed74a2657e42 (texto adaptado). ChatGPT. Acessado em: 28/11/2023 13:51.

[3] OPENAI. ***Limpeza de vírus clamav:*** https://chat.openai.com/c/cf622c42-4ca3-4d3e-b040-ac8da48e7c29 (texto adaptado). ChatGPT. Acessado em: 29/11/2023 08:16.

[4] OPENAI. ***Excluir metasploit da verificação*** https://chat.openai.com/c/53b8ca99-e911-4057-900d-318fecab1c4f (texto adaptado). ChatGPT. Acessado em: 22/01/2024 22:53.
