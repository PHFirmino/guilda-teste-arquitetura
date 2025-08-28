
# Guilda de Teste

## O que é uma Guilda
Uma Guilda é um momento de troca de ideias, não uma palestra.
O objetivo é compartilhar conhecimento, discutir problemas e propor soluções juntos.

## Foco desta Guilda
O foco desta guilda é em **testes de UI**, ou seja, como podemos desenvolver melhor o front-end para facilitar os testes automatizados, tornando-os mais rápidos, legíveis e confiáveis.

Também será proposta uma mudança na esteira de desenvolvimento, acrescentando as colunas **Ready for Test** e **Testing**, ou substituindo as colunas de *Reviews*.

## O que é Teste de UI
Teste de UI valida a **interface que o usuário final utiliza**.

O teste de UI verifica:
- Fluxos principais (exemplo: criação de Analytics);
- Interações do usuário (exemplo: clique em botões, preenchimento de formulários, seleção de dropdowns);
- Mensagens de feedback ao usuário (exemplo: alertas de sucesso ou erro).

## Problemas que temos hoje
1. **IDs e Classes que mudam constantemente**
   No Angular/PrimeNG, muitos IDs são gerados automaticamente (`id="ui-4-label"`).
   Esses valores mudam quando novos elementos são adicionados na tela, fazendo com que o teste quebre facilmente.

   Problema:
   ```html
   <label id="ui-4-label">Email</label>
   <input id="ui-4" type="text" class="p-inputtext p-component" />
   ```

   Solução, utilizar `data-testid` com um valor:
   ```html
   <label id="ui-4-label" for="email-input">Email</label>
   <input id="email-input" type="text" class="p-inputtext p-component" data-testid="email-input" />
   ```

2. **Elementos sem identificadores claros**
   Muitos elementos não possuem identificadores únicos, dificultando para encontrar o elemento.

   Problema:
   ```html
   <button class="p-button p-component">Salvar</button>
   ```

   Solução, utilizar `data-testid` com um valor:
   ```html
   <button class="p-button p-component" data-testid="save-button">Salvar</button>
   ```

3. **Pegar elementos por texto**
   Selecionar elementos apenas pelo texto não é confiável:
   - Se o sistema for traduzido para outro idioma, o teste quebra;
   - Nem sempre o texto é único na tela.

   Problema:
   ```html
   <button class="p-button p-component">Novo</button>
   ```

   Solução, utilizar `data-testid` com um valor:
   ```html
   <button class="p-button p-component" data-testid="create-button">Criar</button>
   ```

## Uso de `data-testid` com Playwright
Configurando Playwright:
```ts
use: {
  testIdAttribute: 'data-testid',
}
```
Exemplo de uso:
Sempre que usar o método `getByTestId("save-button")` o Playwright vai buscar o elemento que possui `data-testid="save-button"`.
```ts
await page.getByTestId('save-button').click();
```

## Boas práticas para `data-testid`
- Valor do atributo deve refletir a função do elemento.
  Exemplos:
  - Botão de salvar: `data-testid="save-button"`
  - Input de email: `data-testid="email-input"`
  - Modal de confirmação: `data-testid="confirm-modal"`

- **Elemento único na tela**:
```html
<button data-testid="login-button">Entrar</button>
```

- **Elemento repetido na tela** (colocar no pai e também nos filhos):
```html
<div data-testid="user-card">
  <span data-testid="user-name">Pedro</span>
  <button data-testid="edit-button">Editar</button>
</div>
```

- **Registro de tabela** (usar ID único do registro):
```html
<tr data-testid="user-123">
  <td>Pedro</td>
  <td>QA</td>
  <td><button data-testid="delete-button">Excluir</button></td>
</tr>
```

## Quando utilizar `data-testid`
- Sempre que o elemento será usado em testes.
- Nem todos os elementos precisam, mas o padrão de desenvolvimento deve incluir esse atributo.

Benefícios:
- Criação de testes mais rápida;
- Testes legíveis;
- Testes mais confiáveis.

## Proposta de melhoria na esteira (Jira)
- Criar colunas **Ready for Test** e **Testing** (ou substituir as colunas de *Reviews*).
- Fluxo proposto:
  - Depois que o card está na arquitetura, o dev move para **Ready for Test**, indicando quem vai testar;
  - Quando QA inicia o teste, move para **Testing**;
  - Depois de testado, move para **Deploy**.

Benefícios:
- Nenhuma demanda passa sem QA olhar;
- Evita que o QA fique “no escuro”;
- Garante que todos os cards sejam testados corretamente;

## Conclusão
- Adotar `data-testid` como padrão torna os testes mais confiáveis, rápidos e legíveis.
- Com a esteira atualizada, temos melhor visibilidade e controle de qualidade antes das entregas.
- Qualidade é responsabilidade de toda a equipe.

## Perguntas
- Alguém tem alguma dúvida ou queira dar uma sugestão?
