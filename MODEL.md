# DigiOdonto — Modelo de Dados WordPress

Tema: `digiodonto`
Figma: https://www.figma.com/design/RkU0coI26Msec9PX3jJoIg/DigiOdonto---Site-Institucional
Frame raiz: `7010:279` (Desktop, 1440x7732)

## Estratégia
- Meta fields **nativos** (sem ACF) via `add_meta_box`.
- Conteúdo global (telefone, copy do hero, redes sociais) no **Customizer**.
- Conteúdo repetível em **CPTs**.
- Seeder one-click importa o conteúdo do Figma (copy + imagens dos `assets/`) na ativação do tema.
- Updates via **Git Updater** — `GitHub Theme URI` no `style.css`.

## CPTs

| Slug WP            | Label          | Campos principais                                                              | Origem (Figma section) |
|--------------------|----------------|--------------------------------------------------------------------------------|------------------------|
| `dgo_differential` | Diferenciais   | título, ilustração (SVG escolhido), descrição                                  | Differentials (4)      |
| `dgo_solution`     | Soluções       | título, descrição, imagem-capa (featured)                                      | Solutions (5)          |
| `dgo_area`         | Áreas de atuação | título, ícone (slug do set Phosphor), ordem                                  | Areas (5)              |
| `dgo_post` (use o `post` nativo) | Blog | título, categoria (taxonomy `category`), excerpt, featured           | Blog (3 cards)         |
| `dgo_partner`     | Parceiros/Logos | logo (featured), nome, URL                                                    | Hero logo strip (6)    |

> Optamos por usar `post` nativo para Blog em vez de CPT — categoria/tag/RSS prontos.

## Customizer (theme_mods)

| Chave                           | Tipo         | Uso                                       |
|---------------------------------|--------------|-------------------------------------------|
| `dgo_phone`                     | text         | Telefone principal                        |
| `dgo_email`                     | email        | Email contato                             |
| `dgo_whatsapp`                  | text         | Número WhatsApp (link wa.me)              |
| `dgo_address`                   | textarea     | Endereço (rodapé)                         |
| `dgo_hero_pretitle`             | text         | "ASSESSORIA DE PERFORMANCE" (vertical tag) |
| `dgo_hero_title`                | textarea     | Título do hero                            |
| `dgo_hero_subtitle`             | textarea     | Subtítulo do hero                         |
| `dgo_hero_cta_primary_label`    | text         | "Quero atrair mais pacientes"             |
| `dgo_hero_cta_primary_url`      | url          | URL do CTA primário                       |
| `dgo_hero_cta_secondary_label`  | text         | "Consultoria gratuita"                    |
| `dgo_hero_cta_secondary_url`    | url          | URL do CTA secundário                     |
| `dgo_about_pretitle`            | text         | "SOBRE A DIGIODONTO"                      |
| `dgo_about_title`               | textarea     | Título da seção Sobre                     |
| `dgo_about_body`                | textarea     | Corpo da seção Sobre                      |
| `dgo_about_image`               | image        | Foto da equipe (selo D)                   |
| `dgo_diff_title`                | textarea     | Título "O que diferencia a DigiOdonto"    |
| `dgo_diff_pretitle`             | text         | (pretitle vertical, se houver)            |
| `dgo_stats_pretitle`            | text         | "RESULTADOS" (vertical tag)               |
| `dgo_stats_title`               | textarea     | "Experiência consolidada em resultados"   |
| `dgo_stats_main_value`          | text         | "+ R$1 bilhão"                            |
| `dgo_stats_main_label`          | text         | "gerado em receita para nossos clientes"  |
| `dgo_stats_card1_value`         | text         | "+800" etc.                               |
| `dgo_stats_card1_label`         | text         |                                           |
| `dgo_stats_card2_value`         | text         | "+ R$ 350 mi"                             |
| `dgo_stats_card2_label`         | text         |                                           |
| `dgo_stats_card3_value`         | text         | "Presença"                                |
| `dgo_stats_card3_label`         | text         |                                           |
| `dgo_solutions_title`           | textarea     | "Soluções integradas para o crescimento da sua clínica" |
| `dgo_areas_pretitle`            | text         | "ATUAÇÃO" (vertical tag)                  |
| `dgo_areas_title`               | textarea     | "Como atuamos junto à sua clínica"        |
| `dgo_areas_intro`               | textarea     | parágrafo intro                           |
| `dgo_blog_title`                | textarea     | "Conexões sobre marketing e crescimento…" |
| `dgo_blog_subtitle`             | textarea     |                                           |
| `dgo_cta_title`                 | textarea     | "Pronto para estruturar o crescimento…"   |
| `dgo_cta_body`                  | textarea     |                                           |
| `dgo_footer_about`              | textarea     | Texto curto do rodapé                     |
| `dgo_footer_signature_url`      | url          | "Feito por A" — link Atorhe ou similar    |

## Páginas
- **Home** (`front-page.php`): renderiza todas as seções via `template-parts/`.
- **Sobre**, **Soluções**, **Blog**, **Contato**: criadas pelo seeder, usam `page.php` por enquanto.

## Seeder
`inc/seeder.php` roda 1x na ativação do tema (`after_switch_theme`) E é exposto em `admin.php?page=dgo-seed`. Ele:
1. Cria as páginas padrão (Home/Sobre/Soluções/Blog/Contato) e define `show_on_front=page` + `page_on_front=Home`.
2. Importa imagens da pasta `assets/seed/` para a biblioteca de Mídia, anexando-as como featured images.
3. Popula 4 `dgo_differential`, 5 `dgo_solution`, 5 `dgo_area`, 3 posts, 6 `dgo_partner` com a copy do Figma.
4. Aplica os defaults do Customizer.
5. Marca `option('dgo_seeded', 1)` para não rodar duas vezes (rodável de novo via página admin).

## Git Updater
`style.css` carrega:
```
GitHub Theme URI: https://github.com/<owner>/digiodonto
Primary Branch: main
Release Asset: true
```
