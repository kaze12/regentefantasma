# Regente Fantasma — Publicação do Obsidian com Quartz 5

## 1. Objetivo

Documentar a publicação do vault **Regente fantasma** como um site público e de leitura, utilizando Obsidian, Quartz 5, Quartz Syncer, GitHub e GitHub Pages.

A configuração deve permitir:

- Publicar notas tanto pelo Linux quanto pelo iPhone.
- Publicar por padrão os arquivos da pasta `site`.
- Manter a pasta `spoilers` fora da publicação.
- Preservar os arquivos originais no Obsidian.
- Atualizar o site automaticamente após os envios ao GitHub.

## 2. Arquitetura

```text
Obsidian — Regente fantasma
│
├── site/               → Conteúdo publicável
│   ├── index.md
│   ├── Regras/
│   └── 04 Locais/
│
└── spoilers/           → Conteúdo privado
    └── Nunca publicar
```

Fluxo de publicação:

```text
Obsidian (Linux ou iPhone)
          │
          ▼
     Quartz Syncer
          │
          ▼
GitHub — kaze12/regentefantasma
          │
          ▼
      GitHub Actions
          │
          ▼
        Quartz 5
          │
          ▼
       GitHub Pages
          │
          ▼
kaze12.github.io/regentefantasma
```

**Importante:** o Quartz Syncer sincroniza conteúdo do Obsidian para o repositório, não do repositório para o Obsidian. A sincronização das notas entre dispositivos é um processo separado.

## 3. Configurações do projeto

| Propriedade | Valor |
|---|---|
| Site | https://kaze12.github.io/regentefantasma/ |
| Repositório | https://github.com/kaze12/regentefantasma |
| Branch | `v5` |
| Gerador | Quartz 5 |
| Plugin Obsidian | Quartz Syncer 2.2.0 |
| Hospedagem | GitHub Pages |
| Publicação | GitHub Actions |
| Diretório publicável | `site` |
| Diretório privado | `spoilers` |
| Diretório de destino no repositório | `content` |

### 3.1. Caminhos no Linux

Vault identificado durante a configuração:

```text
/mnt/taiwan_com/Offline/icloud/Obsidian/Regente fantasma
```

Projeto Quartz:

```text
/mnt/taiwan_com/t_rpg/t_regente_fantasma
```

A configuração do Quartz Syncer utiliza o nome relativo `site`, não o caminho absoluto da pasta.

**Nota:** confirmar que o Obsidian está utilizando o mesmo vault que recebe as alterações do iPhone. Também foi identificado anteriormente um caminho de acesso ao iCloud em `/home/icm/Nuvens/icloud/Obsidian/Regente fantasma`.

## 4. Política de publicação e privacidade

### 4.1. Conteúdo público

Todo arquivo destinado ao site deve permanecer dentro de:

```text
Regente fantasma/site/
```

O Quartz Syncer deve utilizar:

| Configuração | Valor |
|---|---|
| Vault root folder | `site` |
| All notes publishable by default | On |

Com essas opções, todas as notas elegíveis dentro de `site` podem ser publicadas sem adicionar `publish: true`.

### 4.2. Conteúdo privado

Todo conteúdo que não deve ser publicado deve permanecer em:

```text
Regente fantasma/spoilers/
```

Essa pasta não pode aparecer entre os arquivos enviados ao GitHub.

**Atenção:** com `All notes publishable by default` ativado, o Quartz Syncer ignora a propriedade `publish`. Portanto, `publish: false` **não impede a publicação** de uma nota que esteja dentro de `site`.

Para manter uma nota privada, mova-a para fora de `site`, preferencialmente para `spoilers`.

### 4.3. Repositório público

O repositório GitHub está público.

Isso significa que qualquer arquivo enviado pode ser consultado diretamente pelo GitHub, independentemente de aparecer na navegação do site.

Excluir posteriormente uma nota do site não elimina automaticamente todas as cópias que possam existir no histórico do Git.

**Regra:** revisar o escopo de publicação antes de enviar material privado pela primeira vez.

## 5. Configuração no Linux

### 5.1. Abrir o Quartz Syncer

Caminho completo:

**Obsidian → Settings → Community plugins → Quartz Syncer**

### 5.2. Configurar a conexão Git

Caminho completo:

**Obsidian → Settings → Community plugins → Quartz Syncer → Git**

| Campo | Valor |
|---|---|
| Provider | GitHub |
| Remote URL | `https://github.com/kaze12/regentefantasma.git` |
| Branch | `v5` |
| Authentication type | Username & token/password |
| Username | `kaze12` |
| Access token | Token pessoal do GitHub |
| Vault root folder | `site` |

Use **Test connection** para verificar a permissão de leitura e escrita.

### 5.3. Configurar o Quartz

Caminho completo:

**Obsidian → Settings → Community plugins → Quartz Syncer → Quartz**

| Campo | Valor |
|---|---|
| Content folder | `content` |
| Local Quartz repo path | `/mnt/taiwan_com/t_rpg/t_regente_fantasma` |

Na tela inicial do Quartz Syncer, manter **Publish to → Remote repository**.

### 5.4. Configurar o Frontmatter

Caminho completo:

**Obsidian → Settings → Community plugins → Quartz Syncer → Frontmatter**

Ativar:

`All notes publishable by default`

Isso torna todas as notas da pasta `site` elegíveis para publicação.

## 6. Configuração no iPhone

O Quartz Syncer pode publicar diretamente do iPhone para o GitHub, sem depender de o Linux estar ligado.

### 6.1. Instalar o Quartz Syncer

Caminho completo:

**Obsidian → Settings → Community plugins → Browse → Quartz Syncer → Install → Enable**

Se já estiver instalado, apenas verifique se está ativado.

### 6.2. Configurar o GitHub

Caminho completo:

**Obsidian → Settings → Community plugins → Quartz Syncer → Git**

Preencher:

| Campo | Valor |
|---|---|
| Provider | GitHub |
| Remote URL | `https://github.com/kaze12/regentefantasma.git` |
| Branch | `v5` |
| Authentication type | Username & token/password |
| Username | `kaze12` |
| Access token | Token autorizado |
| Vault root folder | `site` |

Salvar a configuração.

O iPhone não utiliza automaticamente o token armazenado no Linux.

Se a conexão mostrar a mensagem:

> Connected read-only. No token is configured, so publishing will fail.

Significa que o repositório pode ser consultado, mas não há credencial salva para enviar alterações.

### 6.3. Criar o token pelo iPhone

No Safari, abrir:

https://github.com/settings/personal-access-tokens/new

Configurar:

| Campo | Valor |
|---|---|
| Token name | `Quartz Syncer iPhone` |
| Resource owner | `kaze12` |
| Repository access | Only select repositories |
| Selected repository | `regentefantasma` |
| Repository permissions → Contents | Read and write |

Para o envio de notas ao repositório já existente, não é necessário conceder permissões administrativas.

Após clicar em **Generate token**, copiar a credencial e armazená-la em local seguro, como um gerenciador de senhas.

O GitHub não permite visualizar novamente o valor completo de um token depois da criação.

### 6.4. Salvar e testar o token

Caminho completo:

**Obsidian → Settings → Community plugins → Quartz Syncer → Git → Access token → Save**

Depois:

**Obsidian → Settings → Community plugins → Quartz Syncer → Git → Connection test → Test connection**

O resultado esperado é acesso de leitura e escrita.

Se o teste continuar indicando `read-only`, verificar se o token foi efetivamente salvo e se possui a permissão **Contents: Read and write**.

### 6.5. Configurar a publicação por padrão

Caminho completo:

**Obsidian → Settings → Community plugins → Quartz Syncer → Frontmatter → All notes publishable by default → On**

Confirmar também:

**Obsidian → Settings → Community plugins → Quartz Syncer → Git → Vault root folder → `site`**

A segunda configuração é a responsável por excluir `spoilers` do escopo.

## 7. Publicar notas

### 7.1. Pelo Linux

Caminho completo:

**Obsidian → Command palette → Quartz Syncer: Open Publication Center**

Ou utilizar o ícone do Quartz Syncer na barra lateral.

No **Publication Center**:

1. Abrir as categorias `UNPUBLISHED` e `CHANGED`.
2. Conferir os arquivos identificados.
3. Selecionar as notas desejadas.
4. Clicar em **Publish to remote**.
5. Conferir a conclusão da publicação.

### 7.2. Pelo iPhone

Caminho completo:

**Obsidian → Command palette → Quartz Syncer: Open Publication Center**

No **Publication Center**:

1. Selecionar as notas de `site` com alterações pendentes.
2. Tocar em **Publish to remote**.
3. Manter o Obsidian aberto até o término do envio.

A publicação direta pelo iPhone não exige que o Linux esteja ligado.

### 7.3. Conferir o resultado

Abrir:

https://github.com/kaze12/regentefantasma/actions

Verificar se o workflow de implantação terminou com sucesso.

Depois, acessar:

https://kaze12.github.io/regentefantasma/

As notas poderão aparecer no **Explorer**, na busca do Quartz ou nos links internos do site.

Uma nota publicada não aparece necessariamente na página inicial: isso depende dos links existentes em `index.md`.

## 8. Imagens e anexos

O Quartz Syncer publica mídias vinculadas às notas selecionadas, incluindo imagens referenciadas em Markdown.

Arquivos sem vínculos podem não ser enviados automaticamente.

Na configuração inicial do projeto foi identificada esta imagem:

```text
site/04 Locais/T. Livres 2026-10-13-21-52.png
```

Tamanho aproximado: 5,8 MiB.

A publicação desse anexo ainda precisa ser confirmada. Verificar a seção **Advanced → Linked media** do Publication Center e conferir se o arquivo existe no repositório após o envio.

Arquivos grandes podem atingir limites da hospedagem ou do GitHub. Manter os anexos que não serão publicados fora da pasta `site`.

## 9. Publicação automática

### 9.1. Atualização do site

Sempre que o Quartz Syncer envia alterações à branch `v5`, o GitHub Actions executa o processo de construção e implantação do Quartz.

Essa etapa já está configurada no repositório.

### 9.2. Publicação automática pelo Linux

O Quartz Syncer também oferece publicação automática por intervalo no desktop.

Caminho completo:

**Obsidian → Settings → Community plugins → Quartz Syncer → Performance → Auto-publish interval**

Exemplos:

| Valor | Funcionamento |
|---|---|
| `0` | Desativado |
| `15` | A cada 15 minutos |
| `60` | A cada 60 minutos |

O recurso exige que o Obsidian esteja em execução. A publicação automática fica pausada enquanto o Publication Center estiver aberto.

**Estado:** a automação por intervalo ainda não foi confirmada como ativada neste projeto.

### 9.3. Limitação no iPhone

O Quartz Syncer para iPhone permite publicar manualmente.

A opção **Auto-publish interval** é exclusiva do desktop. Portanto, o iPhone não publica automaticamente em segundo plano apenas porque uma nota foi editada.

## 10. Diagnóstico de problemas

| Problema | Causa possível | Solução |
|---|---|---|
| `Connected read-only` | Token ausente ou não salvo | Configurar **Access token** |
| Falha de autorização | Token expirado ou com permissão insuficiente | Renovar token com **Contents: Read and write** |
| Nota não aparece no Publication Center | Está fora de `site` ou não foi reconhecida como publicável | Conferir **Vault root folder** e **Frontmatter** |
| Nota publicada não aparece no site | Build pendente ou com falha | Conferir **GitHub Actions** |
| Anexo não aparece | Arquivo não foi reconhecido como mídia vinculada | Conferir **Advanced → Linked media** |
| Alterações do iPhone não aparecem no Linux | Sincronização entre dispositivos ainda não concluída | Conferir a sincronização do vault |
| O site não atualiza sozinho | Auto-publish desativado, Obsidian fechado ou envio pendente | Conferir **Performance** e **Publication Center** |

Se a publicação falhar em um dispositivo, não altere imediatamente o conteúdo do outro. Primeiro verifique o estado remoto e evite enviar edições concorrentes.

## 11. Segurança e manutenção

- Nunca armazenar tokens em notas Markdown, arquivos do repositório ou capturas públicas.
- Utilizar um token específico para cada dispositivo, preferencialmente.
- Conceder acesso apenas ao repositório `regentefantasma`.
- Renovar ou substituir tokens expirados.
- Revogar tokens de dispositivos perdidos.
- Manter `Vault root folder = site` em todos os dispositivos.
- Conferir a seleção de arquivos antes de publicações que envolvam conteúdo privado.
- Não presumir que um repositório público oferece privacidade para arquivos omitidos da navegação do site.
- Verificar periodicamente o histórico de execuções do GitHub Actions.

## 12. Estado conhecido da instalação

Conferido durante a preparação desta documentação:

- Quartz 5 configurado e publicado pelo GitHub Pages.
- Repositório `kaze12/regentefantasma`, branch `v5`.
- Workflow `.github/workflows/deploy.yml` existente.
- Arquivos publicados identificados: `content/index.md` e `content/Regras/Regras de Jornada.md`.
- Pasta `spoilers` não identificada nos arquivos atuais da branch `v5`.
- Publicação direta pelo iPhone disponível, mas a autenticação de escrita no aparelho ainda precisa de confirmação.
- Publicação automática por intervalo ainda precisa ser configurada ou validada.
- Publicação da imagem de Terras Livres ainda não confirmada.

## 13. Referências

- [Site Regente Fantasma](https://kaze12.github.io/regentefantasma/)
- [Repositório GitHub](https://github.com/kaze12/regentefantasma)
- [GitHub Actions](https://github.com/kaze12/regentefantasma/actions)
- [Quartz — Documentação](https://quartz.jzhao.xyz/)
- [Quartz Syncer — Setup Guide](https://github.com/saberzero1/quartz-syncer/blob/main/docs/Setup%20Guide.md)
- [Quartz Syncer — Usage Guide](https://github.com/saberzero1/quartz-syncer/blob/main/docs/Usage%20Guide.md)

# Glossário

### === Obsidian ===

**Vault:** conjunto de notas, anexos e configurações do Obsidian.

**Frontmatter:** metadados no início de uma nota Markdown.

**Vault root folder:** pasta do vault autorizada para publicação.

**Community plugins:** área de instalação e configuração de plugins de terceiros.

**Command palette:** menu de comandos do Obsidian.

### === Quartz Syncer ===

**Remote repository:** destino remoto utilizado para enviar o conteúdo.

**Publication Center:** interface para selecionar, publicar e remover conteúdo remoto.

**UNPUBLISHED:** nota elegível ainda não publicada.

**CHANGED:** nota publicada com modificações pendentes.

**Linked media:** anexos vinculados a notas publicadas.

**All notes publishable by default:** torna todas as notas dentro do escopo elegíveis à publicação, ignorando a propriedade `publish`.

**Auto-publish interval:** frequência de publicação automática no desktop.

### === GitHub ===

**Repository:** repositório de arquivos e histórico de alterações.

**Branch:** ramificação utilizada para organizar versões do conteúdo.

**Personal Access Token:** credencial de acesso ao GitHub.

**Contents: Read and write:** permissão para ler e modificar arquivos de um repositório.

**GitHub Actions:** serviço que executa o processo de implantação do Quartz.

**GitHub Pages:** serviço de hospedagem do site público.

### === Quartz ===

**Quartz 5:** gerador do site estático a partir das notas Markdown.

**`content`:** pasta de entrada utilizada pelo Quartz.

**`index.md`:** arquivo Markdown da página inicial.

**Build:** transformação das notas em páginas e arquivos estáticos.

**Deploy:** disponibilização da versão gerada no serviço de hospedagem.

**Explorer:** navegação pelas notas e pastas do site.
