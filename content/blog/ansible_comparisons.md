+++
title = "Dica rápida sobre docker_container comparisons no ansible"
date = "2024-10-22"
draft = false
Categories = ["portugues", "devops", "ansible"]
Tags = ["portugues", "devops", "ansible"]
+++

## Introdução

Se você está usando o módulo docker_container do ansible, fique atento com relação a como o container é recriado. Se você precisar aplicar variável de ambiente, toda vez que trocar os valores seu container não será recriado. A solução está abaixo:

![Exemplo de uso do ansible comparisons](/img/ansible_comparisons.png)

Usando o comparisons você pode dizer para o ansible reiniciar o container toda vez que qualquer mudança é feita no env. Sem isso o ansible vai ignorar a mudança e marcar aquela task como "OK" e nada mudará no seu container.

## O que fala a documentação oficial

De acordo com a documentação do ansible, é assim que funciona a opção comparisons do módulo docker_container:

Permite especificar como as propriedades de contêineres existentes são comparadas com opções de módulo para decidir se o contêiner deve ser recriado/atualizado ou não.

Somente opções que correspondem ao estado de um contêiner conforme manipulado pelo daemon do Docker podem ser especificadas, assim como redes.
Deve ser um dicionário especificando para uma opção uma das chaves strict, ignore e allow_more_present.

Se strict for especificado, os valores serão testados para igualdade, e as alterações sempre resultam em atualização ou reinicialização. Se ignore for especificado, as alterações serão ignoradas.

allow_more_present é permitido apenas para listas, conjuntos e dicionários. Se for especificado para listas ou conjuntos, o contêiner será atualizado ou reiniciado somente se a opção do módulo contiver um valor que não esteja presente nas opções do contêiner. Se a opção for especificada para um dicionário, o contêiner será atualizado ou reiniciado somente se a opção do módulo contiver uma chave que não esteja presente na opção do contêiner, ou se o valor de uma chave presente for diferente.

A opção curinga * pode ser usada para definir um dos valores padrões strict ou ignore para *todas* as comparações que não são explicitamente definidas para outros valores.

## Conclusão

Se você não precisa reiniciar o container quando uma variável é modificada, não precisa se preocupar com essa informação. Caso contrário, aplique no seu código ansible para evitar problemas.