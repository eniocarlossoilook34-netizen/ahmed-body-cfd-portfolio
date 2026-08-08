# Rota Automotiva: Escultura Aerodinâmica

Simulação de CFD (OpenFOAM 12) do corpo de Ahmed (35° slant) com pós-processamento
de streamlines no ParaView, como base para peça de arte conceitual automotiva.

## Pipeline
1. **Geometria**: corpo de Ahmed (ahmed-bluff-body-cfd), STL watertight validado
2. **Malha**: blockMesh + snappyHexMesh (refinamento de superfície + camadas prismáticas
   parciais, wall functions, y+ médio ~87)
3. **Simulação**: simpleFoam (k-omega SST, U=40 m/s, Re≈2.8M)
4. **Resultado**: Cd≈0.55, Cl≈0.59 (nota: acima da literatura ~0.25-0.29 devido a
   cobertura incompleta de camada limite nas pernas de suporte — ver observações)
5. **Pós-processamento**: streamlines coloridas por magnitude de velocidade no ParaView

## Estrutura
- `system/` — dicionários de configuração (blockMesh, snappyHexMesh, fvSchemes, etc.)
- `constant/transportProperties` — propriedades do fluido (ar, ν=1.5e-5 m²/s)
- `constant/turbulenceProperties` — modelo k-omega SST
- `render_final.jpg` — imagem final da peça

## Observações técnicas
Cobertura de camada prismática ficou em ~89% da superfície devido a colisão geométrica
nas pernas de suporte do modelo (estrutura fina, eixo medial reduzido). Ajustes de
parâmetro testados extensivamente; y+ variou entre 0.6-2627 (wall functions fora do
regime válido em regiões pontuais). Resultado é fisicamente qualitativo mas não
quantitativamente preciso — adequado para visualização, não para validação de Cd/Cl.
