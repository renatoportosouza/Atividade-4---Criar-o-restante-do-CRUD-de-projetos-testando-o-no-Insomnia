#Atividade de Desenvolvimento de API FullStack SENAI

## Visão Geral
Este projeto tem como objetivo a implementação do restante do CRUD para a API ExoApi. Você irá desenvolver os métodos necessários nas classes `ProjetosController.cs` e `ProjetoRepository.cs` para completar as operações de **Create, Read, Update e Delete** de projetos. Os testes serão realizados utilizando o Insomnia ou Postman.

## Requisitos
Antes de iniciar, garanta que os seguintes softwares estão instalados:

- **Visual Studio Code**: [Download](https://code.visualstudio.com)
- **.NET SDK (versão 6 ou 7)**: [Download](https://dotnet.microsoft.com/en-us/download)
- **SQL Server Management Studio (SSMS)**: Para gerenciar o banco de dados
- **Insomnia ou Postman**: Para testes da API ([Baixe o Insomnia aqui](https://insomnia.rest/download))

## Passo a Passo

### 1. Configuração do Banco de Dados
1. Abra o **SQL Server Management Studio (SSMS)**.
2. Clique em **Arquivo > Abrir Arquivo...** e selecione o script `cria-db.sql` dentro da pasta fornecida na atividade.
3. Com o arquivo aberto, clique em **Executar** para criar o banco de dados e as tabelas.
   - Caso já exista um banco e precise recriá-lo, apague o banco antigo antes de executar o script novamente.

### 2. Preparação do Ambiente no VS Code
1. Extraia o arquivo **ATIVIDADE-04.zip** disponível no AVA.
2. Abra a pasta `Exo.WebApi` e clique com o botão direito para abrir o terminal Git Bash.
3. Execute o comando abaixo para verificar a versão do .NET instalada:
   ```sh
   dotnet --version
   ```
   - Certifique-se de que a versão seja **6 ou superior**.
4. Abra o projeto no Visual Studio Code executando:
   ```sh
   code .
   ```

### 3. Implementação do CRUD
Abra e edite os seguintes arquivos para adicionar os métodos necessários:

#### **ProjetoRepository.cs**
Adicione os seguintes métodos à classe `ProjetoRepository`:
```csharp
public void Cadastrar(Projeto projeto) {
    _context.Projetos.Add(projeto);
    _context.SaveChanges();
}

public Projeto BuscarporId(int id) {
    return _context.Projetos.Find(id);
}

public void Atualizar(int id, Projeto projeto) {
    Projeto projetoBuscado = _context.Projetos.Find(id);
    if (projetoBuscado != null) {
        projetoBuscado.NomeDoProjeto = projeto.NomeDoProjeto;
        projetoBuscado.Area = projeto.Area;
        projetoBuscado.Status = projeto.Status;
        _context.Projetos.Update(projetoBuscado);
        _context.SaveChanges();
    }
}

public void Deletar(int id) {
    Projeto projetoBuscado = _context.Projetos.Find(id);
    if (projetoBuscado != null) {
        _context.Projetos.Remove(projetoBuscado);
        _context.SaveChanges();
    }
}
```

#### **ProjetosController.cs**
Adicione os métodos correspondentes na classe `ProjetosController`:
```csharp
[HttpPost]
public IActionResult Cadastrar(Projeto projeto) {
    _projetoRepository.Cadastrar(projeto);
    return StatusCode(201);
}

[HttpGet("{id}")]
public IActionResult BuscarPorId(int id) {
    Projeto projeto = _projetoRepository.BuscarporId(id);
    return projeto != null ? Ok(projeto) : NotFound();
}

[HttpPut("{id}")]
public IActionResult Atualizar(int id, Projeto projeto) {
    _projetoRepository.Atualizar(id, projeto);
    return StatusCode(204);
}

[HttpDelete("{id}")]
public IActionResult Deletar(int id) {
    try {
        _projetoRepository.Deletar(id);
        return StatusCode(204);
    } catch (Exception) {
        return BadRequest();
    }
}
```

### 4. Execução da API
1. No terminal, execute:
   ```sh
   dotnet restore  # Restaura os pacotes
   dotnet build    # Compila a aplicação
   dotnet run      # Executa o servidor
   ```
2. Copie o endereço da API exibido no terminal para uso nos testes.

### 5. Testes no Insomnia/Postman

#### **Listar Projetos (GET)**
- **URL**: `https://localhost:7154/api/projetos`
- **Método**: GET

#### **Cadastrar Projeto (POST)**
- **URL**: `https://localhost:7154/api/projetos`
- **Método**: POST
- **Body (JSON)**:
```json
{
  "nomeDoProjeto": "Projeto X",
  "area": "Tecnologia",
  "status": true
}
```
- **Resposta esperada**: Status **201 Created**

#### **Buscar Projeto por ID (GET)**
- **URL**: `https://localhost:7154/api/projetos/1`
- **Método**: GET
- **Resposta esperada**: Status **200 OK** com os dados do projeto

#### **Atualizar Projeto (PUT)**
- **URL**: `https://localhost:7154/api/projetos/1`
- **Método**: PUT
- **Body (JSON)**:
```json
{
  "nomeDoProjeto": "Projeto X - Atualizado",
  "area": "Educação",
  "status": false
}
```
- **Resposta esperada**: Status **204 No Content**

#### **Deletar Projeto (DELETE)**
- **URL**: `https://localhost:7154/api/projetos/1`
- **Método**: DELETE
- **Resposta esperada**: Status **204 No Content**

## Conclusão
Com este guia, você poderá implementar e testar o CRUD da API ExoApi com sucesso. Certifique-se de seguir cada etapa corretamente e utilizar o Insomnia/Postman para validar suas requisições. Caso encontre problemas, verifique os logs no terminal e revise seu código.

