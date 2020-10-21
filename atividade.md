# Atividade Computação Forense 20/10/2020

## Simulação de ataque backdoor (com prints).

Vamos começar inicializando o listener e deletando-o logo em seguida.
![startFreedom](https://user-images.githubusercontent.com/19397815/96663608-5f646780-1327-11eb-9904-188ba6db1efa.png)


Vamos confirmar que o processo está rodando na porta que foi definida.
![freedomPID](https://user-images.githubusercontent.com/19397815/96663701-95a1e700-1327-11eb-8307-c130cc78f3ff.png)


Agora verifcaremos que o diretorio de trabalho do processo é na pasta tmp e que o arquivo que originou o processo foi deletado.
![freedomDeleted](https://user-images.githubusercontent.com/19397815/96664681-b3704b80-1329-11eb-8639-afb8897e6c8d.png)


O que precisa ser feito agora é recuperar o binário que originou o processo.
![recovered_bin](https://user-images.githubusercontent.com/19397815/96665032-5e810500-132a-11eb-835b-61598fe92216.png)


Com isso, poderemos pegar a hash do arquivo, uma informação bacana de se ter. No caso, vamos comparar com a hash do netcat e ver que são iguais.
![ComparedHashes](https://user-images.githubusercontent.com/19397815/96665267-e23af180-132a-11eb-9bc9-1f789ed10cd3.png)


Neste próximo print, vamos usar comandos pra mostrar a linha de comando do processo e qual o nome do comando. Verificaremos se há muita diferença entre o nome do processo e quais foram
os comandos executados para lançá-lo, pois alguns programas maliciosos mascaram seus nomes para evitar suspeitas.
![comm_cmdline](https://user-images.githubusercontent.com/19397815/96665698-dbf94500-132b-11eb-8024-94fd110bad4b.png)


Com esse comando iremos verificar quais eram as condições do ambiente que o nosso processo pode ter herdado.
![strings_environ](https://user-images.githubusercontent.com/19397815/96666875-575bf600-132e-11eb-9f0e-1060a656c609.png)


Em seguida, iremos verificar os descritores do arquivo, quais possíveis arquivos e diretórios o processo está interagindo (o PID do processo
alterou pois precisei criar um novo processo para recuperar esse print).
![fd](https://user-images.githubusercontent.com/19397815/96666610-dd2b7180-132d-11eb-8ae1-fd049babf9fc.jpg)


Verificando as bibliotecas que o processo está utilizando.
![cat_maps](https://user-images.githubusercontent.com/19397815/96665797-16fb7880-132c-11eb-8957-4cfca27f0a3a.png)



Agora vamos dar uma olhada na stack, onde iremos encontrar algumas informações pertinentes ao intuito do processo (no caso são as chamadas de network_accept() ).
![stack](https://user-images.githubusercontent.com/19397815/96667088-c1749b00-132e-11eb-93cf-158058541282.png)


E por fim vamos verificar os detalhes gerais do processo.
![status](https://user-images.githubusercontent.com/19397815/96667229-0bf61780-132f-11eb-9d56-8d0593a79a77.png)

## Bind Shell, Reverse Shell e Encrypted Shell.
Para finalizar essa atividade, vamos falar brevemente sobre esses tipo de ataque que envolvem shell. O bind shell pode ser efetuado como resultado de um exploit de backdoor
(como foi descrito acima). O listener é colocado na máquina-alvo e o atacante aproveita-se da vulnerabilidade para conectar-se. O problema é que outros possíveis atores não
desejados podem se utilizar do listener. Na técnica de Reverse Shell, o listener é configurado na máquina do atacante e ele deve fazer com que a máquina-alvo se conecta com
o listener através de um shell. O problema de ambas as técnicas é que as informações trocadas são todas em forma de texto puro, então qualquer sniffer de pacotes pode capturar
essas transações e descobrir seus conteúdos.
