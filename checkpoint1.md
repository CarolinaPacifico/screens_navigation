# Navegação entre Telas - Kotlin

## Descrição
O projeto consiste no desenvolvimento de um aplicativo utilizando Jetpack Compose, com foco na implementação de navegação entre telas e passagem de parâmetros. A aplicação simula um fluxo simples de uso com telas como Menu, Perfil e Pedidos, permitindo demonstrar diferentes formas de comunicação entre componentes.

## Objetivo do Checkpoint
A avaliação tem como objetivo verificar a capacidade do aluno de evoluir um projeto já existente, aplicando conceitos de navegação entre telas no Android por meio do Jetpack Compose Navigation, com ênfase na passagem de parâmetros.

O aluno deve demonstrar domínio técnico e compreensão do código previamente desenvolvido, sendo capaz de ampliá-lo de forma consistente e bem documentada.

---
### Commit 1 — Passagem de parâmetros obrigatórios na tela de Perfil
Neste commit, foi implementado a passagem de parâmetros obrigatórios na navegação para a tela de Perfil. 
Antes dessa alteração, a tela de Perfil era acessada de forma genérica, sem receber nenhuma informação específica do usuário. 
Com essa mudança, a navegação passou a exigir o envio de dados, garantindo que a tela sempre seja carregada com informações dinâmicas.

#### Código Alterado:
- Adição do parâmetro: nome - PerfilScreen.kt
- Enviar um nome como parâmetro na rota - MenuScreen.kt
```python
onClick = { navController.navigate("perfil/Fulano de Tal") },
```
- Definição da rota com parâmetro, utilizando o valor enviado na navegação e mostrando na tela de Perfil - MainActivity.kt
```python
composable(route = "perfil/{nome}") {
  val nome: String? = it.arguments?.getString("nome", "Usuário Genérico")
  PerfilScreen(modifier = Modifier.padding(innerPadding), navController, nome!!)
```










---

### Commit 2 — Passagem de parâmetros opcionais na tela de Pedidos
Neste commit foi implementada a passagem de um parâmetro opcional para a tela de Pedidos. 
Diferente da tela de Perfil, onde o envio de dados é obrigatório, aqui a proposta foi permitir que a tela funcione o sem o recebimento desse parâmetro, 
utilizando um valor padrão sempre que nenhuma informação for fornecida.

#### Código Alterado:
- Definir que o parâmetro cliente é uma string opcional, podendo ter um valor ou ser nulo - PedidosScreen.kt
```python
cliente: String?
```
- Exibir o cliente recebido como parâmetro junto com o título - PedidosScreen.kt
```python
text = "PEDIDOS - $cliente",
```
- Definir uma rota com um parâmetro opcional cliente, utilizando um valor padrão para garantir que a tela de Pedidos funcione corretamente.
```python
composable(
  route = "pedidos?cliente={cliente}",
  arguments = listOf(navArgument("cliente") {
  defaultValue = "Cliente Genérico"
})
)
```
- Chamar a tela de Pedidos e passar para ela o valor do parâmetro cliente, que foi recebido da navegação.
```python
PedidosScreen(modifier = Modifier.padding(innerPadding), navController, it.arguments?.getString("cliente"))
)
```
---











### Commit 3 — Inserindo valor ao parâmetro opcional na tela de Pedidos
Esse commit complementa a implementação anterior, mostrando como um parâmetro opcional pode ser utilizado.

- Enviar "cliente" como parâmetro opcional na rota. - MenuScreen.kt
```python
onClick = { navController.navigate("pedidos?cliente=Cliente Exemplo") },
```













---
### Commit 4 — Passagem de múltiplos parâmetros
Neste commit foi implementado a passagem de múltiplos parâmetros na navegação, permitindo que mais de uma informação seja enviada para a mesma tela. 
Além do parâmetro nome, já utilizado anteriormente, foi adicionado o parâmetro idade.

- Adição do parâmetro: idade - PerfilScreen.kt
- Insere o nome e a idade recebidos para exibir uma mensagem personalizada - PerfilScreen.kt
```python
text = "PERFIL - $nome tem $idade anos",
```
- Enviar nome e idade como parâmetro rota. - MenuScreen.kt
```python
onClick = { navController.navigate("perfil/Carol/20") },
```
- Definir uma rota que exige nome e idade, especificar o tipo de cada um desses parâmetros e
prepara o sistema para extrair e converter corretamente esses valores quando a tela de Perfil for acessada. - MainActivity.kt
```python
composable(
  route = "perfil/{nome}/{idade}",
  arguments = listOf(
  navArgument("nome") { type = NavType.StringType },
  navArgument("idade") { type = NavType.IntType }
)
) {
```
- Pegar o valor da idade enviado na navegação e, caso ele não exista, usa 0 como valor padrão - MainActivity.kt
```python
val idade: Int? = it.arguments?.getInt("idade", 0)
```
- Forçar a variável idade a ser tratada como não nula, permitindo que ela seja passada corretamente para a tela de Perfil, já que o valor padrão garante que nunca será nulo.
```python
PerfilScreen(
  modifier = Modifier.padding(innerPadding),
  navController,
  nome!!,
  idade!!
)
```









