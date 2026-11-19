# Trucker Cap Stems v0.3 — música inteira, seleção rápida e MP3
Separação local real com Demucs/ONNX. Sem API paga, sem backend, sem upload do áudio a servidor. Modelo aberto existente; não é IA treinada do zero ou exclusiva Trucker Cap.

## GitHub e Vercel
Envie todo este projeto ao GitHub. Na Vercel: Root Directory ./; framework Other; install npm ci --ignore-scripts; build npm run build; output dist. Não selecione dist como Root Directory. Runtime vendorizado, sem chaves. Modelo de aproximadamente181MB baixado após o usuário tocar Preparar IA; não está no ZIP/repositório.

## Uso
MP3/WAV. Música inteira selecionada por padrão. Também há teste30s,1min,2min e trecho personalizado de duração livre. Sem teto de duração ou tamanho de arquivo imposto pelo aplicativo. A capacidade real é limitada por RAM, decodificador, navegador e tempo de processamento. Arquivos grandes podem não abrir em celulares. Não prometa processamento infinito ou compatibilidade total.

IA por blocos24s com sobreposição2s e normalização da junção. Áudio de entrada é decodificado por inteiro; os quatro resultados também permanecem em RAM para player/exports. O motor recebe somente um bloco por vez; saída é acumulada diretamente nos AudioBuffers para evitar uma cópia integral adicional. Worker CPU/WASM de1thread, sem exigir COOP/COEP. Esta versão não usa WebGPU.

Voz,bateria,baixo,outros. Solo,mute,volume,player sincronizado,seek,download MP3 ou WAV de cada faixa e mix. Outros reúne guitarras/teclas/demais instrumentos. Sem separação independente de guitarra/teclado. Nova separação substitui o mixer anterior; baixe antes. Resultados não salvos automaticamente. WAV padrão tem limite de tamanho próprio (4GiB por arquivo); o app informa erro na exportação se excedido.

## Modelo e off-line
https://huggingface.co/timcsy/demucs-web-onnx/resolve/main/htdemucs_embedded.onnx
SHA256 e5e425c17683f163a472462eb5f5a4ffcd11c31858d57fbd0833b012d8b88077
Download somente mediante ação; validação de hash. Pode fornecer o mesmo arquivo .onnx local. Modelo em IndexedDB; shell/JS/WASM em Service Worker. O navegador pode remover cache. Valide primeira preparação e reabertura em modo avião antes de prometer off-line. Atualização não troca a identidade do banco nem remove modelo salvo.

## Validação
Testes de WAV,soma/solo/mute,limites de trecho e duração completa; janelas cobrem todas as amostras,sem lacunas ou duplicação,com normalização da sobreposição. Teste do modelo real em runtime WASM no Node com35s de sinal sintético,mais de um bloco,e quatro saídas finitas na duração exata. Não valida qualidade em música real, exibição, velocidade em celulares ou compatibilidade Android/iPhone/iPad. Compatibilidade física/navegadores reais ainda em teste.

Créditos/licenças: dist/LICENSES.txt. Projetos vendorizados MIT. Modelo HTDemucs convertido por timcsy. Avalie a distribuição comercial final antes de lançar.

## v0.3: abertura e MP3
Selecionar arquivo usa URL local e metadados do elemento Audio; não chama arrayBuffer/decodeAudioData nessa etapa. A decodificação completa fica para Separar ou Converter. Isso torna a seleção mais leve, mas não elimina o tempo de decodificação nem acelera por si só o modelo de IA. Arquivos grandes ainda podem demorar ou falhar por memória.
Exportação padrão MP3, com128/192/256/320kbps; WAV opcional. MP3 de cada faixa, mix com solo/mute/volume capturados no início da exportação e conversão do original sem preparar IA. Encoder em worker separado, alimentado por blocos e sem desanexar/corromper os buffers do player. Cancelar exportação preserva o motor de IA carregado. Resultados de MP3 ficam em memória até o download; não garantido para duração infinita.
Biblioteca LAME/lamejs1.2.1 não modificada, carregada separadamente, sob LGPL-3.0. Fonte distribuída em dist/vendor/lame/lamejs-1.2.1-source.tgz e referência https://github.com/zhuker/lamejs e https://lame.sourceforge.net. Arquivo substituível por versão compatível para relink. A interface não impede modificação/depuração dessa biblioteca.
Teste real do encoder: sinal estéreo1s em blocos,192kbps, inspecionado com FFprobe: MP3/44100Hz/2canais/192000bps; decodificação FFmpeg sem erros. Não é validação em aparelho físico.

## v0.4 — processamento
Modo Rápido padrão: 10% de sobreposição, blocos alinhados a três inferências; Qualidade: 25%. O motor deixa de executar segmentos extras depois de cobrir todo o áudio. Não há redução da duração nem da taxa de amostragem. Menos sobreposição pode alterar a qualidade nas transições. Vercel inclui COOP/COEP para habilitar até quatro threads quando crossOriginIsolated; outros hosts podem permanecer em uma thread. Velocidade depende do aparelho. Sem promessa de tempo real. Guitarra/piano separados ainda não integrados nesta versão.

## v0.5 — Trucker Cap Separador de Faixas
Identidade visual verde com o logotipo fornecido. Pan L/R por faixa na reprodução e na mixagem MP3/WAV. Faixas individuais continuam originais. A mixagem normaliza picos acima de 1 para evitar clipping.

Click opcional: a bateria separada é analisada localmente após cada separação. O estimador usa envelope de transientes, autocorrelação (60–180 BPM) e acompanhamento de picos locais. O resultado estima BPM e posições das batidas; não identifica automaticamente compasso/primeiro tempo e não garante acerto para rubato, mudanças bruscas, síncopes ou grooves ambíguos. Não é um modelo neural de beat tracking. Ajustar BPM/alinhamento ou usar metade/dobro troca para grade fixa; Reconhecer beat restaura análise automática. Reprodução usa relógio Web Audio; pausar/buscar recalcula a posição do click.

Ouvir click controla monitoração. Incluir click ao baixar a mixagem controla exportação independentemente. Pan e volume próprios do click, inclusive saída só à esquerda ou só à direita. O processamento permanece local e sem API paga. Nenhuma dependência externa foi adicionada.

Validação: testes DSP de pan, click entre blocos, detecção de BPM/alinhamento em sinais com referência conhecida, ciclo do detector/worker e agendamento de reprodução; MP3 real com LAME e testes anteriores. Não houve teste em navegador/dispositivo físico nesta revisão. Músicas reais precisam de conferência auditiva.
