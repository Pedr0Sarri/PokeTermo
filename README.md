# Pokémon do Dia

Jogo web de adivinhação diária: todo dia um Pokémon é sorteado e você tem **6 tentativas** para descobrir qual é. Cada palpite mostra o quanto ele se parece com a resposta, comparando tipo, geração, altura, peso e cor.

Inspirado em jogos diários como Wordle e Pokédle. Usa os dados da [PokéAPI](https://pokeapi.co/).

## Como jogar

1. Digite o nome de um Pokémon (em inglês) e escolha uma das sugestões.
2. Cada atributo do palpite é comparado com o Pokémon do dia:

| Cor | Significado |
|-----|-------------|
| 🟩 Verde | Igual ao Pokémon do dia |
| 🟨 Amarelo | O tipo existe, mas na outra posição (tipo 1 ↔ tipo 2) |
| ⬛ Cinza | Diferente |
| ↑ / ↓ | A resposta tem geração, altura ou peso maior / menor |

3. Depois de 3 erros, a silhueta do Pokémon é revelada.
4. Ao terminar, copie o resultado em emojis para compartilhar, sem revelar a resposta.

Um novo Pokémon é sorteado à meia-noite (horário local).

## Funcionalidades

- **Sorteio determinístico:** o Pokémon do dia é calculado a partir da data com um hash FNV-1a, então todos os jogadores recebem o mesmo Pokémon no mesmo dia, sem precisar de servidor.
- **Todos os 1025 Pokémon** da Pokédex nacional (gerações 1 a 9).
- **Autocompletar** com navegação pelo teclado (↑ ↓ Enter).
- **Progresso salvo** no `localStorage`: recarregar a página não perde os palpites do dia.
- **Resultado compartilhável** em emojis.
- **Contagem regressiva** para o próximo Pokémon.
- **Funciona offline:** os dados e os sprites ficam embutidos no arquivo, sem chamadas de rede durante o jogo.

## Tecnologias

- HTML, CSS e JavaScript puro, sem frameworks nem dependências.
- Dados da PokéAPI (CSVs do repositório [PokeAPI/pokeapi](https://github.com/PokeAPI/pokeapi)) e sprites de [PokeAPI/sprites](https://github.com/PokeAPI/sprites).

## Como rodar

Não precisa instalar nada. Clone o repositório e abra o arquivo no navegador:

```bash
git clone https://github.com/<seu-usuario>/pokemon-do-dia.git
cd pokemon-do-dia
# abra pokemon-do-dia.html no navegador
```

Para publicar com o **GitHub Pages**, renomeie o arquivo para `index.html`. Depois, em *Settings → Pages*, selecione a branch `main` e a pasta raiz.

## Como funciona o sorteio

```js
function dailyId(key) {          // key = "AAAA-MM-DD"
  let h = 0x811c9dc5;            // FNV-1a
  for (const c of key) { h ^= c.charCodeAt(0); h = Math.imul(h, 0x01000193); }
  return ((h >>> 0) % 1025) + 1; // id entre 1 e 1025
}
```

A data local vira uma string, o hash é calculado e o resultado é mapeado para um id da Pokédex. Como a função é determinística, não é necessário backend para sincronizar os jogadores.

## Próximos passos

- [ ] Nomes e tipos em português
- [ ] Modo infinito (sem limite diário)
- [ ] Filtro por geração
- [ ] Estatísticas de vitórias e sequência de dias

## Aviso legal

Projeto de fã, sem fins comerciais, feito para estudo. Pokémon e os nomes dos personagens são marcas registradas da Nintendo, Game Freak e The Pokémon Company. Os dados e as imagens vêm da PokéAPI.
