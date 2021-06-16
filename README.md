# first-and-follow-vue

### Antes de tudo é necessário possuir o Yarn instalado para rodar o projeto

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```


### 1. Após o projeto iniciar a seguinte tela será mostrada:
![image](https://user-images.githubusercontent.com/49729380/122299018-d44a6a00-ced3-11eb-8996-90ac6c7162fc.png)

### 2. O botão 'Adicione uma regra de produção' serve para adicionar as regras de produção da gramática. Ao clicar nele a tela muda para esta: 
![image](https://user-images.githubusercontent.com/49729380/122299436-5f2b6480-ced4-11eb-8364-3b4ffbc9fc90.png)

### 3. Para adicionar uma regra de produção basta colocar o nome da variável (que não pode ser vazio) e clicar no botão mais mostrará a parte de adição das regras de produção, nesta forma:
![image](https://user-images.githubusercontent.com/49729380/122300094-3a83bc80-ced5-11eb-8c8b-f15dcd977108.png)
#### Para adicionar uma regra de produção preencha o campo de texto com a regra.
  - Para adicionar outra regra a mesma variável atual, o botão de mais, abaixo do campo de texto, deve ser clicado, revelando um novo campo para entrada da nova regra;
  - Para adicionar a variável com suas regras, clique no botão 'Salvar regra de produção';
  - O botão com 'X' cancela a adição de uma nova regr. Já o 'X' rodeado com um círculo, exclui uma das regras de produção da variável.

### 4. Após adicionar um nova variável com sua(s) regra(s) de produção, a mesma poderá ser selecionada como variável inicial no seletor que fica abaixo do botão que adiciona as regras de produção (2ª).
![image](https://user-images.githubusercontent.com/49729380/122302301-48870c80-ced8-11eb-8e07-5e3b06ca0fd3.png)

### 5. Ao selecionar uma variável inicial, o botão 'Preencher primeiros e seguintes' irá fornecer os primeiros e seguintes da variável selecionada anteriormente, resultando em algo parecido com a imagem:
![image](https://user-images.githubusercontent.com/49729380/122303278-b1bb4f80-ced9-11eb-96e1-f74f4bbbbfe3.png)


### Portanto, para o melhor funcionamento do programa siga os passos acima com as seguintes observações:
#### 1. Variáveis são letras maiúsculas, já os símbolos terminais, caracteres mínúsculos.
#### 2. Variáveis e símbolos terminais são lidos apenas caractere a caractere, ou seja, uma variável E' teria de ser chamada E ou outra letra maiúscula. A mesma regra aplica-se as variáveis, lembrando da observação 1.
#### 3. O epsilon é representado por uma string vazia, literalmente uma stringa vazia. Caso queira colocar o epsilon em uma entrada, apenas as strings do tipo espaço vazio (' '), serão corretamente lidas. Uma string vazia dentro do campo de entrada pode ser colocada ao pressionar a tecla espaço entre qualquer caractere, ou mesmo sozinha.
#### 4. Utilize gramáticas em que, caso regras de produção contenham letras maiúsculas (observação 1), suas regras de produção estejam explicitas dentro do conjunto de regras de produção.
