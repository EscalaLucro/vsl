# Como adicionar um novo vídeo (VSL) ao repositório (Windows)

Este guia mostra como, no **Windows**, pegar um arquivo `.mp4`, quebrar em HLS (`.m3u8` + `.ts`) com **FFmpeg** e publicar como uma nova VSL no repositório `dev-txt/vsl`.

---

## 1. Pré-requisitos (Windows)

### 1.1. Ter Git instalado

No Prompt de Comando ou PowerShell:

    git --version

Se não aparecer a versão, instale o Git pelo site oficial antes de continuar.

### 1.2. Instalar o FFmpeg com 1 comando (via winget)

No Windows 10 ou 11, abra o **PowerShell** (pode ser normal mesmo) e rode:

    winget install "FFmpeg (Essentials Build)"

Depois confirme se deu certo:

    ffmpeg -version

Se aparecer a versão do FFmpeg, está pronto para uso.

---

## 2. Clonar ou atualizar o repositório

### 2.1. Se ainda NÃO tiver o repositório local

No PowerShell ou Prompt:

    git clone https://github.com/dev-txt/vsl.git
    cd vsl

### 2.2. Se JÁ tiver o repositório local

    cd vsl
    git pull origin main

---

## 3. Preparar o novo vídeo

1. Escolha um nome de pasta para a nova VSL, seguindo o padrão:

   - `vsl_001`
   - `vsl_002`
   - `vsl_003`
   - `vsl_004`
   - etc.

2. Copie o arquivo `.mp4` para a **raiz do repositório** (mesma pasta onde está o `player.html`), por exemplo:

   - `meu_video.mp4`

3. Crie a pasta da nova VSL (exemplo: `vsl_004`):

    mkdir vsl_004

---

## 4. Quebrar o vídeo em HLS com FFmpeg (comando em 1 linha)

Ainda na raiz do repositório (onde está `meu_video.mp4`), rode:

    ffmpeg -i meu_video.mp4 -codec:v libx264 -codec:a aac -hls_time 6 -hls_playlist_type vod -hls_segment_filename "vsl_004/segment_%03d.ts" vsl_004/index.m3u8

Esse comando vai:

- Ler o arquivo de entrada `meu_video.mp4`
- Criar a playlist HLS em: `vsl_004/index.m3u8`
- Gerar vários segmentos `.ts`:

  - `vsl_004/segment_000.ts`
  - `vsl_004/segment_001.ts`
  - `vsl_004/segment_002.ts`
  - ...

Confirme depois:

- Dentro de `vsl_004` deve existir:
  - 1 arquivo `index.m3u8`
  - vários arquivos `segment_XXX.ts`

---

## 5. Atualizar o `player.html` para reconhecer a nova VSL

Abra o arquivo `player.html` na raiz do repositório e procure a linha:

    const allowed = ['vsl_001', 'vsl_002', 'vsl_003'];

Inclua a nova VSL na lista. Exemplo, se criou `vsl_004`:

    const allowed = ['vsl_001', 'vsl_002', 'vsl_003', 'vsl_004'];

Salve o arquivo.

---

## 6. Comitar e enviar as mudanças para o GitHub

Na raiz do repositório:

    git add vsl_004
    git add player.html
    git commit -m "Add nova VSL vsl_004"
    git push origin main

O GitHub Pages vai atualizar o site automaticamente após o `push`.

---

## 7. Como usar a nova VSL

### 7.1. Via iframe (usando `player.html`)

A URL base do site é:

    https://dev-txt.github.io/vsl/

A nova VSL ficará acessível em:

    https://dev-txt.github.io/vsl/player.html?v=vsl_004

Exemplo de uso com `<iframe>` em qualquer página HTML:

    <iframe
      src="https://dev-txt.github.io/vsl/player.html?v=vsl_004"
      width="100%"
      height="360"
      frameborder="0"
      allow="autoplay; fullscreen"
      allowfullscreen>
    </iframe>

### 7.2. Via player próprio (embed direto em outro player HLS)

A playlist HLS pública da nova VSL será:

    https://dev-txt.github.io/vsl/vsl_004/index.m3u8

Você pode usar essa URL em qualquer player compatível com HLS (por exemplo, usando `hls.js` em uma página própria).
