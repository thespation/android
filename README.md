# Android: Desinstale apps de fábrica, sem root

Antes de realizar qualquer operação do tipo, tenha total certeza que não será prejudicial ao seu dispositivo, sistema ou funcionalidade, tendo em vista que a remoção de determinados apps pode acarretar mau funcionamento ou até mesmo a inviabilidade do sistema carregar.

A motivação foi ter aplicativos de fábrica descontinuados e sem a possibilidade de remoção, *Google Play Music* por exemplo. Como se tornar *root* pode deixar outros aplicativos sem uso, optei por esse método.

## Pré-requisitos
* Celular em modo de **Depurador**
* Drivers ADB
* Powershell
* Android SDK Platform Tools

## Celular em modo de **Depurador**
Cada celular possui uma forma de habilitar, de um modo geral, basta acessar as configurações de seu dispositivo: "*Sobre o telefone*" e pressione diversas vezes sobre "*Número da versão*" até aparecer a informação “ *Você agora é um desenvolvedor!* ”. No caso de um xiaomi, pressione sobre a "versão do MIUI". Toque em "*Opções do desenvolvedor*" na interface "*Configurações*" e habilite " *Depuração USB*"

Na dúvida, pesquise sobre como ativar especificamente no modelo de seu celular.

## Drivers ADB
Vou levar em consideração a base Debian, pois foi o sistema utilizado para realizar essa operação. Fique a vontade para testar em outras bases, sempre adaptando o que for necessário.
1. Em seu terminal, atualize o gerenciador de pacotes:

```
sudo apt update
```

2. Instale os pacotes abaixo:
```
sudo apt install android-tools-adb android-tools-fastboot
```
3. Verifique se instalou corretamente:
```
adb version
```
---

Para termos certeza se o ADB foi instalado corretamente e está funcionando, com o celular em modo de depuração habilitado e conectado ao computador, abra o terminal e digite:
```
adb devices
```
Seu celular solicitará autorização para que o computador possa realizar modificações. Clique em "OK"

![image](https://user-images.githubusercontent.com/84329097/183400491-aadccf20-efbc-4480-ac3b-4b521139b060.png)

Caso receba o erro: `???????????? no permissions` ao rodar o comando `adb devices`, será necessário reiniciar o daemon do ADB. Rode o comando:
```
sudo adb kill-server
```

E depois, esse outro comando para executá-lo:

```
sudo adb start-server
```
## Powershell
Os comandos utilizados são para Windows, para que não tenhamos que utilizá-lo, podemos executar pelo PowerShell. Para instalá-lo do Debian 11:


```
# Instala componentes do sistema
sudo apt update  && sudo apt install -y curl gnupg apt-transport-https

# Importar as chaves GPG do repositório público
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Registre o feed de produtos da Microsoft
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-bullseye-prod bullseye main" > /etc/apt/sources.list.d/microsoft.list'

# Instala o PowerShell
sudo apt update && sudo apt install -y powershell

# Inicie o PowerShell
pwsh
```
Caso queira instalar em outra distribuição, link oficial da Microsoft nas referências.

## Android SDK Platform Tools
1. [Baixe o arquivo ZIP Android SDK Platform Tools](https://raw.githubusercontent.com/thespation/android/main/platform-tools_r33.0.2-linux.zip?token=GHSAT0AAAAAABXI2PNLEUDHV6M5USQLHY34YXQ5K2Q)
2. Extraia o arquivo e acesse a pasta
3. Para facilitar, clique com o botão direito em um espaço vazio da pasta e clique em "Abrir o terminal aqui"

![image](https://user-images.githubusercontent.com/84329097/183400541-7d7ae916-5410-4bbd-858c-73af16460185.png)

Caso prefira, pode abrir o terminal e navegar até a pasta onde estão os arquivos (só é mais trabalhoso).
4. Verifique se o dispositivo está sendo reconhecido com o  comando:
```
adb devices
```
O meu mostrou a seguinte saída:

```
PS /home/william/Downloads/platform-tools> adb devices
List of devices attached
265a975e7cf5	device

PS /home/william/Downloads/platform-tools>
```
 5. Abra o Powershell, com o seguinte comando:
```
pwsh
```
O meu mostrou a seguinte saída:

```
PowerShell 7.2.5
Copyright (c) Microsoft Corporation.

https://aka.ms/powershell
Type 'help' to get help.

PS /home/william/Downloads/platform-tools>
```
6. Vamos finalmente abrir o shell ADB, utilize o comando:
```
adb shell
```

Deve mostrar algo parecido com:

```
PS /home/william/Downloads/platform-tools> adb shell
rosy:/ $ 
```
7. Para listar os aplicativos instalados, podemos pesquisar pelo nome, ou parte do nome, com o seguinte comando:
```
pm list packages | grep 'NomeDoAppDesejado'
```
Por exemplo, ao pesquisar pela palavra "google"
```
rosy:/ $ pm list packages | grep 'google'
package:com.google.android.youtube
package:com.google.android.ext.services
package:com.google.android.onetimeinitializer
package:com.google.android.ext.shared
package:com.google.android.apps.paidtasks
package:com.google.android.configupdater
package:com.google.android.marvin.talkback
package:com.google.android.gm
package:com.google.android.setupwizard
package:com.google.android.apps.docs
package:com.google.android.apps.maps
package:com.google.android.webview
package:com.google.android.syncadapters.contacts
package:com.google.android.packageinstaller
package:com.google.android.gms
package:com.google.android.gsf
package:com.google.android.tts
package:com.google.android.partnersetup
package:com.google.android.feedback
package:com.google.android.printservice.recommendation
package:com.google.android.apps.photos
package:com.google.android.calendar
package:com.google.android.syncadapters.calendar
package:com.google.android.backuptransport
package:com.google.android.inputmethod.pinyin
package:com.google.android.inputmethod.latin
rosy:/ $  
```
Com isso conseguimos o nome completo, será necessário para a remoção.

8. Para remover um aplicativo indesejado, utilize o comando:

```
pm uninstall -k --user 0 NomeDoAppDesejado
```
Um exemplo de remoção que fiz:

```
rosy:/ $ pm uninstall -k --user 0 com.google.android.googlequicksearchbox
Success
rosy:/ $ 
```
9. Repita os passos quantas vezes forem necessárias para remover todos os aplicativos desejados e ao final digite "exit" para sair do ADB e "exit" para sair do PowerShell:

```
rosy:/ $ exit                                                                                      
PS /home/william/Downloads/platform-tools> exit
PS /home/william/Downloads/platform-tools> exit
 ~/Downloads/platform-tools
```
<hr>

### Referências:
* [Instalar o PowerShell no Linux](https://docs.microsoft.com/pt-br/powershell/scripting/install/installing-powershell-on-linux?view=powershell-7.2)
* [Olhar Digital: Como remover os apps de fábrica do seu Android sem root](https://olhardigital.com.br/2018/08/10/dicas-e-tutoriais/como-remover-os-apps-de-fabrica-do-seu-android-sem-root/)
* [How to Install ADB on Windows, macOS, and Linux](https://www.xda-developers.com/install-adb-windows-macos-linux/#adbsetuplinux)

