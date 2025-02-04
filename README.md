Trabalho realizado para a disciplina Computação em GPU.

O objetivo do código é simular imagens em subaberturas de um sistema de óptica adaptativa e utilizar um método de otimização para estimar o centróide de uma estrela real observada, comparando a posição real com a estimada.

Para isso, o código gera duas imagens: uma da estrela considerando o ruído e outra de referência sem defeitos. 
Em seguida, a imagem de referência sem defeitos percorre uma janela de busca (imagem observada) em pequenos passos, como se estivéssemos explorando uma grade. 
A cada passo, calcula-se uma métrica para avaliar se a posição atual está mais próxima do centróide da estrela.

A aplicação do método de busca exaustiva (grid search) foi implementada em GPU, dividindo o espaço de busca em uma grade e calculando simultaneamente o erro para cada deslocamento. 
Essa abordagem utiliza um modelo estatístico baseado na distribuição de Poisson para calcular a verossimilhança da imagem simulada em relação à imagem observada.

Foram testadas, na GPU, as bibliotecas CuPy e Numba. A aplicação direta em CuPy, sem especificar blocos e threads, funcionou corretamente. 
No entanto, ao tentar dividir o grid em blocos e threads, usando Raw Kernel do cupy, o erro retornava apenas a posição inicial do grid, sugerindo um problema na forma como o cálculo estava sendo realizado. 
Mantive o teste em Raw Kernel para que, em uma correção futura, seja possível identificar o erro.

Também foi testada a biblioteca Numba para esse processo de paralelização, que funcionou conforme o esperado, trazendo pontos de centróide muito próximos à posição real da estrela em um tempo esperado.

*Obs: para fazer com que o código funcione, deverá colocar o arquivo profile.mat junto a pasta. Pois as imagens são simuladas considerando um perfil de sódio.*
