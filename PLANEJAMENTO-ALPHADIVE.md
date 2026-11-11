# Planejamento — Migração Alpha Dive Cabo Frio

WordPress (backup offline) -> site estático de página única

- Projeto: `site-estatico-alphadive`
- Backup de origem: `C:\Users\dayan\Downloads\migracao-sites\alphadive\`
- Data: 2026-11-10
- Status: planejamento (nenhum arquivo do site foi criado ainda)

---

## 1. Objetivo

Recriar o site `alphadive.com.br` como site estático de página única, mantendo o visual, as cores, a logomarca, os links e o conteúdo do site original. O servidor Hostgator foi desligado e não existe site online; a única fonte é o backup completo (arquivos + banco de dados).

Referências de padrão: `site-estatico` (LD Line) e `site-estatico-rodrigo` (Rodrigo Sant'Ana).

## 2. Premissas e restrições

- Sem site online: todo o material vem do backup `backup-alphadive.com.br-10-20-2026.tar.gz` e do dump `alpha_alphadiv_wp_s6iqe.sql.gz`.
- Liberdade para reduzir o número de páginas: consolidar Home, Cursos, Sobre Nós, Galeria, Depoimentos e Contato em uma única página com âncoras.
- Manter contatos, WhatsApp, e-mail, endereço, redes sociais e links externos (RAID/DiveRAID, SDI, Linktree).
- Deploy previsto em GitHub Pages com domínio próprio `alphadive.com.br` (a confirmar).
- Formulário de contato não terá backend PHP; será substituído por WhatsApp/Formspree/mailto.
- O backup e o WordPress local ficam fora do repositório publicado.

## 3. Inventário técnico do backup

### 3.1 Arquivos

| Arquivo | Tamanho | Conteúdo |
|---|---:|---|
| `backup-alphadive.com.br-10-20-2026.tar.gz` | 928 MB | Home cPanel completa, 40.612 entradas, `public_html` com WordPress, plugins, temas, uploads, backups Updraft |
| `alpha_alphadiv_wp_s6iqe.sql.gz` | 9,3 MB | Dump MySQL (60 MB descomprimido) |
| `aliases-alphadive.com.br.gz` | 65 B | Aliases de e-mail |

### 3.2 WordPress / banco

- Site: **Alpha Dive Cabo Frio** — "Operadora e Escola de Mergulho"
- `siteurl` / `home`: `https://alphadive.com.br`
- Idioma: `pt_BR`; permalink `/%postname%/`
- Front page: página ID **697** ("Homepage", slug `home`)
- Prefixo das tabelas: **`9RsWPC3_`**
- Tema ativo: **hello-elementor** (Hello Elementor)
- Elementor 3.33.4 + Elementor Pro 3.16.2
- Kit global ativo: **1213**

### 3.3 Plugins ativos (19)

akismet, awesome-contact-form7-for-elementor, click-to-chat-for-whatsapp, contact-form-7, elementor-pro, elementor, envato-elements, ewww-image-optimizer, flamingo, ga-google-analytics, gtranslate, header-footer-elementor, heartbeat-control, hummingbird-performance, premium-addons-for-elementor, ssl-insecure-content-fixer, updraftplus, wordfence, wordpress-seo, wps-hide-login.

### 3.4 Conteúdo no banco

| Tipo | Qtd. |
|---|---:|
| `revision` | 481 |
| `attachment` | 153 |
| `flamingo_inbound` | 69 |
| `wphb_minify_group` | 53 |
| `elementor_library` | 13 |
| `page` | 9 |
| `flamingo_contact` | 7 |
| `elementor-hf` | 4 |
| `post` | 3 |
| `nav_menu_item` | 3 |
| `wpcf7_contact_form` | 2 |

### 3.5 Páginas e templates relevantes

| ID | Título | Status | Slug | Template | Elementor |
|---:|---|---|---|---|---:|
| 697 | Homepage | publish | `home` | default | 45.036 |
| 683 | Contato | publish | `contato` | `elementor_canvas` | 24.554 |
| 677 | Cursos | draft | `cursos` | `elementor_canvas` | 45.063 |
| 4684 | Raid | draft | `raid` | `elementor_canvas` | 15.994 |
| 1185 | Home-FINAL | publish (library) | — | — | 136.576 |
| 1192 | CURSOS-FINAL | publish (library) | — | — | 120.062 |
| 1196 | FALE-FINAL | publish (library) | — | — | 75.920 |
| 92267 | Homepage 2025 | publish (library) | — | — | 61.454 |
| 92278 | Sobre Nos 2025 | publish (library) | — | — | 37.256 |
| 92292 | Contato 2025 | publish (library) | — | — | 24.570 |
| 92295 | Blog 2025 | publish (library) | — | — | 18.805 |

### 3.6 Menu

- Menu real: termo **4 "Principal"** (`nav_menu`), localização `menu-1`.
- Itens: Home (página 697), Cursos (página 677), Contato (página 683).
- Os templates 2025 referenciam `menu: "menu"` (slug inexistente); o menu pode ter renderizado vazio. No site novo, criar um menu com slug `menu` ou substituir por navegação própria com âncoras.

## 4. Fonte de verdade recomendada

Usar os **templates 2025** (conteúdo real e mais recente) e o **CURSOS-FINAL**:

- `92267` Homepage 2025 — hero, sobre, diferenciais, Cabo Frio, depoimentos, footer.
- `1192` CURSOS-FINAL — 6 cursos com textos completos e imagens locais.
- `92278` Sobre Nos 2025 — equipe, missão, visão.
- `92292` Contato 2025 — contatos, mapa, formulário CF7.
- `92295` Blog 2025 — 2 posts (opcional).

Descartar:

- Página **697** (Home antiga): contém placeholders de demonstração ("(123) 456-7890", "info@company.com", "Alpha Vibes").
- Página **677** (Cursos draft): usa imagens do staging `teste5.gabrielpacheco.com.br`.
- Templates `elementor-hf` (header/footer): todos draft, não usados.

## 5. Arquitetura proposta da página única

| Ordem | Seção | Âncora | Fonte | Conteúdo |
|---:|---|---|---|---|
| 1 | Header fixo | `#topo` | 92267 | Logo, menu (Home, Cursos, Sobre Nós, Galeria, Depoimentos, Contato, RAID), botão WhatsApp |
| 2 | Hero | `#home` | 92267 | Vídeo de fundo `Video-Capa.mp4`, título "Visite os mais belos pontos de mergulho de Cabo Frio conosco.", CTA |
| 3 | Sobre / A atividade de Mergulho | `#sobre` | 92267 | Texto histórico, 3 ícones (Treinamento, Segurança, Equipamento), mosaico de imagens |
| 4 | Diferenciais | `#diferenciais` | 92267 | 4 colunas: Lancha Alpha Dive, Operação Alpha Dive, Estação de recarga de ar comprimido e nitrox, Equipe |
| 5 | Cabo Frio | `#cabo-frio` | 92267 | Texto sobre a cidade + imagens |
| 6 | Cursos | `#cursos` | 1192 | 6 cards: Open Water, Advanced Open, Nitrox, Resgate, Dive Master, Free Diving |
| 7 | Sobre Nós / Equipe | `#equipe` | 92278 | 4 integrantes, missão, visão |
| 8 | Depoimentos | `#depoimentos` | 92267 | 9 depoimentos (carrossel ou grid) |
| 9 | Galeria | `#galeria` | 92267/92278 | 11 fotos com lightbox |
| 10 | Notícias / Blog (opcional) | `#noticias` | 92295 | 2 posts: "Mergulhe com Nitrox!" e "TRIP SCUBA" |
| 11 | Contato | `#contato` | 92292 | WhatsApp, e-mail, endereço, mapa, formulário |
| 12 | Footer | `#rodape` | 92267 | Logo, resumo, menu, RAID/QR, redes sociais, copyright |
| — | Botão flutuante WhatsApp | — | plugin original | Fixo no canto inferior direito |

### 5.1 Navegação

- Home -> `#topo` / `#home`
- Cursos -> `#cursos`
- Sobre Nós -> `#equipe`
- Galeria -> `#galeria`
- Depoimentos -> `#depoimentos`
- Contato -> `#contato`
- RAID -> link externo DiveRAID (afiliado)
- CTA WhatsApp -> `https://api.whatsapp.com/send?phone=5522981538480`

## 6. Design system

### 6.1 Cores globais (kit 1213)

| Papel | Cor |
|---|---|
| Primária | `#E60803` |
| Secundária | `#123267` |
| Texto | `#E4F4F5` |
| Accent | `#FFFFFF` |
| Custom | `#1F548C`, `#418EAC`, `#031A34`, `#58B8CE`, `#3BA6C6`, `#16F8F7`, `#D4FF66`, `#C2D3D4` |
| Seções escuras | `#252627` |
| Footer | `#212121` |
| Header (variações) | `#003156` / `#162245` |
| Botão teal (2025) | `#20C7CA` |

### 6.2 Tipografia

| Uso | Fonte | Tamanho / peso |
|---|---|---|
| Corpo | Poppins | 15 px, 1.5, `#676767` |
| H1 | Merriweather | 60 px, 700, `#222222` |
| H2 | Merriweather | 40 px |
| H3 | Merriweather | 32 px |
| Botões | Montserrat | 600 |

Recomendação: self-host dos arquivos woff2 (Poppins 300/400/500/600/700, Merriweather 400/700, Montserrat 600) para performance e privacidade.

### 6.3 Logomarca

- Header: `2021/11/Logo-Titulo.png` (usada nos templates 2025) ou `2025/09/alpha_id-simplificada-branca-hr.png` (1360x340, alta resolução).
- Ícone/favicon: `2022/02/alpha_id-completa-branca.png` (1700x1132), redimensionar para 512/192/32.
- Rodapé/RAID: `2023/10/RAID-WHITEdiveraid_horz.png`, `2023/09/raid.png`, `2023/09/QR-CODE-AD.png`, `2025/09/Transparent-SDI-Logo-Color.png`.

## 7. Conteúdo detalhado

### 7.1 Contato e links

- WhatsApp: `(22) 981773211` -> `https://api.whatsapp.com/send?phone=5522981538480`
- E-mail: `cassianoalphadive@gmail.com`
- Endereço: "Dentro da Marina Pirata - R. José Augusto Saraiva, n°5 - Ilha da Draga, Cabo Frio - RJ, 28922-587"
- Google Maps: `https://www.google.com/maps/place/Alpha+Dive+Cabo+Frio+Operadora+e+Escola+de+Mergulho/@-22.8749525,-42.025848,17z/...`
- RAID/DiveRAID: `https://diveraid.mobi/Home/RegisterUser?affiliate=QjQKn5lnniNYKstTScy6%2Bg%3D%3D`
- Facebook: `https://pt-br.facebook.com/alphadivecabofrio/`
- Instagram: `https://www.instagram.com/alphadivecabofrio/`
- Linktree: `https://linktr.ee/ALPHADIVECF`
- WhatsApp (social): `https://api.whatsapp.com/send?phone=5522981538480`

### 7.2 Cursos (6)

1. **Curso Básico — Open Water** — certificação internacional, habilidades fundamentais, respiração, flutuabilidade.
2. **Curso Avançado — Advanced Open** — mergulhos mais fundos, misturas gasosas, naufrágios, mergulho noturno.
3. **Nitrox / Enriched Air** — mistura enriquecida de oxigênio, limite não descompressivo, recarga e análise do gás.
4. **Mergulhador de Resgate** — detecção e prevenção de problemas, exercícios em ambiente real, planejamento e segurança.
5. **Dive Master** — primeira etapa profissional, operações, manutenção, física/fisiologia, tabelas, equipamentos, guia.
6. **Free Diving** — mergulho livre, técnicas com máscara, snorkel e nadadeiras.

Textos completos no dump (template 1192). Cada card terá CTA "Quero me inscrever" -> WhatsApp com o nome do curso.

### 7.3 Equipe (4)

| Nome | Função | Instagram |
|---|---|---|
| Cassiano | Comandante / Instrutor | `https://www.instagram.com/cassiano_ricardo_jordao/` |
| Raphael | Dive Master / Marinheiro | `https://www.instagram.com/raphael.alphadive/` |
| Waleria | Marinheira | `https://www.instagram.com/waleriacioli/` |
| Maria Fernanda | CEO | `https://www.instagram.com/nandaacioli_/` |

Missão: "apresentar o oceano às pessoas de uma forma mais íntima, através do desenvolvimento do mergulhador e suas características de educação e preservação ambiental."
Visão: "atingir o lado consciente do ser humano através da atividade de mergulho... colaborando para um presente mais responsável."

### 7.4 Depoimentos (9)

Roger Coelho, Rodrigo Honorato, Adriana Olandim, Alexandre Farago, Fernanda Sposato, Ubiracy G. Mello, nanook hinze, smartdiverj (operadora parceira), soutodorj.

Avatares: `2022/01/unnamed.png`, `unnamed-1.png`, `unnamed-2.png`, `unnamed-3.png` (4 primeiros); os demais sem imagem.

### 7.5 Galeria (11)

`2021/12/PSX_20200727_152756.jpg`, `20210314_093457-scaled-1.jpg`, `Copia-de-IMG_20190415_192053_627.jpg`, `IMG_20190606_061057_299.jpg`, `PSX_20200317_213416-scaled-1.jpg`, `IMG_20200809_025428_492.jpg`, `IMG_20211212_185051_180.jpg`, `61177906_1291577114328119_2842393520948379648_n.jpg`, `20201203_142941-scaled-1.jpg`, `2021/11/IMAGEM-3.jpg`, `2021/11/Imagem-1.jpg`.

### 7.6 Blog (opcional)

- "Mergulhe com Nitrox!" (2021-12-01) — texto curto sobre Nitrox incluído nas operações.
- "TRIP SCUBA" (2021-12-01) — link para vídeo de 12 minutos.
- Thumbs: `2021/12/20201203_142941-scaled-1.jpg` e `2022/02/Filmes-e-TV-16_12_2020-11_10_19.jpg`.

## 8. Assets e mapeamento

- Hero: `2021/11/Video-Capa.mp4` + poster `2021/11/video-capa.jpg`.
- Sobre: `2021/11/IMAGEM-2.jpg`, `IMAGEM-3.jpg`, `Imagem-1.jpg`, `IMAGEM-5.jpg`, `IMAGEM-9.jpg`, `Cabo-Frio-imagem-8-scaled-1.jpg`, `dots-green.png`; ícones `Icon-1.png`, `Icon-2a.png`, `Icon-3.png`.
- Diferenciais: `2022/03/alphapassodogigante.jpg`, `2021/11/IMAGEM-5.jpg`, `2022/02/IMG_20210606_201414_240-1-scaled-1.jpg`, `2022/02/VideoCapture_20220223-123357.jpg`.
- Cursos: `2022/02/PSX_20200727_152138.jpg`, `POLVO-1-1.jpg`, `VideoCapture_20210919-182449-1.jpg`, `20201203_142941-scaled-1.jpg`, `20210314_093457-scaled-1.jpg`, `VideoCapture_20220223-131925-1.jpg`.
- Equipe: `2022/02/GOPR5151.jpg`, `2021/12/IMG_20211212_185051_180.jpg`, `2022/02/IMG_20210308_200121_352-1.jpg`, `2022/02/Foto-de-Cassiano-Alpha-Dive.jpg`.
- Sobre Nós (fundos): `2022/02/Copia-de-Foto-de-Rapha-Jordao-2.jpg`, `2022/02/IMG_20210512_133400_873-scaled-1.jpg`, `2022/01/alpha_dive.jpg`.
- Missão/visão: `2022/02/PSX_20200317_213416-scaled-1.jpg`, `2022/02/IMG_20220220_204012_295.webp`, `2022/02/IMG_20220207_130702_604-scaled-1.jpg`.
- RAID: `2023/09/raid.png`, `2023/09/QR-CODE-AD.png`, `2023/10/RAID-WHITEdiveraid_horz.png`, `2025/09/Transparent-SDI-Logo-Color.png`.

### 8.1 Imagens faltantes

- `2021/11/Cabo-Frio-imagem-8-scaled.jpg` -> usar `Cabo-Frio-imagem-8-scaled-1.jpg`.
- `2021/12/20210314_093457-scaled.jpg` -> usar `20210314_093457-scaled-1.jpg` (template antigo 677).
- `2022/02/IMG_20220207_130702_604-scaled.jpg` -> usar `IMG_20220207_130702_604-scaled-1.jpg`.
- `2022/02/PSX_20200317_213416-scaled.jpg` -> usar `PSX_20200317_213416-scaled-1.jpg`.
- Logos de parceiros `logo-2.png`, `logo-4.png`, `logo-5.png`, `logo-8.png`, `logo-10.png`: **não existem no backup** (eram do staging). Omitir a seção "Parceiros" ou substituir por texto/logos SDI e RAID.

### 8.2 Otimização

- Converter JPG/PNG para WebP (qualidade 80), largura máx. 1600 px; manter fallback JPEG.
- `loading="lazy"` e `decoding="async"` em todas as imagens exceto hero.
- Hero: comprimir `Video-Capa.mp4` para 720p (H.264, sem áudio, ~2-4 MB) ou usar poster + play. Manter `preload="metadata"`, `muted`, `loop`, `playsinline`, `autoplay`.
- Minificar CSS/JS e remover código não usado (Elementor, plugins).
- Self-host de fontes e ícones (Font Awesome Free ou SVG inline).

## 9. Abordagem técnica

### 9.1 Opção A (recomendada) — WordPress local + espelhamento

Requer Docker Desktop em execução (atualmente parado). Espaço livre: 50,1 GB.

1. Extrair `./public_html` do tar para `_local/public_html` (fora do repositório).
2. Subir `docker-compose` com MySQL 8, WordPress PHP 8.1 e WP-CLI.
   - Ajustar `wp-config.php`: `DB_HOST 'db:3306'`, `FS_METHOD 'direct'`, `WP_HOME`/`WP_SITEURL` = `http://localhost:8080`.
   - Importar `alphadive.sql` no banco `alpha_alphadiv_wp_s6iqe`.
3. `wp search-replace` de `https://alphadive.com.br` e `https://teste5.gabrielpacheco.com.br` por `http://localhost:8080` em todas as tabelas.
4. Criar páginas com os templates 2025 usando `[elementor-template id="..."]` e template `elementor_canvas`:
   - `home-2025` (92267), `cursos-final` (1192), `sobre-nos-2025` (92278), `contato-2025` (92292), `blog-2025` (92295).
5. Criar menu com slug `menu` (Home, Cursos, Sobre Nós, Galeria, Depoimentos, Contato, RAID) para o widget de navegação renderizar.
6. Validar as páginas no navegador; corrigir imagens `teste5`, logos faltantes e o link `/raid`.
7. Espelhar com um container Alpine + GNU wget (não há wget no Windows):
   `wget --mirror --convert-links --adjust-extension --page-requisites --no-parent --restrict-file-names=windows --span-hosts --domains=wordpress,localhost http://wordpress/...`
8. Consolidar as seções em `index.html`:
   - Manter um header e um footer; remover duplicatas.
   - Ordem da tabela da seção 5.
   - Substituir o menu vazio pela navegação com âncoras.
   - Ajustar "Discover More" -> "Saiba Mais" e o link `/raid` -> afiliado DiveRAID.
9. Limpar cruft WordPress: `wp-admin`, `wp-json`, `wp-login.php`, `xmlrpc.php`, feeds, oEmbed, `admin-ajax.php`, PHP, `readme.html`, `license.txt`, `wp-config.php`, plugins/temas não usados, uploads não usados.
10. Otimizar assets (seção 8.2) e validar responsivo/Lighthouse.

Vantagem: fidelidade visual alta, aproveita o CSS/JS do Elementor e os textos exatos.
Risco: complexidade do ambiente local, regeneração de CSS do Elementor, dependência do Docker.

### 9.2 Opção B — Espelhamento com Python

Se o Docker não puder ser usado: subir o WordPress local de outra forma (ou usar um host temporário) e espelhar com um script Python (`urllib`/`requests` + `html.parser`), baixando HTML, CSS, JS e imagens e reescrevendo URLs. Mesmo resultado da Opção A, sem GNU wget.

### 9.3 Opção C — Reconstrução manual

Criar `index.html` + CSS/JS próprios usando os textos e assets do backup, reproduzindo cores, fontes e layout. Mais rápido e limpo, porém sem fidelidade pixel a pixel do Elementor. Útil se o WordPress local não for viável.

## 10. Formulário de contato

Formulário CF7 original (ID 652):

- Campos: Nome (`your-name`), E-mail (`your-email`), Assunto (`your-subject`), Mensagem (`your-message`).
- Destinatário original: `cassianoalphadive@gmail.com`.
- Remetente original: `contato@alphadive.com.br`.

Opções para o site estático:

1. **WhatsApp (recomendado)**: o botão monta a mensagem com nome, e-mail, assunto e texto e abre `wa.me/5522981538480`. Sem backend, boa conversão.
2. **Formspree/Web3Forms**: envia por e-mail; requer conta/chave e plano gratuito.
3. **mailto:**: abre o cliente de e-mail; simples, porém pior experiência em mobile.

Manter também os links diretos de WhatsApp, e-mail e mapa.

## 11. SEO, redirects e deploy

### 11.1 SEO

- Título: `Alpha Dive Cabo Frio – Operadora e Escola de Mergulho`.
- Descrição: "Mergulhe em Cabo Frio com a Alpha Dive: cursos Open Water, Advanced, Nitrox, Resgate, Dive Master e Free Diving, batismos, passeios de lancha e recarga de ar comprimido e nitrox. Fale conosco pelo WhatsApp."
- Canonical: `https://alphadive.com.br/`.
- Open Graph/Twitter com imagem do hero (`video-capa.jpg` ou `IMAGEM-2.jpg`).
- JSON-LD `LocalBusiness`/`SportsActivityLocation`: nome, endereço, telefone, e-mail, geo, sameAs (Facebook, Instagram, Linktree, WhatsApp), logo, imagem.
- `sitemap.xml` (página única) e `robots.txt` (allow all + sitemap).
- `lang="pt-BR"`.

### 11.2 Redirects das URLs antigas

Criar stubs com `<meta http-equiv="refresh">` + canonical + JS para:

- `/contato/` -> `/#contato`
- `/cursos/` -> `/#cursos`
- `/raid/` -> link externo DiveRAID
- `/home/`, `/homepage/` -> `/`
- `/sobre-nos/` -> `/#equipe`
- `/blog/` -> `/#noticias`
- `/mergulhe-com-nitrox/`, `/trip-scuba/` -> `/#noticias`

### 11.3 Deploy

- Repositório GitHub `site-estatico-alphadive`, branch `main`, GitHub Pages na raiz.
- `CNAME` = `alphadive.com.br`; `.nojekyll`; `404.html`.
- DNS (a confirmar provedor): `A` para `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`; `CNAME www` -> `<usuario>.github.io`.
- HTTPS automático do GitHub Pages.
- Testes: home, âncoras, WhatsApp, e-mail, mapa, formulário, mobile, Lighthouse, links antigos.

## 12. Fases e checklist

### Fase 0 — Inventário (concluída)

- [x] Ler backup, dump, tema, plugins, páginas e templates.
- [x] Extrair textos, cores, fontes, logo, contatos, redes sociais.
- [x] Mapear imagens e identificar faltantes.
- [x] Ler projetos de referência (LD Line e Rodrigo).

### Fase 1 — Conteúdo e decisões

- [ ] Confirmar fonte de design (templates 2025).
- [ ] Decidir blog, RAID, formulário, vídeo, copyright e idioma.
- [ ] Aprovar estrutura da página única.

### Fase 2 — Ambiente e espelhamento

- [ ] Iniciar Docker Desktop.
- [ ] Extrair `public_html`, subir WordPress + MySQL, importar dump.
- [ ] Ajustar URLs e criar páginas/menu.
- [ ] Espelhar as páginas e consolidar `index.html`.

### Fase 3 — Limpeza e otimização

- [ ] Remover cruft WordPress e plugins não usados.
- [ ] Converter imagens para WebP, comprimir vídeo, self-host de fontes.
- [ ] Minificar CSS/JS, revisar responsivo e acessibilidade.

### Fase 4 — SEO e deploy

- [ ] Metadados, JSON-LD, sitemap, robots, redirects.
- [ ] Criar repositório, `CNAME`, `.nojekyll`, `404.html`.
- [ ] Configurar DNS e HTTPS; validar produção.

## 13. Decisões pendentes

1. Fonte de design: templates 2025 (recomendado) ou Home antiga 697?
2. Blog/Notícias: incluir os 2 posts ou remover?
3. Formulário: WhatsApp (recomendado), Formspree/e-mail ou mailto?
4. RAID: link externo do afiliado (recomendado) ou recriar seção/página?
5. Deploy: GitHub Pages + `alphadive.com.br`? Quem tem acesso ao DNS/registro.br?
6. Vídeo do hero: manter vídeo comprimido (recomendado) ou usar imagem estática?
7. Copyright: atualizar para 2026 ou manter "2021"?
8. Idioma: somente `pt_BR` (remover GTranslate)?
9. Docker: pode ser iniciado para a abordagem fiel? Se não, usar reconstrução manual?

## 14. Riscos e mitigação

| Risco | Mitigação |
|---|---|
| Docker indisponível | Opção B (Python) ou Opção C (reconstrução manual) |
| CSS do Elementor não regenerar | Copiar `wp-content/uploads/elementor/css/` do backup; ou usar CSS inline |
| Imagens do staging ausentes | Usar equivalentes locais (`-scaled-1`) e omitir logos de parceiros |
| Menu vazio nos templates | Criar menu com slug `menu` ou substituir por nav própria |
| Vídeo pesado | Comprimir para 720p ou usar poster + play |
| Formulário sem backend | WhatsApp/Formspree/mailto |
| Redirects no GitHub Pages | Stubs HTML com meta refresh + JS |
| DNS sem acesso | Solicitar acesso ao registrador/provedor antes do deploy |

## 15. Referências

- `C:\Users\dayan\Downloads\migracao-sites\site-estatico\MIGRAR-WP-PARA-ESTATICO.md`
- `C:\Users\dayan\Downloads\migracao-sites\site-estatico-rodrigo\MIGRACAO-RODRIGO.md`
- `C:\Users\dayan\AppData\Local\Temp\opencode\alphadive\dump_extract.json`
- `C:\Users\dayan\AppData\Local\Temp\opencode\alphadive\content_2025.txt`
- `C:\Users\dayan\AppData\Local\Temp\opencode\alphadive\elementor_outline.txt`
- `C:\Users\dayan\AppData\Local\Temp\opencode\alphadive-tar-list.txt`
