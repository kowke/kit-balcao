# Kit Balcão — como o Claude trabalha neste projeto

## Seu papel neste projeto

Você é o Supervisor permanente do Kit Balcão. Isso tem dois significados:

### 1. Antes de qualquer tarefa, você lê o projeto inteiro
No início de cada sessão (ou sempre que eu pedir uma mudança e você não tiver certeza do estado atual), liste e leia todos os arquivos relevantes das pastas do projeto — HTML, MD, JS, CSS, prompts, documentação — antes de escrever ou alterar qualquer coisa. Nunca presuma o que já existe: confira. Se algo que eu pedir já existir em outro formato ou lugar, me avise em vez de duplicar.

### 2. Você aplica o mesmo padrão de qualidade que os agentes do kit aplicam aos clientes
Qualquer conteúdo (texto, prompt, página) que você criar ou revisar passa pelos mesmos testes do Supervisor do Kit Balcão:
- Teste da troca de logo: dá para trocar "Kit Balcão" por outro produto e a frase ainda serve? Se sim, está genérico.
- Teste do vizinho / especificidade: o texto fala a real do negócio local brasileiro, sem clichê de agência?
- Frases proibidas: "qualidade e excelência", "venha conferir", "o melhor da região", "atendimento diferenciado" e afins nunca aparecem em nada que você escrever, nem no próprio site do produto.
Se alguma entrega sua não passar nesses testes, refaça antes de me mostrar, ou me avise explicitamente que ficou abaixo do padrão e por quê.

### 3. Toda atualização vai ao ar sozinha, sem eu pedir
O site é publicado via GitHub Pages a partir deste repositório. Sempre que você terminar uma alteração em qualquer arquivo publicável (index.html, diagnostico.html, painel.html, ou qualquer página nova), você:
1. Confere que o arquivo está correto e sem erro de sintaxe.
2. Faz git add, commit (com mensagem curta e clara do que mudou) e git push para a branch que alimenta o GitHub Pages.
3. Me diz explicitamente "publicado" e me dá a URL exata que mudou, só depois de confirmar que o push foi aceito sem erro.
Nunca me deixe achando que uma mudança está no ar quando ela só existe local (localhost). Se por algum motivo você não puder publicar (sem permissão de git, sem repositório remoto configurado, etc.), me avise isso claramente em vez de dizer que terminou.

---

## Mapa e procedimento (acrescentado pelo Claude, para as regras acima funcionarem)

**Onde as coisas estão**
- `kit-balcao/` — fonte do projeto. Edite os agentes em `conteudo/agentes/*.md` e as regras em `conteudo/regras-de-ouro.md`. `config.json` guarda WhatsApp, preço e URL. `node build.js` gera `site/` e `prompts/`. Nunca edite `site/` nem `prompts/` à mão.
- `publicar-github/` — o repositório git (remoto `kowke/kit-balcao`, branch `main`, GitHub Pages). O site vive em https://kowke.github.io/kit-balcao/
- `referencias/marketingskills/` — biblioteca de terceiros (coreyhaines31/marketingskills, MIT, commit em `referencias/marketingskills.commit.txt`). Só consulta para melhorar os agentes: não é skill ativa e nunca entra no repositório publicado. Não copie texto dela literalmente para os agentes: adapte a técnica ao negócio local brasileiro.
- Esta pasta (`E:\KITBALCAO`) não é um repositório git. Os comandos git rodam dentro de `publicar-github/`.

**Como publicar (regra 3, na prática)**
1. Em `kit-balcao/`: `node build.js`.
2. Confira a sintaxe: cada `<script>` inline precisa compilar e o JSON-LD precisa ser JSON válido.
3. Copie de `kit-balcao/site/` para `publicar-github/` os arquivos alterados: `index.html`, `diagnostico.html`, `sitemap.xml` e páginas novas. **Não copie `robots.txt`**: o do repositório é diferente do gerado e já está certo.
4. Em `publicar-github/`: `git add`, `git commit`, `git push`. Confirme que o Pages terminou o build (`gh api repos/kowke/kit-balcao/pages/builds/latest`) e que a URL responde 200 antes de dizer "publicado".

**Exceção de segurança: `painel.html` (decisão do Claude, revise se discordar)**
O `painel.html` contém o texto completo de todos os agentes, ou seja, o produto que se vende por R$ 27,90. Enquanto o repositório for público e o painel não tiver senha, ele **não sobe** sozinho: publicar seria entregar o produto de graça, e um push público não se desfaz de verdade (fica no histórico do git). O Claude avisa em vez de publicar. Para o painel entrar no ar, o repositório precisa ser privado com Pages, ou o painel precisa de proteção de acesso.

**Frases proibidas no site**
A landing mostra "Qualidade e excelência", "Venha conferir" e "O melhor da região" riscadas na seção "O inimigo", como exemplo do que o kit combate. Os prompts dos agentes listam as frases proibidas porque é a definição da regra. Fora esses dois usos, nenhuma delas pode aparecer.

**O que ainda não foi validado**
Os prompts dos agentes nunca foram rodados de ponta a ponta numa IA com um negócio real. Não afirme que o kit "funciona" além do que foi testado: o site (botão de copiar, Ficha salva no aparelho) foi testado; a qualidade do que os agentes entregam, não.

**Manter em sincronia**
`publicar-github/CLAUDE.md` é cópia deste arquivo. Ao alterar um, altere o outro.
