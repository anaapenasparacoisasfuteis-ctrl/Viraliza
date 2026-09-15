# Viraliza V3 — banco, autenticação, upload, feed e ADM

Esta versão entrega o frontend completo já preparado para Supabase.

## O que a V3 faz
- autenticação real via Supabase Auth;
- cadastro com nome e tipo de conta;
- perfil persistente;
- trends persistentes no banco;
- feed persistente;
- upload de imagens/vídeos para Storage;
- posts persistentes;
- curtidas persistentes;
- comentários persistentes;
- métricas básicas;
- painel ADM protegido por `profiles.is_admin`;
- ADM pode cadastrar/excluir trends;
- ADM pode visualizar usuários;
- ADM pode moderar/remover posts;
- RLS (Row Level Security) no banco.

## Configuração — passo a passo
1. Crie um projeto no Supabase.
2. Abra o SQL Editor.
3. Execute `supabase/schema.sql`.
4. Execute `supabase/seed.sql`.
5. Em Storage, crie um bucket chamado `post-media` e deixe o bucket público para o protótipo.
6. Abra Project Settings > API e copie a Project URL e a chave `anon`/publishable.
7. Cole os dois valores em `config.js`.
8. Abra o `index.html` por um servidor local/host HTTPS. Para testes simples, uma extensão de Live Server no VS Code funciona.
9. Crie sua conta na Viraliza.
10. Para transformar essa conta em ADM, depois do cadastro execute no SQL Editor:
   `update public.profiles set is_admin=true where id=(select id from auth.users where email='SEU_EMAIL');`
11. Saia e entre novamente. O botão Painel ADM aparecerá.

## Segurança
A chave `anon`/publishable pode estar no frontend. A `service_role` NÃO pode. O controle de administrador é feito por RLS e pela coluna `is_admin` no banco.

## Observação sobre Storage
O código faz upload para `post-media` usando uma pasta com o UUID do usuário. Para uma publicação real, recomendamos configurar as políticas do Storage para permitir que usuários autenticados façam upload somente em sua própria pasta.

## Publicação
Depois de configurar o Supabase, a pasta pode ser publicada em Vercel, Netlify ou GitHub Pages. Como o site usa Supabase via HTTPS/CDN, não precisa de servidor próprio para o frontend.
