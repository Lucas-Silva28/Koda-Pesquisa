KODA PESQUISA — v1.3.0

NOVIDADES DESTA VERSÃO:

1) IMPORTAÇÃO — suporte ao formato da Tabela FIT
   O importador de JSON agora também reconhece arquivos no formato
   "categorias > marca > produtos" (o mesmo do seu backup completo),
   além do formato simples já suportado (lista de produtos ou
   {"products":[...]}). Testado com o arquivo de 303 produtos enviado:
   todos os 303 foram reconhecidos e validados corretamente.
   IMPORTANTE: nenhum produto foi embutido no código. O banco continua
   iniciando vazio — para carregar os produtos, use Ferramentas >
   Importar e selecione o arquivo JSON.

2) CONEXÃO DE CONTA (sincronização entre aparelhos)
   Novo ícone de pessoa (👤) no topo da tela. Ao conectar com uma conta
   Google, os produtos, categorias, histórico e lixeira passam a ser
   sincronizados com a nuvem (Firebase) e ficam disponíveis em
   qualquer aparelho conectado com a mesma conta.
   - Sem conta conectada: tudo continua funcionando 100% local, como
     antes.
   - Ao conectar pela primeira vez, se houver produtos no aparelho E
     na nuvem, o sistema pergunta o que fazer antes de sincronizar
     (nunca sobrescreve dados sem confirmação).
   - Estrutura no Firestore: coleção "usuarios", 1 documento por conta
     (uid), sincronizado com merge (não sobrescreve o documento
     inteiro).

   ⚠️ PARA ATIVAR: este arquivo já vem com a estrutura pronta, mas com
   um FIREBASE_CONFIG de exemplo (placeholder). Antes de usar a
   conexão de conta, você precisa:
   a) No topo do <script>, substituir o objeto FIREBASE_CONFIG pelos
      dados reais do seu projeto Firebase (apiKey, authDomain,
      projectId, storageBucket, messagingSenderId, appId).
   b) No Firebase Console, ativar o provedor "Google" em
      Authentication > Sign-in method.
   c) Autorizar o domínio onde o sistema será hospedado em
      Authentication > Settings > Authorized domains.
   d) Criar o Firestore (modo produção) e aplicar esta regra de
      segurança:
        match /usuarios/{uid} {
          allow read, write: if request.auth != null && request.auth.uid == uid;
        }
   Sem esses três ajustes, o botão de conta mostra um aviso e o
   sistema continua funcionando normalmente apenas no modo local.

3) TUTORIAL COMPLETO
   Novo botão "Tutorial" em Ferramentas, com um guia passo a passo de
   todas as funções do sistema (pesquisa, cadastro, edição, exclusão/
   lixeira, categorias, histórico, importar/exportar, diagnóstico e
   conexão de conta).

4) LISTA DE PRODUTOS — REFINAMENTO v1.3.0
   A lista foi redesenhada de forma incremental: itens mais compactos,
   melhor hierarquia entre nome, preço, categoria e dados secundários,
   identificação cromática suave por categoria e ações mais discretas.

5) UX/UI — REFINAMENTO v1.3.0
   - Estado vazio orienta o primeiro cadastro sem inserir produtos fictícios.
   - Estado de pesquisa sem resultados oferece recuperação rápida.
   - Botão de limpar pesquisa aparece somente quando há texto.
   - Edição ganhou identificação visual mais clara.
   - Rodapé passou a exibir a versão técnica v1.3.0.
   - Tema claro/escuro foi preservado e as cores de categoria usam
     tratamento suave para manter legibilidade.

6) FIREBASE — PREPARAÇÃO
   O Firebase permanece opcional. Com as credenciais placeholder atuais,
   nenhuma biblioteca externa é carregada e o sistema continua local.
   Quando o FIREBASE_CONFIG real for fornecido, a estrutura existente
   poderá ser ativada sem tornar o Firebase obrigatório.

FUNCIONALIDADES QUE CONTINUAM (validadas nesta versão):
- Pesquisa por código, nome e preço, com filtro por categoria.
- Cadastro, edição e exclusão (para a Lixeira, com restauração).
- Categorias com cor.
- Histórico completo de operações.
- Importação (JSON/CSV) e Exportação (JSON/CSV).
- Diagnóstico do sistema.
- Tema claro/escuro.
- Persistência local via localStorage, com chaves versionadas.

ARMAZENAMENTO:
Local (localStorage) sempre. Nuvem (Firestore) apenas quando uma conta
está conectada — e mesmo assim, o local continua sendo atualizado
junto, como cache/backup.

COMO TESTAR:
1. Abra o index.html no navegador (mobile-first).
2. Ferramentas > Tutorial para conhecer todas as telas.
3. Ferramentas > Importar > selecione seu arquivo de backup de
   produtos para carregar a base.
4. Teste pesquisa, cadastro, edição, lixeira e categorias.
5. Se já tiver configurado o Firebase (ver item 2 acima), toque no
   ícone de pessoa para conectar a conta e testar a sincronização em
   outro aparelho/navegador.

Formato CSV aceito na importação:
codigo;nome;preco;categoria;marca
