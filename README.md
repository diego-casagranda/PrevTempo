# Previsão do Tempo

ShellScript para Exibir informações do clima em uma cidade

```
#!/bin/bash
#
#===============================================================================
#
# File...........: prevtempo.sh
# Title..........: Previsão do Tempo
# Program........: Shell Code -  GNU/Linux
#
# Description....: Exibe a previsão do tempo para uma cidade
#
# Copyright......: Copyright(c) 2024 / @Diego.Casagranda - HackLab
# License........: GNU GENERAL PUBLIC LICENSE - Version 3, 29 June 2007
#
# Author.........: Diego Casagranda
# E-Mail.........: diego.casagranda@mail.ru
#
# Dependency.....: curl jq
#
# Date...........: 05/06/2024
# Update.........: None
#
# Version........: 0.1.0
#
#===============================================================================
#
# ###########
# # History #
# ###########
#
#     05/06/2024 : Criação do template
#
#===============================================================================

# Inicia a variavel
mensagem_tempo=""

# Arquivo para leitura
JSONFILE="/tmp/${0##*/}.tmp.json"

# Codigo da cidade 
CIDADE=4290

# Token de usuario da clima tempo
TOKEN=32e6ad73da4248ca01a2d358d796b5dc

function comando_tempo() {

  # limpa arquivos temporarios 
  cleanup

  # baixa o arquivo do site clima tempo para a cidade passada 
  curl -s "http://apiadvisor.climatempo.com.br/api/v1/weather/locale/$CIDADE/current?token=$TOKEN" > $JSONFILE

  # pega a temperatura em graus
  tempo_graus=$(jq ".data .temperature" <"${JSONFILE}")

  # Pega a condição do tempo
  cond_tempo=$(jq ".data .condition" <"${JSONFILE}")

  # String para exibição na tela
  mensagem_tempo=$(echo "A temperatura atual é de $tempo_graus graus a condição do tempo para hoje é $cond_tempo." | tr -d '""')

  # Exibe na tela a saida
  echo $mensagem_tempo
}

# Limpa arquivos temporarios
function cleanup() {
    rm -rf $JSONFILE
    rm -rf $JSONFILE.part
}

# Executa a função
comando_tempo

```


Você pode movelo para /usr/bin e disponibilizar o comando para todos os usuarios
```
# copiar para /usr/bin
sudo cp prevtempo.sh /usr/bin/

# Permição para usuarios e grupos
sudo chmod 777 /usr/bin/prevtempo.sh

# Tornalo executavel
sudo chmod +x /usr/bin/prevtempo.sh
```
