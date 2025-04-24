*Ou como evitar que dados ruins entrem e piorem seu código*

Recentemente, me deparei com um bug causado por um valor inesperado que passou despercebido na entrada de um sistema. Nada muito grave, mas o suficiente para causar uma dor de cabeça. Foi um daqueles momentos em que pensei: *"Eu já devia ter validado isso na borda."* 

Essa situação reforça algo que considero essencial em qualquer projeto: **validar os dados desde a entrada**. Pode parecer uma prática simples, mas ela evita problemas muito maiores lá na frente.

## O que são as bordas?

Chamamos de "borda" qualquer ponto de entrada de dados no seu sistema. Pode ser um endpoint de API, um formulário da web, uma fila de mensagens, um arquivo de configuração, ou até mesmo o construtor de uma classe. Sempre que você recebe algo de fora — ou até de outra parte do seu próprio sistema — está lidando com uma borda.

E como em qualquer fronteira, **você não pode confiar cegamente no que está entrando**. É aqui que a validação se torna essencial.

## Validação não é desconfiança. É proteção.

Não importa se os dados vêm do usuário final, de uma aplicação interna, ou até do seu próprio código: **dados não validados são uma ameaça em potencial**. Pode ser algo simples, como um campo obrigatório vazio, um número fora do intervalo esperado, uma string em formato inválido, um valor de enum desconhecido ou até mesmo um `null` onde não deveria existir. 

A princípio, essas falhas podem parecer inofensivas, mas, com o tempo, um desses valores incorretos pode causar uma exceção em produção ou, pior, corromper silenciosamente a lógica de negócio. E o impacto disso nem sempre é imediato. Às vezes, os efeitos só aparecem quando já é tarde demais.

## Ferramentas modernas estão aí para ajudar

Felizmente, frameworks modernos oferecem muitas maneiras de automatizar a validação. Se você utiliza Java, por exemplo, pode contar com anotações como `@NotNull`, `@Min`, `@Size`, `@Email` e outras. Essas anotações permitem validar os dados já na entrada do seu controller ou DTO, sem a necessidade de escrever condições manuais repetidamente. Essa abordagem é rápida, padronizada e evita a redundância de código, te livrando de várias dores de cabeça.

Se a sua stack não oferece esse tipo de suporte de forma eficiente, vale a pena considerar outra abordagem ou até trocar de ferramenta. Reescrever as mesmas regras básicas de validação o tempo todo é trabalho perdido.

## Não pare por aí: valide também dentro do sistema

Muita gente faz a validação na camada de entrada e acha que está tudo resolvido, mas o que acontece se alguém chamar uma função interna diretamente, sem passar por aquela primeira camada de validação? O código ainda está vulnerável.

Por isso, **valide sempre, também, dentro do seu sistema**. Métodos e construtores precisam checar se os parâmetros estão corretos. Isso garante que, mesmo que algo passe pelas camadas externas, você ainda tenha uma segurança adicional antes que valores inesperados causem problemas mais profundos.

E quando algo der errado, **gere erros descritivos**. Ao invés de lançar um `NullPointerException` genérico, por exemplo, prefira criar uma exceção com uma mensagem clara, como:  
> `"O Usuário não possui um país de origem"`  
> Essa prática facilita a manutenção e ajuda quem estiver debugando no futuro (incluindo você mesmo).

## Valide em camadas. E sim, pode repetir.

Pode parecer exagero, mas é justamente a redundância de validação que torna seu sistema mais confiável. Você deve validar em várias camadas do seu sistema:

- No controller ou ponto de entrada da API
- No domínio, com regras de negócio
- No banco de dados, com restrições (`NOT NULL`, `CHECK`, etc.)

Essa validação em camadas é uma forma de garantir que, mesmo que uma delas falhe, as outras ainda protegerão a aplicação. E caso um ponto intermediário mude de comportamento, as outras camadas continuarão a proteger seu sistema.

## Conclusão

Embora todos saibam que dados ruins geram problemas, ainda assim deixamos passar algumas validações aqui e ali. Às vezes por pressa, outras vezes por descuido. O que é certo é que, invariavelmente, o resultado aparece mais tarde — seja como um bug irritante, uma regra de negócio quebrada ou uma exceção misteriosa que ninguém sabe de onde veio.

Validar nas bordas não é uma questão de paranoia. Trata-se de cuidar do seu sistema desde o início, para evitar que problemas se espalhem e comprometam a integridade do seu código.

Depois de adotar esse hábito, você provavelmente vai se perguntar por que não o fez antes. E, se há uma lição que vale a pena reforçar, é esta: **valide logo na entrada. E continue validando ao longo do sistema**. Seu código (e seu eu do futuro) vão agradecer.
