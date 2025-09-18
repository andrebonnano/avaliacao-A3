# Sistema de Gestão de Projetos e Equipes (Console, MVC/POO)

O projeto será concebido como uma solução acadêmica e prática para demonstrar a aplicação dos princípios de **Programação Orientada a Objetos (POO)** e do padrão **MVC (Model–View–Controller)** em um sistema simples de gestão.  
O sistema será desenvolvido em **Java (console)**, com código modular e preparado para evoluções (persistência em arquivo/banco, relatórios e UI gráfica).

---

## Objetivo Geral

O sistema será projetado para permitir **criar, listar e remover** entidades de domínio — **Usuários, Projetos e Tarefas** — além de **alterar status** de Projetos e Tarefas.  
Ele servirá como laboratório para consolidar:

- **Herança e Polimorfismo**: bases `Entidade`, `Pessoa`, `ItemTrabalho` e especializações `Usuario`, `Projeto`, `Tarefa`.
- **Interfaces**: contrato `Transicionavel<S>` para padronizar transições de status.
- **Separação de responsabilidades (MVC)**: domínio/serviços desacoplados de I/O.
- **DTOs e Mappers**: apresentação desacoplada do modelo de domínio.
- **Repository** em memória: fácil troca futura por persistência real.

---

## Arquitetura (MVC)

- **Model**  
  Conterá entidades, enums, classes base e **services** com regras de negócio (criar, listar, remover, mudar status).  
  Não conhecerá UI.

- **View (console)**  
  Exibirá menus, coletará entradas via teclado e formatará saídas.  
  Não conterá lógica de negócio.

- **Controller**  
  Orquestrará fluxos entre View e Services, validará entradas e decidirá o que renderizar.

---

## Modelo de Domínio

- **Bases**
    - `Entidade` → identidade (`id`)
    - `Pessoa` → herda de `Entidade` (nome, email)
    - `ItemTrabalho` → herda de `Entidade` (título, responsável) e exporá `tipo()`/`resumo()` polimórficos
- **Interface**
    - `Transicionavel<S extends Enum<S>>` → `getStatus`, `setStatus`, `podeTransicionarPara`
- **Entidades**
    - `Usuario` (perfil)
    - `Projeto` (status, responsável opcional) — herdará de `ItemTrabalho`
    - `Tarefa` (prioridade, status, projetoId, responsávelId) — herdará de `ItemTrabalho`
- **Enums**
    - `Perfil { ADMIN, GESTOR, USUARIO }`
    - `StatusProjeto { PLANEJADO, EM_ANDAMENTO, CONCLUIDO, CANCELADO }`
    - `StatusTarefa { ABERTA, EM_PROGRESSO, CONCLUIDA, BLOQUEADA }`
    - `Prioridade { BAIXA, MEDIA, ALTA }`

---

## Funcionalidades (versão básica)

- **Usuários**: criar, listar, remover.
- **Projetos**: criar, listar, remover, alterar status.
- **Tarefas**: criar, listar, remover, alterar status; vínculo a projeto e responsável; prioridade.

---

## Estrutura de Pastas (proposta)

```
src/
  app/Main.java
  controller/
    Router.java
    UsuarioController.java
    ProjetoController.java
    TarefaController.java
  view/console/
    IO.java
    MenuView.java
    UsuarioView.java
    ProjetoView.java
    TarefaView.java
    Formatters.java
  model/base/
    Entidade.java
    Pessoa.java
    ItemTrabalho.java
  model/polimorfismo/
    Transicionavel.java
  model/dominio/
    Usuario.java
    Projeto.java
    Tarefa.java
  model/enums/
    Perfil.java
    StatusProjeto.java
    StatusTarefa.java
    Prioridade.java
  repository/
    CrudRepository.java
    InMemoryRepository.java
  service/
    DomainException.java
    UsuarioService.java
    ProjetoService.java
    TarefaService.java
  dto/
    UsuarioDTO.java
    ProjetoDTO.java
    TarefaDTO.java
  mapper/
    UsuarioMapper.java
    ProjetoMapper.java
    TarefaMapper.java
```

---

## Requisitos

- **Java 

## Execução (console)

Ao iniciar, o **Router** exibirá o menu principal. O usuário navegará pelos módulos de **Usuários**, **Projetos** e **Tarefas**, utilizando opções para **Criar**, **Listar**, **Remover** e **Alterar Status** (onde aplicável).  
Mensagens de erro previsíveis serão lançadas como `DomainException` e serão tratadas no Controller para informar o usuário.

---

## Decisões de Projeto

- **POO explícita**: herança (`Pessoa`, `ItemTrabalho`) e polimorfismo (`tipo()`, `resumo()`, `Transicionavel`).
- **Camadas desacopladas**: Services não conhecem UI; Views não conhecem repositórios.
- **DTO/Mapper**: View receberá apenas dados necessários, evitando expor o domínio.
- **Repository em memória**: viabilizará testes rápidos e evolução posterior sem quebrar as camadas.

---

## Evoluções Futuras

- Persistência em **arquivo** (CSV/JSON) ou **banco de dados**, trocando apenas a implementação de `Repository`.
- **Relatórios** (tarefas por projeto/responsável; produtividade).
- Regras de **workflow** mais rígidas (ex.: proibir pular estados).
- UI **gráfica** (Swing/JavaFX) ou API **REST** mantendo o mesmo domínio/serviços.

---

## Licença

Este projeto será distribuído para fins educacionais. Uma licença aberta (MIT/Apache-2.0) poderá ser adotada conforme necessidade do curso/trabalho.
