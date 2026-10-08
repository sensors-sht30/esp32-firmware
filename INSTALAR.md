# Como instalar o firmware num gateway novo (Inter Sensores)

Guia rápido para quem vai testar. Leva uns 10 minutos.

## Você vai precisar

- Um **computador** (Windows, Mac ou Linux) com **Google Chrome** ou **Microsoft Edge**.
  Celular, Firefox e Safari **não** funcionam.
- O gateway (placa ESP32 ou ESP32-C3) e um **cabo USB de dados**.
  Cabo que só carrega celular não serve: se o computador não "enxergar" a placa, troque o cabo.
- Um login de **administrador** no painel da Inter Sensores.
- Um celular, para configurar o gateway depois.

## Parte 1 — Gravar o firmware (no computador)

1. Ligue a placa no computador pelo cabo USB.
2. No Chrome/Edge, entre no painel e clique em **Install firmware** no menu da esquerda.
3. Escolha a placa certa: **ESP32 (DevKit)** ou **ESP32-C3 SuperMini** (a SuperMini é a
   plaquinha pequena, do tamanho de um polegar, com conector USB-C e antena externa).
4. Clique em **Download and verify**.
   Deve aparecer em verde: *"Image verified: size, SHA-256 and signature match."*
5. Clique em **Connect and install**. Abre uma janelinha do navegador com as portas:
   escolha a que tem **CP2102**, **CH340**, **USB Serial** ou, na SuperMini,
   **USB JTAG/serial debug unit** no nome, e clique em **Conectar**.

   **ESP32-C3 SuperMini:** ela usa o USB do próprio chip (no Windows 10 ou mais novo não
   precisa de driver). Se o navegador não encontrar a porta: desligue o cabo, **segure o
   botão BOOT enquanto liga o cabo USB** de novo, solte o botão e tente outra vez.
6. Espere. A tela mostra *Connecting*, depois *Erasing* (até 1 minuto) e *Writing* com a
   barra de progresso. **Não desligue o cabo** nesse tempo.
7. No fim aparece **"Installed. Next steps"**. Pronto, pode passar para a Parte 2.

> Atenção: a gravação **apaga tudo** da placa. Ela volta como um gateway novo
> (outro ID). Se ela já estava ativada antes, vai precisar de um código de ativação novo.

## Parte 2 — Configurar (no celular)

1. Deixe o gateway ligado (pode ficar no USB do computador ou num carregador).
2. Segure o botão **BOOT** da placa por uns **5 segundos**, até o LED piscar devagar, e solte.
   O LED fica **aceso fixo**: a configuração está aberta (por 15 minutos).
3. No celular, conecte na rede WiFi **InterSensores-XXXX** (a senha de fábrica é passada
   pela equipe Inter Sensores).
4. A página de configuração costuma abrir sozinha. Se não abrir, digite
   **192.168.4.1** no navegador do celular. Entre com o usuário e senha de instalador.
5. Siga as telas: **WiFi** (escolha a rede do local e digite a senha), **Nome do aparelho**
   e **Sensores**.
6. No painel, abra o cliente e clique em **New code**. Digite esse código de 6 números
   na tela **Ativação** do celular.
7. Em até 1 minuto o gateway aparece no painel, no cliente escolhido.

Depois disso as atualizações chegam sozinhas pela internet.

## Se der problema

| O que aparece | O que fazer |
|---|---|
| Aviso amarelo "needs Chrome or Edge on a desktop computer" | Use Chrome ou Edge num computador. |
| A janelinha não mostra nenhuma porta | Troque o cabo (tem que ser de dados). ESP32-C3 SuperMini: segure **BOOT** enquanto liga o cabo. ESP32 DevKit no Windows: instale o driver **CP210x** ou **CH340** e ligue a placa de novo. |
| "The serial port is busy" | Feche Arduino IDE, PlatformIO, monitor serial ou outra aba que esteja usando a placa e tente de novo. |
| "Could not talk to the chip" | Segure **BOOT**, aperte e solte **EN/RST** (na SuperMini: **RST**), solte **BOOT** e clique em **Connect and install** de novo. Na SuperMini também dá para segurar BOOT enquanto liga o cabo. |
| "Wrong chip" | Você escolheu a placa errada no passo 3. Escolha a outra e repita. |
| "SHA-256 mismatch" ou "Signature is not valid" | Não instale. Avise a equipe Inter Sensores. |
| "No port was selected" | Você fechou a janelinha sem escolher. Clique de novo em **Connect and install**. |
| A rede InterSensores-XXXX não aparece | Segure BOOT por 5 s de novo (o LED tem que ficar aceso fixo). |
| Segurou BOOT por 15 s ou mais | Isso é "restaurar de fábrica"; não estraga nada, só repita a configuração. |
