# COMANDOS LINUX

## virar sudo:
```sh
sudo -i
```

## verificar ip:
```sh
ip a
```

## atualizar pacotes do sistema operacional:
```sh
sudo apt-get update && sudo apt-get upgrade -y
```

## reiniciar o sistema:
```sh
reboot
```

## limpar a tela:
```sh
ctrl l / clear
```

## sair do fim:
```sh
ctrl c
```

## mostrar o diretorio atual:
```sh
pwd
```

## listar os arquivos e diretorios:
```sh
ls
ls -a (ocultos)
ls -R (arquivos nos subdiretorios)
ls  -al(listar com info)
```

## comando cd:
```sh
cd ~/ (diretorio home)
cd / (diretorio root)
cd - (voltar ao ultimo diretorio)
```

## adicionar, remover, mover e copiar diretorios/arquivos:
```sh
mkdir (adiciona)
rmdir (remove)
mv teste2.txt /teste22 (manda um arquivo para outro diretorio e depois apaga)
mv teste.txt teste1.txt (Muda o nome do arquivo teste.txt para teste1.txt)
mv teste.txt teste.new (supondo que teste.new já exista Copia o arquivo teste.txt por cima de teste.new e apaga teste.txt após terminar a cópia)
cp -r testes /root 
```

## adicionar e remover arquivos:
```sh 
touch (cria um novo arquivo)
rm (exclui arquivos)
```

## comando find:
```sh
find . -name "arquivo.txt" (pelo nome)
find / -type d (diretorios)
find / -type b (bloqueados)
find / -type f (arquivo normal)
Find / -user ziri (arquivos deste usuario)
find / -perm 644 (arquivos com permissao para ler e escrever)
```

## comando locate:
```sh
sudo apt-get update
sudo apt-get install plocate
locate .txt
```

## comando tar:
```sh
: tar -cvf delta.tar /home/ziri(compactar) -- tar -xvf sampleArchive.tar(descompactar) -- tar -tvf sampleArchive.tar(listar) -- tar -xvf sampleArchive.tar example.sh(extrair 1) -- tar -xvf sampleArchive.tar --wildcards '*.jpg'(extrai varios do mesmo padrao) -- tar -rvf sampleArchive.tar example.jpg(adicionar arquivos)

: tar -cvzf delta.tar.gz /home/ziri(compactar) -- tar -xvf sampleArchive.tar.gz(descompactar) -- tar -tvf sampleArchive.tar.gz(listar) -- tar -zxvf sampleArchive.tar.gz example.sh(extrair 1) -- tar -zxvf sampleArchive.tar.gz --wildcards '*.jpg'(extrai varios do mesmo padrao) -- X

: tar -cvjf delta.tar.bz2 /home/ziri(compactar) -- tar -xvf sampleArchive.tar.bz2(descompactar) -- tar -tvf sampleArchive.tar.bz2(listar) -- tar -jxvf sampleArchive.tar.bz2 example.sh(extrair 1) -- tar -jxvf sampleArchive.tar.bz2 --wildcards '*.jpg'(extrai varios do mesmo padrao) -- X
```

## comando gzip
```sh
gzip GG.odt(compactar) -- gzip -d GG.odt.gz(descompactar) -- gzip -l GG.odt.gz(listar) -- gzip -t GG.odt.gz(testa integridade)
```

## descobrir qual a sua shell:
```sh
echo $SHELL
```

## imprime todos os comandos shell:
```sh
set
```

## atribuir mensagem de apresentacao do sistema
```sh
vim /etc/issue
```

## atribuir uma mensagem pós login no sistema:
```sh
vim /etc/motd
```

 ## ver os processos em execucao no linux:
```sh
top
```

## lista todos os comandos anteriores:
```sh
history
```

##  pesquisa ultimo comando no historico:
```sh
Ctrl + R
```

## entrar no diretorio em que todos os comandos ja usados foram salvos:
```sh
vim /bash_history
```

## criação de novos arquivos de texto de forma rápida:
```sh
cat > arquivo.txt -- enter -- Ctrl + D
cat arquivo.txt(visualiza o arquivo)
cat fonte.txt > destino.txt(redirecionar os arquivos)
cat fonte1.txt fonte2.txt > destino.txt(concatenar Arquivos)
```

## lista todos os comandos linux:
```sh
ls /bin
```

## modificar o idioma do linux:
```sh
vim /etc/locale.gen -- locale-gen
```

## # ver todas as informacoes do sistema:
```sh
uname -a
exibir o tipo de hardware:
uname -m
exibir o nome da maquina:
uname -n
exibir o processador:
uname -p
exibir a versao do kernel:
uname -r
exibir o nome do SO:
uname -s
exibir a data da compilacao do SO:
uname -v
```

## exemplo basico de funcao bash
```sh
#!/bin/bash
testfunction(){
echo "My first function"
}
testfunction -- bash ./testFunction.sh
```

## comando grep
```sh
grep --help(manual)
grep cerveja arquivostestes(encontrar palavras em um diretorio)
grep -c cerveja arquivostestes (quantas vezes a palavra pesquisada aparece no arquivo de texto) 
```

## editor de textos:
```sh
comando vim
vim -- i(inserir) -- esc(visualizar) -- :wq(sair e salvar) -- q(sair) 
vim /etc/vim/vimrc(acessar configuracoes)

comando nano
sudo apt-get update -- sudo apt-get install nano
Ctrl + x(sair e salvar)

comando emacs
sudo apt-get update -- sudo apt-get install emacs
Ctrl + x + s (salvar arquivo)
Ctrl + x + c (sair)
```










