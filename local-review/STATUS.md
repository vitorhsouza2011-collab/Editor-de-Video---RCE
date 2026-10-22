# Primeira revisão — 22/10/2026

Objetivo: preservar as funções atuais, corrigir defeitos e só depois ampliar funções.
Base: jub0t/Concat, commit 797c8c7 (main obtida em 22/10/2026).
Esta base é de desenvolvimento; não é uma reconstrução exata do instalador 0.2.8.

## Alteração preparada (ainda não homologada)

Issue https://github.com/jub0t/Concat/issues/506 relata que nomes longos na
exportação não acompanham as setas do teclado, na versão 0.2.8/macOS.
O componente Field usa TextInput diretamente, sem tratar o movimento do cursor.
A documentação do Slint exige que o componente envolvente cuide dessa rolagem:
https://docs.slint.dev/latest/docs/slint/reference/keyboard-input/textinput/

A alteração em src/crates/concat/ui/primitives/modal.slint adiciona uma área
recortada dentro das margens e deslocamento horizontal para acompanhar o cursor.
Como Field é compartilhado, todos os seus usos precisam de regressão.
Não altera o motor de vídeo, formato de projeto ou lista de funções.

## Verificação realizada

- Inspeção do código e confronto com documentação do Slint.
- git diff --check: aprovado.
- Sem compilação, teste visual ou execução de testes Rust: cargo/rustc não
  disponíveis neste ambiente. Não afirmar que o defeito já foi resolvido em uso.
- Nenhum instalador Windows foi gerado. Alteração registrada no PR #1 do fork do usuário.

## Testes necessários antes de entregar um instalador

1. Compilar com Rust >= 1.93 e dependências nativas descritas em src/README.md.
2. Executar cargo fmt --check, cargo clippy --all-targets -- -D warnings e cargo test.
3. No Windows, abrir exportação e escrever um nome maior que a largura do campo.
4. Navegar para ambos os lados, Home/End, selecionar com Shift e copiar/colar.
5. Apagar o texto, substituir por nome curto e redimensionar a janela.
6. Repetir com acentos, emoji, campos de senha e demais usos de Field.
7. Confirmar que o texto não cobre as margens e que o nome exportado é exato.
8. Abrir, salvar e reabrir projeto; exportar amostra com vídeo e áudio.

## Próximas etapas

- Confirmar o patch no Windows antes de ampliar o lote de correções.
- Triar relatos de travamentos, perda de projeto e falhas de exportação,
  conferindo versão, reprodução e correções já existentes na base.
- Usar nome e ícone próprios para distribuir uma versão modificada,
  conforme TRADEMARK.md; preservar licença e disponibilizar fonte correspondente.
- Preparar compilação Windows com CI em repositório próprio, ou ambiente local.
- Só depois escolher novas funções com o usuário.
