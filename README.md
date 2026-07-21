res_camex_publico
=================

Repositorio de artefatos estaticos publicados no GitHub Pages para visualizacao de resolucoes CAMEX.
As arvores `resolucoes/`, `documentos/` e `inlabs/` sao saidas de publicacao e nao constituem o pipeline
de coleta ou consolidacao.

Integracao com projetos relacionados
------------------------------------

| Projeto | Papel na integracao |
|---|---|
| `resolucoes_camex` | Gera os parquets finais e prepara/publica HTMLs ou PDFs neste clone. O caminho pode ser fornecido por `RESOLUCOES_CAMEX_GITHUB_PAGES_REPO_DIR` ou `--github-pages-repo-dir`. |
| `monitor_resolucoes_camex` | Orquestra a contingencia INLABS e confirma a disponibilidade publica dos artefatos antes de ativar o overlay no pipeline. |
| `res_camex_publico_inlabs_publish` | Clone dedicado deste mesmo remoto, usado pelo monitor para commits automaticos restritos a `inlabs/`. Nao e outro site nem outra fonte de dados. |

Este repositorio nao e necessario para executar o build local basico de `resolucoes_camex`. Ele se
torna necessario quando o fluxo precisa publicar artefatos ou reconhecer, pelo clone local, arquivos
ja disponiveis no GitHub Pages. A ausencia do clone nao deve interromper o build local, mas impede a
publicacao e pode fazer o pipeline escolher outra URL de fallback.

Operacao segura
---------------

- Trate o clone chamado `res_camex_publico` como o worktree principal para publicacoes manuais e manutencao do site.
- Use o clone chamado `res_camex_publico_inlabs_publish` para a autopublicacao INLABS do monitor.
- Os dois diretorios devem apontar para este mesmo remoto; o clone dedicado nao e um projeto separado.
- Nao execute dois publicadores simultaneamente sobre o mesmo worktree.
- Antes de publicar, confirme remoto, branch e estado limpo do worktree.
- Evite editar manualmente artefatos gerados; a fonte e a auditoria permanecem em `resolucoes_camex`.
- A publicacao automatica INLABS deve alterar somente os caminhos previstos em `inlabs/`.
- O clone dedicado e necessario apenas quando a autopublicacao INLABS esta habilitada e ha artefatos a publicar.
- Credenciais INLABS, estado shadow, parquets finais e arquivos de fonte nao devem ser armazenados neste repositorio.

Configuracao do clone dedicado INLABS
-------------------------------------

- `MONITOR_RESOLUCOES_CAMEX_INLABS_PUBLICATION_REPO_DIR`: caminho do clone `res_camex_publico_inlabs_publish`.
- `MONITOR_RESOLUCOES_CAMEX_INLABS_PUBLICATION_GIT_REMOTE_URL`: remoto esperado.
- `MONITOR_RESOLUCOES_CAMEX_INLABS_PUBLICATION_GIT_BRANCH`: branch de publicacao.
- `MONITOR_RESOLUCOES_CAMEX_INLABS_AUTO_PUBLISH=0`: desativa a autopublicacao quando o clone dedicado nao estiver disponivel.

Fluxo resumido
--------------

1. `resolucoes_camex` consolida os atos e prepara os artefatos.
2. Uma rotina de publicacao copia, valida e envia os arquivos para este remoto.
3. O GitHub Pages disponibiliza as URLs consumidas pelo painel.
4. No caso INLABS, o monitor usa o clone dedicado e confirma URL e hash antes de solicitar a ativacao do overlay.
